Проблема: если мы при регистрации с лендинга как destination на продукте указываем url содержащий GET параметры - человека не авторизует после регистрации.

Пример: мы в значение redirect uri указываем https://turabet.com/tr/promotions?modal=payment-cashier Ожидаемое поведение - человека отправит на экран депозита сразу после авторизации. Но эндпоинт https://tura.bet/websitewebapi/api/v3/GoogleAccount/Callback формирует некорректный редирект урл вида: https://turabet.com/tr/promotions?modal=payment-cashier?playerToken=a5d9f688e2cf48b39e6bc0d94ebbbe31
Обратите внимание, что вместо добавления playerToken как get параметра через `&` осуществлена конкатенация его с redirectUri, что привозит к некорректному урл.

Рекомендуется использовать методы класса URL для работы с урл, вместо методов работы со строками