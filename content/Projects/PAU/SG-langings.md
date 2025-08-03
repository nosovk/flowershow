Goal: реализовать возможность делать кастомные лендинги на платформе SoftGaming, регистрация на которых должна вести к авторизации на продукте.

## Pre-reqirements
Для реализации необходимо согласовать несколько изменений со стороны SG:
- перейти на использование JWT на продукте (SG продукты могут использовать сессионную или JWT авторизацию. Для нашего флоу необходимо чтобы useJWT=1 было как на продукте, так и на апи лендингов)
- необходимо заказать у СГ отдельное зеркало, на служебном домене. К примеру sever-shadow.com, на котором оно будет размещено за НАШИМ fastly. Это зеркало должно отличаться от остальных, тем что на нем не должно быть сайта. Только API.
- Необходимо изменить настройки отправки почтовых уведомлений со стороны СГ так, чтобы во всех письмах использовался домен редиректора. То есть при получении письма о подтверждении регистрации внутри должна быть ссылка вида: server-redirector.com/confirm=token
- Поскольку мы не владеем доменами продукта, нам критически важно уточнить что у SG будет отключен функционал hotlink  protection для https://sever.casino/favicon/favicon-16x16.png на оригинальном продукте и на всех зеркалах. При активном [hotlink protection](https://developers.cloudflare.com/waf/tools/scrape-shield/hotlink-protection/) не будет работать редиректор.
## Flow
Для понимания итогового флоу регистрации пользователя воспользуемся диаграммой:
[![](https://mermaid.ink/img/pako:eNptU1tv2jAU_itHftpEiiBQIHnohLYJbWLSBEWTprycJibxSOzMdmAU8d97HENhgkhRYn-Xc7MPLFUZZzEz_G_DZcq_CMw1VonExirZVC9cJxLowdQqDStAAz9L3J-3a9RWpKJGaWHqwOl6LcoNwrPGdHOPNXesOcrsFoLlzIHL2R1o4ZAFz4TmLhNieNIKHp6eYBrDVhhhDaCLj5ZDKeTGU6YtZRVDrdVWpDwDbFGwCkpKRMgcOmAs5teW8xhUzWXLAPeSbwm24JChRfhQoSgDytEY4DbtfrzSdqiEGCrccNhiKTKXjnYNNhaaWsnWwfOpYhI8uOz2qtHeWxivC0iVC2M1WkEqLEu149n9QJ7J9TnQPXtAfSFysi_o46K50Aa-_3qG_8ypBd-gwC2nhv3Z2QB2BVrXtUx98sT5u_1MOUC7AaXcD-hitLjF_QQ6ztdTFxcr4SKmtsESKqH1HfJV9e2QPO00Dqrjpno6zoXS4pVDY6hJL3vHYgHLtchYbHXDA1ZxTUOlJTs4fcJo2BVPWEy_GepNwhJ5JA0dyN9KVWeZVk1esHiNpaFVU7txn67R-67mMuP6s2qkZXG_9zhoXVh8YP9o3Y-6_f5kMhhNonHYfxwQumfxaNQdRpPheDgMaTMcjY8Be23j9rrjKAoHYa836o3DKJoEzF3X5V6m56Soz9TkH_5yt3f8nNrXFjlldnwDeFpBfQ?type=png)](https://mermaid.live/edit#pako:eNptU1tv2jAU_itHftpEiiBQIHnohLYJbWLSBEWTprycJibxSOzMdmAU8d97HENhgkhRYn-Xc7MPLFUZZzEz_G_DZcq_CMw1VonExirZVC9cJxLowdQqDStAAz9L3J-3a9RWpKJGaWHqwOl6LcoNwrPGdHOPNXesOcrsFoLlzIHL2R1o4ZAFz4TmLhNieNIKHp6eYBrDVhhhDaCLj5ZDKeTGU6YtZRVDrdVWpDwDbFGwCkpKRMgcOmAs5teW8xhUzWXLAPeSbwm24JChRfhQoSgDytEY4DbtfrzSdqiEGCrccNhiKTKXjnYNNhaaWsnWwfOpYhI8uOz2qtHeWxivC0iVC2M1WkEqLEu149n9QJ7J9TnQPXtAfSFysi_o46K50Aa-_3qG_8ypBd-gwC2nhv3Z2QB2BVrXtUx98sT5u_1MOUC7AaXcD-hitLjF_QQ6ztdTFxcr4SKmtsESKqH1HfJV9e2QPO00Dqrjpno6zoXS4pVDY6hJL3vHYgHLtchYbHXDA1ZxTUOlJTs4fcJo2BVPWEy_GepNwhJ5JA0dyN9KVWeZVk1esHiNpaFVU7txn67R-67mMuP6s2qkZXG_9zhoXVh8YP9o3Y-6_f5kMhhNonHYfxwQumfxaNQdRpPheDgMaTMcjY8Be23j9rrjKAoHYa836o3DKJoEzF3X5V6m56Soz9TkH_5yt3f8nNrXFjlldnwDeFpBfQ)
Где:
- **Affilka tracker** (1,2): ссылка на трактор аффилки. Нам их нужно много, на разных доменах (важно не пересекать сеошные и медиабай ссылки)
- **Land** (3,4,5,6,7,8): лендинг интегрированный с апи СГ. Сам по себе лендинг это статичный сайт, мы его можем опубликовать на CloudFlare и отдавать используя Fastly (чтобы избежать проблем с доступностью в РУ). Нам потребуется сразу несколько доменов для лендингов, т.к. их периодически будут блокировать.
- **SG** (4,5,6,7): это апи SG, поднятое специально для лендингов. Оно должно быть на нашем домене обязательно, т.к. нам необходимо самостоятельно менеджить  CORS заголовки, для активации новых доменов с лендингами без запросов к SG.
- **Redirector** (10,11): редиректор, который переправляет человека на активное зеркало (дополнительно пробрасывая его параметры авторизации)
- **SG** (12,13): собственно активное зеркало, на котором человек может играть

### Функциональные требования к лендингам
Есть минимальный набор требований, без которых лендинги не имеет смысла делать:
- запоминание состояния формы: если человек обновляет страницу в процессе заполнения формы - никакие данные или состояние не должны быть утеряны.
- редирект уже зарегистированных пользователей: если человек уже прошел регистрацию на лендинге, то при повторном заходе на лендинг он должен быть автоматически сразу отправлен дальше на редиректор
- побег из inapp браузеров: попытка отркытия в inapp браузере должна приводить к открытию полноценного браузера (нам нужно выдергивать stag в нормальный браузер, чтобы и привязка к аффу и авторизация сохранились при закрытии инаппа)
- интеграция с гугл аналитикой: для проверки эффективности необходимо логировать набор ключевых событий:
	- показ формы регистрации
	- touched для каждого поля
	- нажатие на кнопку регистрация
	- успешная регистрация
	- ошибка регистрации
	- открытие формы логина
	- успешный логин
	- не успешный логин
	- ошибка доступа к апи СГ
- обработка недоступности апи: АПИ СГ может неожиданно ломаться, а так же мы не можем отличить поломку, от геоблока. Поэтому необходим отдельный стейт с кнопкой повторить, на котором мы предложим человеку воспользваться VPN
#### out of scope
- AB тесты: в текущей реализации мы не интегрируем решение для АБ тестов. Для реализации тестов на данный момент делаем просто два разных ленда.

## Дизайн
С точки зрения дизайна лендинги делятся на следующие блоки:
- preloader: максимально легкая SVG и прогресс бар, которые мы показываем на старте загрузки (они инлайнятся в html)
- игровая часть: это может быть слот, колесо или что-то еще. Фактически это вся вариативная часть.
- форма регистрации: этот блок одинаковый на всех лендингах, с какой-то возможностью стилизации (желательно ограничится цветами и не более того). Этот блок содержит интеграцию с апи, и все сложные штуки.
- форма логина: этот блок должен быть опционально доступен для перехода с формы регистрации
