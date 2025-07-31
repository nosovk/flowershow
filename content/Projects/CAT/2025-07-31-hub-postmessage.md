## Goal:
Реализовать механизм уведомления host окна об изменении статуса внутри paymenthub iframe.

Бизнес цель: у нас есть форма быстрого депозита [пример](https://wagibet.com/lands/en/wagibet-fast-deposit?partnerId=4&playerToken=22a9e1557a074fbea1ee7206b44aa145), которая позволяет на платформе nextcode загрузить платежный фрейм, до фактической инициализации nextcode платформы (в том числе и до авторизации пользователя на платформе). Это позволяет существенно сократить время ожидания пользователя. Однако при этом возникает проблема, что если человек совершает депозит - он остается висеть на экране платежного фрейма, с надписью - спасибо за ваш платеж. Хотелось бы, чтобы человека после успешного платежа редиректило уже на nextcode платформу и он мог играть. Соответственно нам нужно получить от фрейма информацию о наступлении события, после которого оригинальную вкладку можно закрывать.

Таких событий у нас 4:
- произошло открытие платежной формы в новой табе
- произошел успешный платеж
- произошел не успешный платеж
- произошел или нет платеж мы не знаем - pending статус

### диаграмма статусов

[![](https://mermaid.ink/img/pako:eNqNU8tu2zAQ_BWCZ8ewZdd6HHJpL7kF6K3QIYy4kQhbJEMu47iG_71L6lEZaIBagCEuZ2dmh9SVN0YCr7iH9wC6gR9KtE70tRYBjQ79Kzh6b9A4Zk_iEldWOFSNskIj0_CJ95UuvN4XrLe1js_Qzx4eH1NbxZ7YOQLQMCsutY7FtEsU1aj2JFfMWNBMvZErqDXtLRhOSh_H_h4m8ZlnoKhYqz4gQe88JJXEnVjUXw1xQvb885k5ykQ5kCR2ZiiImtFvcjCxe3BE_3IxgTVCs-ZkPDDsBLKz0tKcX4a2hTIlMiqPxITELkla0ZIBOBFFXPpgrXHoZ2v_YiKH5LJBMhqj8HY5S2qg0oy2zjTg_RTZPWJI3pkPJWEO1dBNmKhiNFPdh6YBCXLY-TqahPPeD7g0GzhHF6oXR6Xbeydf06SeBccZYt7IWlhYQoHBx_kF683_sNIpSDIx8moaJv0N4AVWS2aNx54GoSMaDmxQi1ebr3jrlOQVugAr3oPrRVzyaySuOXZACfKKXqVwx5rX-kY99H38Mqaf2pwJbcerN0HjrXiwUuD0Pc5VR07AfTdBI6-223KXWHh15Z9pvd5ui2J3KMo8237b0e6FV4fDel8W-3y_z6iYHfLbiv9Oupt1XpbZLttsDps8K8vi9gfA0l8I?type=png)](https://mermaid.live/edit#pako:eNqNU8tu2zAQ_BWCZ8ewZdd6HHJpL7kF6K3QIYy4kQhbJEMu47iG_71L6lEZaIBagCEuZ2dmh9SVN0YCr7iH9wC6gR9KtE70tRYBjQ79Kzh6b9A4Zk_iEldWOFSNskIj0_CJ95UuvN4XrLe1js_Qzx4eH1NbxZ7YOQLQMCsutY7FtEsU1aj2JFfMWNBMvZErqDXtLRhOSh_H_h4m8ZlnoKhYqz4gQe88JJXEnVjUXw1xQvb885k5ykQ5kCR2ZiiImtFvcjCxe3BE_3IxgTVCs-ZkPDDsBLKz0tKcX4a2hTIlMiqPxITELkla0ZIBOBFFXPpgrXHoZ2v_YiKH5LJBMhqj8HY5S2qg0oy2zjTg_RTZPWJI3pkPJWEO1dBNmKhiNFPdh6YBCXLY-TqahPPeD7g0GzhHF6oXR6Xbeydf06SeBccZYt7IWlhYQoHBx_kF683_sNIpSDIx8moaJv0N4AVWS2aNx54GoSMaDmxQi1ebr3jrlOQVugAr3oPrRVzyaySuOXZACfKKXqVwx5rX-kY99H38Mqaf2pwJbcerN0HjrXiwUuD0Pc5VR07AfTdBI6-223KXWHh15Z9pvd5ui2J3KMo8237b0e6FV4fDel8W-3y_z6iYHfLbiv9Oupt1XpbZLttsDps8K8vi9gfA0l8I)
На диаграмме изображены пути взаимодействия игрока с платформой.
Нас интересуют шаги:
- 6: на шаге #6 мы мы показываем человеку сообщение, о том что окно может быть закрыто (фактически человек переходит в новый таб, в котором уже открывается платежная форма psp)
- 11,12,13: на этих шагах мы показываем человеку сообщение, о том как завершился его платеж.
Мы бы хотели, чтобы платежный виджет сообщал о переходе в эти состояния, используя [window.postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage). 

Пример использования:
```javascript
window.postMessage("paymenthub:payment-success")
```
Это апи, позволяет хост окну, вызвашему iframe получить это сообщение. Для упрощения интеграции предлагается префиксировать все сообщения от paymenthub константной строкой: `paymenthub:`. В таком случае на клиенте будет легко отфильтровать сообщения от paymenthub от сообщений получаемых от игр к примеру.

Предлагается использовать 4 сообщения:
- `paymenthub:user-forwarder` - для состояния 6 (открыли новую табу с psp)
- `paymenthub:payment-success` - для состояния 11 (успешный платеж)
- `paymenthub:payment-error` - для состояния 12 (не успешный платеж)
- `paymenthub:payment-pending` - для состояния 13 (pending)

В хост окне мы разместим скрипт вида:
```javascript
window.addEventListener("message", (event) => {
  if (event.data.startswith("paymenthub:") redirectToNextCode()
}
```
так же при необходимости мы сможем редиректить человека в разные места на платформе nextcode, в зависимости от статуса платежа.