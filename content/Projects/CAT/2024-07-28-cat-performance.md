# Goal: понять почему упал перформанс
Замер от Июня: 06 месяц - https://gtmetrix.com/reports/www.catcasino.com/Vi5aPK7e/ (DE)
Замер от Июля: 07 месяц - https://gtmetrix.com/reports/www.catcasino.com/822KOQZE/ (Sweden)
Замер на сейчас - почти август - https://gtmetrix.com/reports/www.catcasino.com/7tjU8PYx/ (Sweden)

Гипотеза: на сайте были произведены изменения, которые привели к ухудшению метрик.

### Поведение loader
Судя по всему на сайте изменилось поведение loader, что привело к ухудшению показателей Cumulative Layout Shift и Time to Interactive. Возмжно событие на которое опирается скрытие лоадера было изменено.


![[CAT-performance-dynamic.png]]

### improve Layout Shift metric
![[Pasted image 20240729001016.png]]
Установка высоты блока со слайдером вместо 100% в фиксированное значение - 400px улучшит метрику layout shift - экран не будет прыгать при догрузке банера.

### improve CORS 
SVG и прочие картинки загружаются с CORS, проставление ```
crossorigin="anonymous" могло бы улучшить ситуацию
[doc](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/crossorigin)