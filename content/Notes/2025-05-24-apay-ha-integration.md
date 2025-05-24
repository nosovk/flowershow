Базово опираемся на схему из https://api.apay.pp.ua/docs/internal/task

[![](https://mermaid.ink/img/pako:eNqNVE1vGjEQ_SsjX_lQYIFlfYiEmraJ1F4a5VJxMbsGrIC99UfJFvHfO-PFfKWVcrM9z_PezDzNnpWmkowzJ38FqUv5oMTKiu1ci-CNDtuFtHguvbHw4uhcC-tVqWqhPTyvTX39MqtF88XYLQgXz0CX95Drl8fZ9T2r3FxD5Ovd33eIhcMT7CjmDSxCA0vUM7gDGSwhCUFISs1BaeWV8BLwtpX4x1JtzhMyakJor036Vf2W4K3QDktURsPTAwhdwcuPbykvoaMUDitCb5R-Teoo1jnVjABD-hLtMpYO556c0BycRJYErIQXl-I6jzOeRF-qI4zYxP6A1GKxkVVsRClsRTHATsbiWg4rXW1wpBGfZJ9ZLnTXwrl_oM6iMUb5KmVl6fkNlG7ECok2wWpr3hpwoSylO3-QGydBGyoQcalTaeiphpsSjkn-W8GHSKW1SIQACim9ete4G9KI_zBlm_1EqKvb2R_7szLkkfglgU-uhNaWVvpgNQS7gYVwOGV0pm9qeenJ3tGUbm12VxZG-WoTzd4Wdem4nVysjbniPFLehm-sr41XywZCXAGsy7bSboWqcHHsCT1nfo2jnDOOx0rY1zmb6wPiaIs8N7pk3Nsgu8yasFozvhQ4jy4LNVo_rZzTK-4AxvfsjfFeMe7n-WA8viuyaV5kxbDLGnweTCb98XSSTfJxPimy0V1-6LI_xmCOQX-IT0VeTAajLBsOhllM-DMGWw04N1xn39u9F9dfUvI5Rk5CVpYqPArH_kj7yQTtGR9Ox6PDX3IvvVY?type=png)](https://mermaid.live/edit#pako:eNqNVE1vGjEQ_SsjX_lQYIFlfYiEmraJ1F4a5VJxMbsGrIC99UfJFvHfO-PFfKWVcrM9z_PezDzNnpWmkowzJ38FqUv5oMTKiu1ci-CNDtuFtHguvbHw4uhcC-tVqWqhPTyvTX39MqtF88XYLQgXz0CX95Drl8fZ9T2r3FxD5Ovd33eIhcMT7CjmDSxCA0vUM7gDGSwhCUFISs1BaeWV8BLwtpX4x1JtzhMyakJor036Vf2W4K3QDktURsPTAwhdwcuPbykvoaMUDitCb5R-Teoo1jnVjABD-hLtMpYO556c0BycRJYErIQXl-I6jzOeRF-qI4zYxP6A1GKxkVVsRClsRTHATsbiWg4rXW1wpBGfZJ9ZLnTXwrl_oM6iMUb5KmVl6fkNlG7ECok2wWpr3hpwoSylO3-QGydBGyoQcalTaeiphpsSjkn-W8GHSKW1SIQACim9ete4G9KI_zBlm_1EqKvb2R_7szLkkfglgU-uhNaWVvpgNQS7gYVwOGV0pm9qeenJ3tGUbm12VxZG-WoTzd4Wdem4nVysjbniPFLehm-sr41XywZCXAGsy7bSboWqcHHsCT1nfo2jnDOOx0rY1zmb6wPiaIs8N7pk3Nsgu8yasFozvhQ4jy4LNVo_rZzTK-4AxvfsjfFeMe7n-WA8viuyaV5kxbDLGnweTCb98XSSTfJxPimy0V1-6LI_xmCOQX-IT0VeTAajLBsOhllM-DMGWw04N1xn39u9F9dfUvI5Rk5CVpYqPArH_kj7yQTtGR9Ox6PDX3IvvVY)

Goal: интегрировать новое платежное направление HA

- в список поддерживаемых провайдеров добавить новый, с названием "ha" (к примеру [тут](https://github.com/apayments/apayments.src/blob/main/configs/userKeys.json#L22))
- в метод инициализации платежа ([тут](https://github.com/apayments/apayments.src/blob/main/src/routes/api/v1/init-payment/%2Bserver.ts)) добавляем поддержку опциональных параметров
	- country
	- street
	- city
	- postal_code
	- state
	- customerLastName
	- phone

все новые параметры опциональны.

- для метода ha меняем возвращаемый url на новый.

Сейчас мы используем [/api/v1/integration/%provider%/process/](https://github.com/apayments/apayments.src/blob/main/src/routes/api/v1/integration/%5Bprovider%5D/process/%2Bserver.ts)
вместо этого предлагаю добавить /api/v1/integraion/ha/process, который был бы уже формой оплаты, а не просто редиректом

- apay payment form
Платежная форма должна содержать предзаполненые данные, которые соответствуют тому что пришло при инициализации платежа, если данных нет - мы предлагаем человеку их заполнить.
Платежная форма должна быть разбита на два степа (степпер). На первом экране человек вносит данные карты и фио, на втором экране человек вносит данные об адресе (если они не были предзаполнены). Если какие-то поля переданы на уровне генерации запроса на оплату - мы их не выводим и скрываем.

- сабмит платежной формы
при сабмите платежной формы мы должны обогатить введенные данные информаций об ip человека и сделать запрос к HA серверу для обработки платежа. Как урл для успешного платежа необходимо передать [/api/v1/integration/ha/success/](https://github.com/apayments/apayments.src/blob/main/src/routes/api/v1/integration/%5Bprovider%5D/success/%2Bserver.ts). 
Мы при сабмите формы в любом случае делаем редирект, просто в зависимости от ответа апи это либо 3ds ссылка, либо success прокси, либо error proxy.

Все данные отправляемые в HA апи необходимо логировать (в том числе данные ип и карты)
## Макет платежной формы
![[apay-form.png]]
Валидация полей - на самом деле нам не нужно на данный момент валидировать поля. Непонятно есть ли вообще какая-то валидация у ннижележащей системы. Пустоту она тоже принимает