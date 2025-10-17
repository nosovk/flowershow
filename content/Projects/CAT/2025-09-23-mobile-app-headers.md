# Goal: обозначить специальные настройки необходимые для запуска доменов для моб приложений

### Мобильные APK
- https://shadowroute.co/kent
- https://shadowroute.co/cat
- https://shadowroute.co/arkada
- https://shadowroute.co/daddy
- https://shadowroute.co/kometa
- https://shadowroute.co/gamma
- https://shadowroute.co/r7
- https://shadowroute.co/wagibet
- https://shadowroute.co/motor

APK разработаны в рамках задачи:
[2025-08-04-CAT-Mobile-app](./2025-08-04-CAT-Mobile-app), которая включает в себя раздел "блокировка не мобильных пользователей".

По сути дела, нам необходимо иметь специальные зеркала, на которые нельзя зайти через веб браузер, но при этом эти зеркала должны успешно открываться в мобильном приложении. Такой хак должен защитить нас от блокировок этих зеркал.

Для реализации блокировки мы в итоге пришли к использованию cookies, как условия для фильтра. Мобильное приложение устанавливает специальный cookies, с именем 'origin_override', в значение которого указывается основной oridgin продукта.

Пример правила на CF:
![mobile-app-domain-protection-1.png](./attachments/mobile-app-domain-protection-1.png)

Важно: мы ни в коем случае не должны блокировать доступ к `/.well-known`, т.к. сломается перехват ссылок из писем, если ОС (без cookies) не может получить доступ к искомому файлу
![mobile-app-domain-protection-2.png](./attachments/mobile-app-domain-protection-2.png)
Документ про перехват ссылок: [2025-09-23-asset-links-for-mobile](./2025-09-23-asset-links-for-mobile)

## Fastly
Аналогичные настройки можно выполнить и в fastly, используя следующий VCL:

Placement: recv (vcl_recv)
```
if (
   (req.http.Cookie:origin_override != "https://r7.casino") && !(req.url.path ~ "^/.well-known")  
) {
   error 618 "mobile block, nothing is here";
}
```