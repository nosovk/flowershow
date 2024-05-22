# Цель:
убрать дублирование аналитики, перевести аналитику на GTM.

## Код аналитики
Сам код GTM:
```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-NSG2B5W');</script>
<!-- End Google Tag Manager -->
```

Код аналитики следует запускать асинхронно, то есть - в коде страницы, до установки GTM  должен быть только сниппет вида:
```html
<script> // важно, тут НЕ должно быть type=module
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
      'gtm.start': new Date().getTime(),
      event: 'gtm.js'
    });
</script>
```
этот снипет должен быть вставлен синхронно в head файла. Он синхронный и ничего не заружает, поэтому не создаст проблем со скоростью загрузки, но благодаря нему всевозможные события аналитик не будет падать.

Код установки самого скрипта желательно отложить на 3 секунды
```html
<script type="module">
function setUpScript() {
    if (!PUBLIC_GTM_TAG_ID) return;
    let gtmScript = document.createElement('script');
    gtmScript.defer = true;
    gtmScript.src = `https://www.googletagmanager.com/gtm.js?id=GTM-NSG2B5W&l=dataLayer`;
    document.head.appendChild(gtmScript);
  }
  setTimeout(setUpScript, 3000);
</script>
```

Все скрипты приведены как референс, их можно адаптировать под себя. Важен принцип.
Мы при загрузке приложения синхронно инициализируем dataLayer, после этого отложено вызываем код инициализации GTM.

Существующее кода GTag и GTM следует удалить.
## События
### Loaded
Это событие вызывается после получения инфорации о том, залогинен ли пользователь.
Для этого на странице необходимо вставить код:
```html
<script>
window.dataLayer.push({
'event' : 'data-loaded',
'user_id' : '12345', // это userid пользователя, должен быть уникальным для каждого пользователя. Если пользователь не залогинен содержит ''
'userLang' : 'en', // текущий язык сайта.
});
</script>
```

## CTA-buy
событие при нажатии на кнопку купить
```javascript
window.dataLayer.push({
'event' : 'cta-buy',
'event-point': 'cdc2edvf' // это event point id, который человек захотел купить
});
```


## Login Success
событие при успешном логине
```javascript
window.dataLayer.push({
'event' : 'login-success',
'user_id': 'cdc2edvf'
});
```

## Login Fail
событие при неуспешном логине
```javascript
window.dataLayer.push({
'event' : 'login-fail',
});
```