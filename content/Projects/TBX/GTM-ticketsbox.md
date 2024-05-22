# Цель:
убрать дублирование аналитики, перевести аналитику на GTM.

## Код аналитики
```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-NSG2B5W');</script>
<!-- End Google Tag Manager -->
```
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
});
```
