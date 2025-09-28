Input: [original doc](./of-original), [figma](https://www.figma.com/design/C2pbaFaNxmV0z8g3asY8w3/Onefun-2.0)

Goals: выписать описание продукта, и необходимые компоненты системы.

Продукт представляет собой каталог событий по разным регионам. К примеру: Лисаббон - SBC конференция 16-19 сентября, запуск воздушных шаров 24 сентября. Мюнхен - фестиваль феерверков 20 сентября. И тому подобное. Ближайший референс: afisha.ua

Приложение соответственно занимается сведением клиентов, и организаторов событий.


## GDS
Самих GDS  (Global Distribution System) есть несколько, кроме известного нам Amadeus, у Galileo тоже есть GDS. Они имеют разное покрытие, и чуть разные схемы расчетов поддерживают.
### Amadeus
Амадеус предоставляет GDS в виде АПИ. Для использования GDS мы должны оплачивать каждый запрос к нему.

На самом деле это несколько независимых АПИ.
- hotel-api: https://developers.amadeus.com/self-service/category/hotels
	- list	
	- search
	- booking
	- ratings
- flight-api https://developers.amadeus.com/self-service/category/flights
	- search
	- book
	- куча разных под апи (выбор места, поиск предложений и т.п.)
- Tours and Activities: _What are the best tours and activities in this location?_ у них есть готовое апи по поиску локальных интересностей.

Однако само по себе апи не предоставляет ничего кроме информации. Для совершения бронирований необходимо вступать во взаимоотношения с продавцом.

В целом в сфере продажи бронирования отелей есть две схемы работы:
- Мы получаем от отеля розничную цену. Клиет покупает у нас бронирование целиком, мы переводим деньги отелю, после выезда человека из отеля мы получаем кешбек от отеля, в виде коммисии.
- Мы получаем от отеля оптовую цену. Клиент покупает у нас бронирование за ту цену, которую мы назовем (с нашим интересом). Мы переводим отелю оплату, за вычетом нашей цены.

GDS поддерживает обе схемы работы, но похоже разные отели используют разные механики.

Сам интерфейс работы с GDS чуть пугает - [инструкция](https://amadeus.com/documents/products/travel-sellers/hotel-content/amadeus-gds-basic-hotel-cryptic-commands-quick-card.pdf), что может привести к тому что этот интерфейс нужно нужно будет несколько раз менять.

### Payment processing
https://developers.amadeus.com/blog/build-hotel-booking-engine-amadeus-api

> Currently, the Hotel Booking API permits booking at hotels that accept credit cards. During booking, Amadeus transmits the payment and guest information to the hotel but does not confirm the validity. Avoid unwanted cancellations by validating guest and payment information before completing the booking  
>
> When your application stores, transmits, or processes cardholder information, you’ll need to comply with PCI Data Security Standard (PCI DSS). For more information, visit the [PCI Security Council website](https://www.pcisecuritystandards.org/merchants).

это на самом деле большая проблема для нас, т.к. это накладываем много ограничений (вплоть до того, что заставляет разбить нас приожение на части, и часть хостить специальным образом).

В апи амадеуса мы в открытой форме передаем данные карты клиента для букинга.
```json
"payment": {
	"method": "CREDIT_CARD",
	"paymentCard": {
		"paymentCardInfo": {
        "vendorCode": "VI",
        "cardNumber": "4151289722471370",
        "expiryDate": "2026-08",
        "holderName": "BOB SMITH"
	  }
}
```
