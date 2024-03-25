Goal: ознакомится с PimCore

Задача:
внтури PimCore создать новый класс (product), с полями
- name: string
- gitn: integer, uniq
- image: asset
- date: date
Создать команду, которая бы принимала URL с JSON внтури, формат json:
```json
{name: "product", gtin: "123456789012", image:"https://example.com/image.jpg", date: "2024-03-25"}
```
Пример URL для импорта: https://liv-cdn.pages.dev/pim/test.json
Необходимо создать товары 
Важно: ключ для поиска элементов - gtin. При запуске команды, значения из PIM перезаписываются новыми из JSON.
Важно: картинки могут быть битыми, необходимо корректно загрузить их в пим
