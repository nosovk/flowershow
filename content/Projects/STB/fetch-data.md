# Получение данных из STRAPI проекта
Все статьи и все сущности доступны в автогенериуремом API. Документация на контракт API:
https://docs.strapi.io/dev-docs/api/rest/

### Авторизация
Для доступа к апи вам понадобиться получить токен. Используется обычный Auth Bearer Token, передавайте его в Authorization Header
```json
headers: {
        Authorization:
          'Bearer xxxxTokenxxxx',
      },
}     
```

## Примеры

Запрос статей с категорией TBX
```bash
curl --request GET \
  --url 'https://statbet.pp.ua/api/articles?filters%5Bsubcategory%5D%5Bstb_id%5D%5B%24eq%5D=tbx&publicationState=preview&filters%5BpublishedAt%5D%5B%24null%5D=true&pagination%5BpageSize%5D=10&pagination%5Bpage%5D=1&sort=createdAt%3Adesc&locale=uk'
```

> Важно: в запросе выше стоит фильтр по статусу статьи - получать только неопубликованные статьи. Он необходим, чтобы статьи не попадали на сайт statbet. Если мы хотим добавлять какие-то статьи которые будут для внешних потребителей - не нужно устанавливать им статус опубликовано


пример ответа:
```json
{
      "id": 49457, // уникальный id статьи
      "createdAt": "2024-05-22T13:46:47.566Z", // дата генерации
      "title": "Чи стане Тейлор Свіфт новою зіркою Marvel? Раян Рейнольдс та Шон Леві зберігають інтригу", // заголовок статьи
      "main_text": "# markdown text", // внутри текст в формате markdown
      "updatedAt": "2024-05-22T13:46:48.943Z", // дата обновления статьи, можно игнорировать
      "publishedAt": null, // дата публикации, можно игнорировать
      "tags": "Тейлор Свіфт,Раян Рейнольдс,Шон Леві,Marvel,Дедпул,Росомаха,Іскра", // теги через запятую
      "slug": "chi-stane-tejlor-svift-novoyu-zirkoyu-marvel-rayan-rejnolds-ta-shon-levi-zberigayut-intrigu", // url для статьи
      "locale": "uk", // язык статьи, по нему можно фильтровать
      "short_text": "", // которткий превью статьи, он выключен
      "image_position": null, // игнориовать
      "sorting_priority": null, // тоже игнорировать
      "youtube": null // может быть ссылка на youtube
    },
```

### Выбор полей
Используя GET параметры можно передать список и порядок полей для получения сущности
`fields[0]=title&fields[1]=body` к примеру вернет только два поля из статьи

## Получение связанных сущностей
Есть опция [Populate](https://docs.strapi.io/dev-docs/api/rest/populate-select#population) через которую можно в одном запросе получать связанные сущности. Желательно не тянуть из апи поля или сущности которые не используются, т.к. это может существенно аффектить производительность — это служебное апи и оно не имеет кеша.

### Текст статьи
Статьи хранятся в формате [Markdown](https://www.markdownguide.org/basic-syntax/). Для получения данных в html их необходимо предваритально отрендерить.