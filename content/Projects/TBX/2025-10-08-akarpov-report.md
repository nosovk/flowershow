В ходе двух первых недель сделаны задачи:

- https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/issues/2815 Довільні назви промокодів
- https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/issues/2803 час закінчення сеансу
- https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/issues/2295 передавать таймзону Отчет по продажам real-time
- https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/work_items/2820 поправити верстку Квота
- https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/issues/2817 інформер в налаштуваннях віджету
- https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/issues/2825 змінні  шаблонів input
- https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/issues/2816 модалка при зміні статуса замовлення
- https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/work_items/2836 зміна підтвердження статусу в модалці
- https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/work_items/2835 модалка Продаж абонементів
- https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/work_items/2820 поправити верстку Квота

Помимо этого была сделанна работа по задаче редактор шаблонов:
https://cp.tbx.pp.ua/git/ticketcrm/frontend/crm/-/issues/2163

Ветка обновлена, реализованна проверка синхронизации данных в метаформате JSON и HTML итогового письма, редактирование шаблона с разными компонентами так же проверенно и работает корретно.
Обнаружено что необходима доработка со стороны бекенда для загрукзи картинок в шаблоны. Рендеринг переменных шаблона в PDF так же работает. Увы созданные в редакторе письма не могут корректно отрендерится в mpdf на бекенде, поэтому задача пока откладывается (в течение пары недель Миша сможет сказать получится ли переехать на другой движок рендеринга PDF - Goetenberg).

Код отправленный в PR корректно сделан и оформлен, с технической стороны все супер. Парень хорошо справляется с поставленными задачами, особенно учитывая сложность проекта и малое время на онборд.