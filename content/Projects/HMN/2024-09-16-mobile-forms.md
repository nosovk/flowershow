# mobile forms
Добавить в мобильное приложение возможномть вставлять настраиваемый список форм. В зависимости от пароля может быть доступен разный список форм.

## PimCore
### Создать классы
Необходимо с Ex забрать следующие два класса:
- [MobileFormElement](http://jsonblob.com/1285404320643014656)
- [MobileLogin](http://jsonblob.com/1285404199213719552)
![[mobile-classes.png]]
### Настроить datahub
добавить эти классы в datahub query schema
![[datahub-query-schema.png]]
В настройках каждого класса необходимо указать что все поля доступны для запроса, к примеру:
![[datahub-query-fields-list.png]]
Важно: уже существующие настройки меняться не должны, на окружениях уже должны быть созданы настройки доступа к appAssetes, их важно не менять.
### Создать объекты
Внутри роута /APP в дата обьектах необходимо создать минимум один экземпляр формы и логина.
К примеру:
![[data-object-login.png]]
![[data-object-form.png]]

> все настройки классов и datahub должны хранится в гите, чтобы их можно было переносить между окружениями. Ручное редактирование схем данных должно сбрасываться при деплое.

Важно: имя интеграции `app-asset` захардкожено в мобильное приложение, его менять не нужно, так же как и пароль в нем менять ни в коем случае не нужно.

Путь /APP прописан в Security схеме уже, его не нужно менять.

## Mobile
Мы бы хотели добавить возможность логина и выбора формы.

В экране профиля добавляем кнопку Login.

При нажатии на нее мы показываем форму ввода пароля, и кнопку Login.

При успешном вводе пароля мы показываем список из доступных форм.

При выборе элемента из списка мы открываем урл связанный с элементом в вебвью.


### API 
Для проверки логина мы используем следующий запрос:
get login:
```
query GetMobileLoginListing($filter: String!) {
  getMobileLoginListing(
    filter: $filter
  ) {
    edges {
      node {
        id
        classname
        creationDate
        name
        password
        role
      }
    }
    totalCount
  }
}
{
    "filter": "{\"password\": \"knjknds\"}"
  }
```
Пример ответа сервера:
```json
{
  "data": {
    "getMobileLoginListing": {
      "edges": [
        {
          "node": {
            "id": "55016",
            "classname": "MobileLogin",
            "creationDate": 1726153153,
            "name": "salesmans",
            "password": "knjknds",
            "role": "sales"
          }
        }
      ],
      "totalCount": 1
    }
  }
}
```
Для проверки успешности логина мы проверяем значение поля totalCount. Если оно больше или равно единице - то логин успешен. Если поле отсутствует или равно нулю - ошибка аутентификации.
При успешном логине мы должны забрать значение поля `role`, оно является фильтром для второго зарпоса - получить список форм.

get forms:
```graphql
query($filter: String) {
  getMobileFormElementListing(
    filter: $filter
  ) {
    edges {
      node {
        id
        classname
        creationDate
        name
        role
        url
      }
    }
    totalCount
  }
}

{
    "filter": "{\"role\": \"sales\"}"
}
```

Response example:
```json
{
  "data": {
    "getMobileFormElementListing": {
      "edges": [
        {
          "node": {
            "id": "55015",
            "classname": "MobileFormElement",
            "creationDate": 1726153101,
            "name": "rfq form",
            "role": "sales",
            "url": "https://human.de/contacts"
          }
        }
      ],
      "totalCount": 1
    }
  }
}
```
Каждый элемент массива edges это наш эдемент в списке форм для показа.
- name - это имя формы в списке
- url - это урл который необходимо открыть в вебвью.