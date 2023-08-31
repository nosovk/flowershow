
Существует два варианта запуска зала.

## Streamer only
В этом варианте зал функционирует только как онлайн студия, то есть все аппараты не доступны для игры локально.

Мы отправляем платы и мини пк которые вставляются в аппараты. 
1 аппарат, одна плата 1 мини пк.

Дополнительная опция: джекпот сервер.
Если вам нужны джекпоты, то мы отправляем сервер для управления джекпотами.


Вы подключаете компьютер и плату к ПК (мы настраиваем их заранее, вам нужно только их включить).

Все компьютеры должны быть заведены в локальную сеть с DHCP, чтобы компьютеры получали интернет и IP адреса.

Компьютер с джекпотом должен быть в той же сети, что и аппараты. Он должен иметь статический адрес (настраивается на роутере).
После подключения оборудования нам нужно до настроить аппараты.

В настройках аппарат должен быть включен SAS и кеш трансфер. Вы можете прислать список моделей и мы каждому напишем что именно в нем нужно включить.

После этого все работает.

## Streamer vs player
В этом режиме один и тот же аппарат работает и для онлайн и для офлайн игроков.

Есть два варианта реализации

- Мы можем интегрироваться с вашей системой управления залом. Вы можете спросить об API со стороны NetEnt BMS,  которая используется у вас. Если вам дадут доступ к API - мы интегрируемся с ним.
- Мы можем установить для вас EasyCMS,  с которой мы уже проинтегрировали решение. Мы пришлем вам другой сервер, и вы будете использовать его как систему управления залом. EasyCMS - работает как веб сайт через который ведется управление залом.

Шаги по аппаратам не отличаются от `Streamer only`, но для работы аппаратов с офлайн игроками нужно интегрировать ее с системой управления залами.