# Goal: иметь возможность хостить треккер аффилки в своей инфраструктуре

Причина: из за блокировок в РУ мы частично покидаем CloudFlare, и начинаем использовать разные другие решения для раздачи контента.

Проблема: аффилка прибита гвоздями к CloudFlare

Предлагаемое решение: проксироать запросы к аффилке через свой микро сервер. Для этого нам необходимо договорится, в каком заголовке передавать IP адрес клиента.

Если мы просто будем проксировать запрос, то в логах аффилки будет фигурироать ип нашего сервера, вместо реального ip  пользователя, что поломает отчетность по гео и т.п.

На данный момент у нас уже есть прототип на cloudflare workers:
```typescript
export default async (  
  request: Request,  
  env: Env,  
  ctx: ExecutionContext,  
): Promise<Response> => {  
  const rURL = new URL(request.url);  
  try {  
    const paths = rURL.pathname.split("/").filter(Boolean);  
    const affId = paths.at(-1);  
  
    const affReq = await fetch(env.AFFILKA_DOMAIN + affId, {  
      headers: request.headers,  
      redirect: "manual",  
    });  
  
    const originalRedirectURL = affReq.headers.get("location");  
    
    const redirectURL = new URL(originalRedirectURL);  
  
    for (const [key, value] of rURL.searchParams.entries()) {  
      if (!redirectURL.searchParams.has(key)) {  
        redirectURL.searchParams.append(key, value);  
      }  
    }  
  
    const finalRedirectUrl = redirectURL.toString();  
  
    const headers = {  
      Location: finalRedirectUrl,  
    };  
  
    const response = new Response(null, {  
      status: 302,  
      headers,  
    });  
    return response;  
  } catch (err) {  
    console.log(err, err.message);  
    return Response.redirect(rURL.origin, 302);  
  }  
};
```

Прототип корректно передает useragent, и работает в специфических сценариях (где по требованиям регулятора нам пришлось внести треккинг домен прямо внутрь продукта, к примеру: https://ganamax.mx/aff/s085cda74). Но не при этом мы не используем его для CIS  продуктов, потому что он ломает статистику внутри affilka.

Предполагаемое решение: добавить возможность передавать real user ip в специальном заголовке. К примеру: `affilka-real-ip=8.8.8.8` Тогда и статистика будет работать, и редиректить будет согласно настроек аффилки и использовать мы сможем такое решене не только на CF, но и на других платфомрах (как-то Fastly или CCDN).
