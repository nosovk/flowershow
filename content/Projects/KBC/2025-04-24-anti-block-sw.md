Goal: улучшить ретеншен игроков, в случае если наши зеркала забанены.

Problem: люди сохранят в закладках забаненые зеркала, потом пробуют на них прийти - а там страница блока.

Solution: мы можем сделать service worker, который бы проверял возможность загрузить favicon, и если он не загружается - кидал нас на редиректор.

То есть, при первой успешной загрузке сайта мы добавляем в браузер пользователя ServiceWorker. Отличительной особенностью этой технологии является то, что он сохраняется в браузере пользователя и может работать даже оффлайн. То есть, когда человек через неделю зайдет на страницу с установленным сервис воркером, даже если страница недоступна, воркер будет запущен. Именно этим свойством мы и хотим воспользоваться. Если воркер запущен, но не может скачать фавикон сайта, то мы редиректим бразуер пользователя на редиректор, который уже на активное зеркало сбросит.

Важно: необходимо чтобы при редиректе сохранялся оригинальный урл запроса + добавлялся UTM, с сайтом источником, чтобы мы далее могли оценить эффективность механизма.

Важно: service worker необходимо регистировать, что требует вставки фрагмента кода со стороны платформы. 

Пример кода регистрации:
```
`navigator.serviceWorker.register('/sw.js', { scope: '/' });`
```

Пример кода воркера:
```javascript
//----------EXAMPLE OF ANTI-BLOCK REDIRECT FUNCTIONALITY FRONTEND-2992 ----------

self.addEventListener('install', (event) => {
    // console.log('self.skipWaiting();');
    self.skipWaiting();
});

self.addEventListener('fetch', (event) => {
    const clientDomainRegexp = /^https:\/\/(www\.)?gslot\.com((\/[a-zA-Z]{2}-[a-zA-Z]{2})|(\/[a-zA-Z]{2}))?\/?$/;
    const imageUrl = `https://www.gslot.com/resources/images/favicon.png?nocache=${new Date().getTime()}`;
    const redirectUrl = 'https://gslot3.com?utm_source=https://gslot.com';

    if (event.request.method === 'GET' && clientDomainRegexp.test(event.request.url)) {
        event.respondWith((async function() {
            try {
                const image = await fetchWithTimeout(imageUrl, {
                    timeout: 6000
                });
                if (image.status !== 200) {
                    // console.log('image.status  !== 200');
                    return Response.redirect(redirectUrl);
                }
            } catch (err) {
                // console.log('CATCH redirectUrl');
                return Response.redirect(redirectUrl);
            }

            return fetch(event.request);
        })());
    }
});


async function fetchWithTimeout(resource, options = {}) {
    const {timeout = 8000} = options;

    const controller = new AbortController();
    const id = setTimeout(() => controller.abort(), timeout);
    const response = await fetch(resource, {
        ...options,
        signal: controller.signal
    });
    clearTimeout(id);
    return response;
}
 */
```

Мы можем вставить /sw.js так же, как мы robots вставляем внутрь всех зеркал. Но вот код регистрации нужно будет попросить СС добавить.



## другой пример
https://tombriches.com/spoofer+offline.js