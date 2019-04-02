---
language_tabs:
  - shell: cURL
  - php

---

# 1. Разработчикам

<aside class='notice'><br />
<b>POSTMAN</b><br/><br />
<p>Для удобства сделана коллекция для postman( GUI для выполнения HTTP запросов ). Можно скачать c оф.<br />
<a target='blank' href='https://www.getpostman.com/'>сайта</a>.<br />
Псле установки нужно:<br />
- скачать и импортировать <a target='blank' href='events.sk.ru.postman_collection.json'>коллекцию</a> запросов.<br />
- загрузить и импортировать <a target='blank' href='api.events.sk.ru.postman_environment.json'>окружение</a><br />
- ApiKey и Hash в окружении заменить на Ваши<br />
</p><br />
<b>SDK</b><br/><br />
Для интеграции с API можно использовать готовый клиент. Описание методов так же можно найти на странице клиента.<br />
- PHP SDK находится <a target='blank' href='https://bitbucket.org/ruvents/runet-id-php-api-client'>тут</a><br />

</aside>

# 2. Общая информация

<aside class='notice'><br />
Запрос к методам API представляет собой обращение по HTTP-протоколу к URL вида http://api.events.sk.ru/<название метода>.<br />
Переменные методов передаются с помощью GET или POST параметров. В качестве результата возвращается JSON объект.<br />
Для каждого мероприятия генерируются ApiKey и Secret. Доступ к API для каждого ApiKey ограничивается списком ip-адресов.<br />
<strong>Обязательные для всех методов заголовки:</strong><br />
1. ApiKey<br />
2. Hash - вычисляется по формуле md5(ApiKey+Secret)<br />
<strong>Формат возвращаемых ошибок:</strong><br />
{<br />
Error: {Code: int, Message: string}<br />
}<br />
<strong>Мультиаккаунты:</strong><br />
В некоторых случаях выдаётся аккаунт без конкретной привязки к мероприятию. В этом случае большинство методов<br />
начинают требовать передачи дополнительного параметра: EventId, с идентификатором мероприятия в контексте которого<br />
необходимо выполнить действие.<br />
<strong>При использовании php-API:</strong><br />
$api = new \RunetID\Api\Api(ApiKey, Secret, Cache = null);<br />
где Cache - объект класса, реализующего интерфейс \RunetID\Api\ICache
</aside>

# 3. Общие параметры

<aside class='notice'><br />
<b>Общие параметры для всех методов</b><br />
Language – переключение языка приложения и его ответов в “ru” или “en”. По-умолчанию, значение устанавливается в 'ru'.
</aside>

# 4. Авторизация

<aside class='notice'><br />
<b>Шаг 1</b><br />
Добавить следующий код на страницу с вызовом диалога авторизации<br />
window.rIDAsyncInit = function() {<br />
rID.init({<br />
apiKey: <key><br />
});<br />
// Additional initialization code here<br />
};<br />
<br />
// Load the SDK Asynchronously<br />
(function(d){<br />
var js, id = 'runetid-jssdk', ref = d.getElementsByTagName('script')[0];<br />
if (d.getElementById(id)) {return;}<br />
js = d.createElement('script'); js.id = id; js.async = true;<br />
js.src = '//events.sk.ru/javascripts/api/runetid.js';<br />
ref.parentNode.insertBefore(js, ref);<br />
}(document));<br />
<br />
<b>Шаг 2</b><br />
Вызвать метод rID.login(); для создания окна авторизации<br />
<br />
<b>Шаг 3</b><br />
После авторизации, пользователь будет переадресован на исходную страницу с GET-параметром token (переадресация происходит в рамках окна авторизации)<br />
<br />
<b>Шаг 4</b><br />
После получения token - необходимо сделать запрос к API методу user/auth передав параметр token, полученный выше.<br />
API вернет данные авторизованного пользователя (RUNET-ID, ФИО, Место работы, Контакты).<br />
<br />
<b>При использовании php-API:</b><br />
$user = \RunetID\Api\User::model($api)->getByToken(token);<br />

</aside>


# 5. Описание объектов

## 5.1. Компания
Компания


> Объект: 

```json
{
    "Id": 77529,
    "Name": "RUVENTS",
    "FullName": "ООО «РУВЕНТС»",
    "Info": null,
    "Logo": "LNK[Лого компании](#5-2)",
    "Url": "http:\/\/ruvents.com",
    "Phone": "+7 (495) 6385147",
    "Email": "info@ruvents.com",
    "Address": "г. Москва, Пресненская наб., д. 12",
    "Cluster": "РАЭК",
    "ClusterGroups": [],
    "OGRN": null,
    "Employments": [
        "LNK[Пользователь](#5-17)"
    ]
}
```

Параметр | Описание
-------- | --------
Id | Идентификатор компании
Name | Коммерческое название (бренд, краткое название)
FullName | Юридическое название
Info | Информация о компании
Logo | Массив ссылок на логотипы в трех разрешениях
Url | Сылка на сайт компании
Phone | Телефон
Email | Email компании
Address | Адрес компании
Cluster | Кластер,к которому относится компания. Пока только РАЭК
ClusterGroups | Список груп кластеров
OGRN | ОГРН компании
Employments | Массив сотрудников компании

## 5.2. Лого компании
Логотипы компании в трех разрешениях


> Объект: 

```json
{
    "Small": "Ссылка на лого компании (50px*50px)",
    "Medium": "Ссылка на лого компании (90px*90px)",
    "Large": "Ссылка на лого компании (200px*200px)"
}
```

Параметр | Описание
-------- | --------
Small | Логотип размерами 50px на 50px
Medium | Логотип размерами 90px на 90px
Large | Логотип размерами 200px на 200px

## 5.3. Фото пользователя
Фото пользователя в трех разрешениях


> Объект: 

```json
{
    "Small": "http:\/\/events.sk.ru\/files\/photo\/0\/454_50.jpg?t=1475191745",
    "Medium": "http:\/\/events.sk.ru\/files\/photo\/0\/454_90.jpg?t=1475191306",
    "Large": "http:\/\/events.sk.ru\/files\/photo\/0\/454_200.jpg?t=1475191317"
}
```

Параметр | Описание
-------- | --------
Small | Фото размерами 50px на 50px
Medium | Фото размерами 90px на 90px
Large | Фото размерами 200px на 200px

## 5.4. Тест
Тест


> Объект: 

```json
{
    "Id": 1,
    "Title": "Исследование профессионального <br>интернет-сообщества",
    "Questions": {
        "C2": {
            "Title": "Укажите Ваш пол",
            "Values": {
                "1": "Мужской",
                "2": "Женский"
            }
        },
        "Q10": {
            "Title": "С какого курса вы работаете на постоянной основе?",
            "Values": {
                "1": "не работаю и никогда не работал на постоянной основе",
                "2": "работал еще до поступления в вуз",
                "3": "с первого курса",
                "4": "со второго курса",
                "5": "с третьего курса",
                "6": "с четвертого курса",
                "7": "с пятого курса",
                "8": "с шестого курса",
                "9": "со времен учебы в аспирантуре или получения второго высшего образования",
                "10": "начал работать на постоянной основе после завершения обучения"
            }
        }
    }
}
```

Параметр | Описание
-------- | --------
Id | Идентификатор теста
Title | Название теста
Questions | Список вопросов к тесту. Каждый вопрос состоит из кода вопроса(например C2), текста вопроса (например - Укажите Ваш пол) и варианты ответа в виде списка.

## 5.5. Результаты теста
Результаты теста


> Объект: 

```json
{
    "competence\\models\\tests\\mailru2013\\First": {
        "Value": "2"
    },
    "competence\\models\\tests\\mailru2013\\S2": {
        "Value": "5"
    },
    "competence\\models\\tests\\mailru2013\\E1_1": {
        "Value": [
            "1",
            "3",
            "7"
        ]
    },
    "competence\\models\\tests\\mailru2013\\E2": {
        "Value": {
            "1": "4",
            "3": "6",
            "7": "1"
        }
    }
}
```

Параметр | Описание
-------- | --------

## 5.6. Место
Место встречи.


> Объект: 

```json
{
    "Id": 4,
    "Name": "Meeting point 1",
    "Reservation": "true",
    "ReservationTime": 20
}
```

Параметр | Описание
-------- | --------
Id | Айди места встречи
Name | Название
Reservation | Возможность бронирования

## 5.7. Встреча
Встреча.


> Объект: 

```json
{
    "Id": 2817,
    "Place": "LNK[Место](#5-6)",
    "Creator": "LNK[Пользователь](#5-17)",
    "Users": [
        {
            "Status": 1,
            "Response": "",
            "User": "LNK[Пользователь](#5-17)"
        }
    ],
    "UserCount": 1,
    "Start": "2009-02-15 00:00:00",
    "Date": "2009-02-15",
    "Time": "00:00",
    "Type": 1,
    "Purpose": "",
    "Subject": "",
    "File": "",
    "CreateTime": "2017-02-12 23:12:34",
    "Status": 2,
    "CancelResponse": ""
}
```

Параметр | Описание
-------- | --------
Id | Идентификатор встречи
Place | Место встречи
Creator | Создатель встречи
Users | Пользователи, приглашенные на встречу
UserCount | Колличество пользователей приглашенных на встречу
Start | Время начала встречи
Date | Дата встречи
Time | Время встречи
Type | Тип встречи. 1-закрытая,2-открытая
Purpose | Цель встречи
Subject | Тема встречи
File | Прилагаемые материалы. Файл.
CreateTime | Дата создания
Status | Статус. 1-открыта,2-отменена.

## 5.8. Зал
Зал, где может проходить часть или все мероприятие. Залы привязываются к секциям.


> Объект: 

```json
{
    "Id": "599",
    "Title": "Зал 1",
    "UpdateTime": "2017-02-19 12:29:58",
    "Order": "0",
    "Deleted": false
}
```

Параметр | Описание
-------- | --------
Id | Идентификатор
Title | Название зала
UpdateTime | Время последнего обновления зала в формате (Y-m-d H:i:s)
Order | Порядок вывода залов
Deleted | true - если зал удален, false - иначе

## 5.9. Мероприятие
Информация о мероприятии.


> Объект: 

```json
{
    "EventId": 3206,
    "IdName": "Meropriyatiegoda",
    "Name": "Мероприятие 2017 года",
    "Title": "Мероприятие 2017 года",
    "Info": "Краткое описание мероприятия 2017",
    "Place": "г. Волгоград, пр-т Ленина, д. 123",
    "Url": "http:\/\/www.events.sk.ru",
    "StartTime": "2017-02-22 12:00:00",
    "EndTime": "2017-02-22 18:00:00",
    "VisibleOnMain": true,
    "Image": {
        "Mini": "http:\/\/events.sk.ru\/files\/event\/example\/50.png",
        "MiniSize": {
            "Width": 50,
            "Height": 50
        },
        "Normal": "http:\/\/events.sk.ru\/files\/event\/example\/120.png",
        "NormalSize": {
            "Width": 120,
            "Height": 120
        },
        "Original": "http:\/\/events.sk.ru\/files\/event\/example\/original.png"
    },
    "GeoPoint": [
        "",
        ""
    ],
    "Address": "г. Волгоград, пр-т Ленина, д. 123",
    "Menu": [
        {
            "Type": "program",
            "Title": "Программа"
        }
    ],
    "Statistics": {
        "Participants": {
            "ByRole": {
                "24": 1
            },
            "TotalCount": 1
        }
    },
    "FullInfo": "<p>Подробное описание мероприятия 2017<\/p>\r\n"
}
```

Параметр | Описание
-------- | --------
EventId | Идентификатор мероприятия
IdName | Символьный код мероприятия
Name | Название мероприятия
Title | Название мероприятия
Info | Информация о мероприятии
Place | Место проведения мероприятия
Url | Сайт мероприятия
UrlRegistration | 
UrlProgram | 
StartTime | Дата и время начала мероприятия
EndTime | Дата и время окончания мероприятия
Image | Ссылки на логотип мероприятия в двух разрешениях
GeoPoint | Координаты места проведеня мероприятия
Address | Адрес места проведения мероприятия
Menu | 
Statistics | Статистика мероприятия по участникам/ролям
FullInfo | Подробное описание мероприятия

## 5.10. Статус на мероприятии
Статус на мероприятии.


> Объект: 

```json
{
    "RoleId": "идентификатор статуса на мероприятии",
    "RoleTitle": "название статуса",
    "UpdateTime": "время последнего обновления"
}
```

Параметр | Описание
-------- | --------
RoleId | Идентификатор роли участника мероприятия.
RoleTitle | Название роли
UpdateTime | Время последнего обновления

## 5.11. Материал
Материал.


> Объект: 

```json
{
 *         'Id': 1,
 *         'Name': '',
 *         'File': '',
 *         'Comment': true,
 *         'Partner':{
 *             'Name': '',
 *             'Site': '',
 *             'Logo': ''
 *     }
```

Параметр | Описание
-------- | --------
Id | Идентификатор
Name | Название
Comment | Комментарий
File | Файл для скачивания

## 5.12. Заказ
Зал.


> Объект: 

```json
{
    "Id": "идентификатор элемента заказа",
    "Product": "объект Product",
    "Owner": "объект User (сокращенный, только основные данные пользователя)",
    "PriceDiscount": "цена с учетом скидки",
    "Paid": "статус оплаты",
    "PaidTime": "время оплаты",
    "Attributes": "массив с атрибутами (если заданы)",
    "Discount": "размер скидки от 0 до 1, где 0 - скидки нет, 1 - скидка 100%",
    "CouponCode": "код купона, по которому была получена скидка",
    "GroupDiscount": "была скидка групповая или нет"
}
```

Параметр | Описание
-------- | --------
Id | идентификатор

## 5.13. Оплаченный заказ
Оплаченный заказ.


> Объект: 

```json
{
    "Id": "идентификатор элемента заказа",
    "Product": "объект Product",
    "Owner": "объект User (сокращенный, только основные данные пользователя)",
    "PriceDiscount": "цена с учетом скидки",
    "Paid": "статус оплаты",
    "PaidTime": "время оплаты",
    "Attributes": "массив с атрибутами (если заданы)",
    "Discount": "размер скидки от 0 до 1, где 0 - скидки нет, 1 - скидка 100%",
    "CouponCode": "код купона, по которому была получена скидка",
    "GroupDiscount": "была скидка групповая или нет"
}
```

Параметр | Описание
-------- | --------
Id | идентификатор

## 5.14. Товар
Оплаченный заказ.


> Объект: 

```json
{
    "Id": "идентификатор",
    "Manager": "строка, название менеджера (участие, питание и другие)",
    "Title": "название товара",
    "Price": "текущая цена",
    "Attributes": "массив ключ-значение с атрибутами товара"
}
```

Параметр | Описание
-------- | --------
Id | идентификатор

## 5.15. Секция мероприятия
Секция мероприятия.


> Объект: 

```json
{
    "Id": "идентификатор",
    "Title": "название",
    "Info": "краткое описание",
    "Start": "время начала",
    "End": "время окончания",
    "TypeCode": "код типа секции",
    "Places": [
        "LNK[Место](#5-6)"
    ],
    "Halls": [
        "LNK[Зал](#5-8)"
    ],
    "Attributes": [
        "атрибуты"
    ],
    "UpdateTime": "дата\/время последнего обновления",
    "Deleted": "true - если секция удалена, false - иначе"
}
```

Параметр | Описание
-------- | --------
Id | идентификатор
Title | название
Info | краткое описание
Start | время начала
End | время окончания
TypeCode | код типа секции
Places | массив с названиями залов, в которых проходит секция (deprecated)
Halls | залы привязанные к секции
Attributes | дополнительные аттрибуты (произвольный массив ключ => значение, набор ключей и значений зависит от мероприятия)
UpdateTime | дата/время последнего обновления
Deleted | true - если секция удалена, false - иначе

## 5.16. Доклад
User, Company, CustomText - всегда будет заполнено только одно из этих полей. Title, Thesis, FullInfo, Url - могут отсутствовать, если нет информации о докладе, либо роль не предполагает выступление с докладом (например, ведущий)


> Объект: 

```json
{
    "Id": "идентификатор",
    "User": "объект User (может быть пустым) - делающий доклад пользователь",
    "Company": "объект Company (может быть пустым) - делающая доклад компания",
    "CustomText": "произвольная строка с описанием докладчика",
    "SectionRoleId": "идентификатор роли докладчика на этой секции",
    "SectionRoleTitle": "название роли докладчика на этой секции",
    "Order": "порядок выступления докладчиков",
    "Title": "название доклада",
    "Thesis": "тезисы доклада",
    "FullInfo": "полная информация о докладе",
    "Url": "ссылка на презентацию",
    "UpdateTime": "дата\/время последнего обновления",
    "Deleted": "true - если секция удалена, false - иначе."
}
```

Параметр | Описание
-------- | --------
Id | идентификатор
User | объект User (может быть пустым) - делающий доклад пользователь
Company | объект Company (может быть пустым) - делающая доклад компания
CustomText | произвольная строка с описанием докладчика
SectionRoleId | идентификатор роли докладчика на этой секции
SectionRoleTitle | название роли докладчика на этой секции
Order | порядок выступления докладчиков
Title | название доклада
Thesis | тезисы доклада
FullInfo | полная информация о докладе
Url | ссылка на презентацию
UpdateTime | дата/время последнего обновления
Deleted | true - если секция удалена, false - иначе.

## 5.17. Пользователь
Объект пользователя.


> Объект: 

```json
{
    "RocId": 454,
    "UserId": 454,
    "LastName": "Борзов",
    "FirstName": "Максим",
    "FatherName": "",
    "CreationTime": "2007-05-25 19:29:22",
    "Visible": true,
    "Verified": true,
    "Gender": "male",
    "Photo": "LNK[Фото пользователя](#5-3)",
    "Attributes": {},
    "Work": {
        "Position": "Генеральный директор",
        "Company": {
            "Id": 77529,
            "Name": "RUVENTS"
        },
        "StartYear": 2014,
        "StartMonth": 4,
        "EndYear": null,
        "EndMonth": null
    },
    "Status": "LNK[Статус пользователя](#5-18)",
    "Email": "max.borzov@gmail.com",
    "Phone": "79637654577",
    "PhoneFormatted": "8 (963) 765-45-77",
    "Phones": [
        "89637654577",
        "79637654577"
    ]
}
```

Параметр | Описание
-------- | --------
RocId | Айди пользователя
UserId | UserId идентификатор пользователя
LastName | Фамилия пользователя
FirstName | Имя пользователя
FatherName | Отчество пользователя
CreationTime | Дата создания аккаунта
Visible | Видимость
Verified | Подтвержден ли аккаунт
Gender | Пол
Photo | Фотографии пользователя в трех разрешениях
Attributes | Атрибуты
Work | Занимаемая должность
Status | Статус на мероприятии, привязанном к используемому аккаунта api
Email | Электронный адрес
Phone | Номер телефона в формате - 79637654577
PhoneFormatted | Номер телефона в формате - 8 (963) 765-45-77
Phones | Массив всех телефонов

## 5.18. Статус пользователя
Статус пользователя на мероприятии


> Объект: 

```json
{
    "RoleId": 1,
    "RoleName": "Участник",
    "RoleTitle": "Участник",
    "UpdateTime": "2012-04-18 12:06:49",
    "TicketUrl": "Ссылка на билет",
    "Registered": false
}
```

Параметр | Описание
-------- | --------



# 6. Компании

Описаие методов для работы с компаниями

## 6.1. Изменение
Позволяет изменять данные о компании.

`POST https://api.events.sk.ru/company/edit/` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
CompanyId | Айди компании. | Y
Code | Псевдоним компании, подходящий для генерации ЧПУ. | N
Name | Краткое название компании. | N
FullName | Полное название компании. | N
Info | Краткое описание компании. | N
FullInfo | Подробное описание компании. | N
Logo | Логотип компании. Ссылка на скачивание изображения. | N
ContactAddress[PostCode] | Почтовый индекс | N
ContactAddress[CityName] | Название города | N
ContactAddress[CountryName] | Название страны | N
ContactAddress[Street] | Улица | N
ContactAddress[House] | Дом | N
ContactAddress[Building] | Строение | N
ContactAddress[Wing] | Крыло | N
ContactAddress[Apartment] | Квартира | N
ContactAddress[Office] | Офис | N
ContactAddress[Comment] | Дополнительная информация, которая поможет людям найти мето. Например, название бизнес-центра | N
ContactAddress[Place] | Место | N
ContactSite | Адрес сайта | N
ContactEmail | Адрес электронной почты | N
ContactPhone | Контактный телефон | N
Attributes | Настраиваемые атрибуты компании. | N
ServiceAccounts | <p>Список социальных аккаунтов в виде `ServiceAccounts[0][TypeId]=14&ServiceAccounts[0][Account]=805729640&ServiceAccounts[1][TypeId]=1&ServiceAccounts[1][Account]=3109394`</p><p>Поля типов аккаунтов:</p><ul><li><b>TypeId</b> - тип аккаунта. Список возможных типов можете посмротеть тут: `/info/service/types`</li><li><b>Account</b> - номер или логин аккаунта (зависит от конкретной социальной сети).</li></ul> | N
Cluster | Кластер, к которому принадлежит компания. Возможные значения: РАЭК | N


## 6.2. Детальная информация
Возвращает подробную информацию о компании. Так же в ответе будет список сотрудников компании (Employments).


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/company/get?CompanyId=77529'
```


> Ответ: 

```json
"LNK[Компания](#5-1)"
```

`GET https://api.events.sk.ru/company/get` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
CompanyId | Айди компании. | Y


## 6.3. Список
Список компаний из указанного кластера. Пока используется только РАЭК. В списке не присутствуют сотрудники компаний.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/company/list?Cluster=%D0%A0%D0%90%D0%AD%D0%9A'
```


> Ответ: 

```json
{
    "Companies": [
        "LNK[Компания](#5-1)"
    ]
}
```

`GET https://api.events.sk.ru/company/list` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
Builders | Список доступных билдеров:<ul><li><b>EmploymentsCount</b> - количество сотрудников<li><b>LinkEmail.Email</b> - контактный Email<li><b>LinkPhones.Phone</b> - контактный телефон<li><b>LinkAddress.Address</b> - адрес<li><b>LinkAddress.Address.City</b> - адрес с информацией о городе<li><b>LinkAddress.Address.Region</b> - адрес с информацией о регионе<li><b>LinkAddress.Address.Country</b> - адрес с информацией о стране</ul> | N
Cluster | (enum[РАЭК]) – кластер, компании которого необходимо получить. В данный момент может принимать единственное значение: РАЭК. | Y
Query | Поисковая строка. | N
PageToken | Указатель на следующую страницу, берется из результата последнего запроса, значения NextPageToken. | N
MaxResults | MaxResults (число) - максимальное количество компаний в ответе, от 0 до 200. Если нужно загрузить более 200 участников, необходимо использовать постраничную загрузку. | N


# 7. Компетенции

Описаие методов для работы с компетенциями.

## 7.1. Результаты теста
Результаты теста с заданным TestId для пользователя с заданным UserId.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/competence/result?UserId=656438&TestId=55'
```


> Ответ: 

```json
"LNK[Результаты теста](#5-5)"
```

`GET https://api.events.sk.ru/competence/result` 
### Parameters
Название | Обязательно
-------- | -----------
UserId | Y
TestId | Y


## 7.2. Тесты
Доступные для мероприятия тесты.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/competence/tests'
```


> Ответ: 

```json
[
    "LNK[Тест](#5-4)"
]
```

`GET https://api.events.sk.ru/competence/tests` 


# 8. Встречи

Методы для работы со встречами.

## 8.1. Принять приглашение
Если встреча с заданным идентификатором существует, то приглашение на нее 'принимается'.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/connect/accept?MeetingId=2817&UserId=678047'
```


> Ответ: 

```json
{
    "Success": true
}
```

`GET https://api.events.sk.ru/connect/accept` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
MeetingId | Айди встречи. | Y
UserId | UserId пользователя. | Y


## 8.2. Отмена встречи
Отменяет встречу. Статус встречи меняется на 'отменена'


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/connect/cancel?UserId=678047&MeetingId=2817&Response=%D0%9F%D1%80%D0%B8%D1%87%D0%B8%D0%BD%D0%B0%20%D0%BE%D1%82%D0%BC%D0%B5%D0%BD%D1%8B'
```


> Ответ: 

```json
{
    "Success": true
}
```

`GET https://api.events.sk.ru/connect/cancel` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
MeetingId | Айди встречи | Y
UserId | Runetid создателя встречи | Y
Response | Причина отмены | Y


## 8.3. Создание встречи
Создает новую встречу


```shell
curl -X POST -H 'Content-Type: application/x-www-form-urlencoded' -H 'ApiKey: XXX' -H 'Hash: XXX' -d 'PlaceId=1&CreatorId=678047&UserId=678047&Date=15-Feb-2017&Type=1&Purpose=Предложение&Subject=Тема встречи&File='
    'https://api.events.sk.ru/connect/create'
```


> Ответ: 

```json
{
    "Success": true,
    "Meeting": "LNK[Встреча](#5-7)",
    "Errors": []
}
```

`POST https://api.events.sk.ru/connect/create` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
PlaceId | Айди места встречи. | Y
CreatorId | Runetid создателя встречи. | Y
UserId | Runetid пользователя, приглашенного на встречу. | Y
Date | Дата встречи. Формат - http://php.net/manual/ru/datetime.createfromformat.php | Y
Type | Тип встречи. 1-закрытая,2-открытая. | Y
Purpose | Предложение. | N
Subject | Тема встречи | N
File | Прилагаемый файл | N


## 8.4. Отклонение приглашения
Отклоняет приглашение.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/connect/decline?UserId=678047&MeetingId=2817'
```


> Ответ: 

```json
{
    "Success": true
}
```

`GET https://api.events.sk.ru/connect/decline` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | Пользователь | Y
MeetingId | Id встречи | Y


## 8.5. Отправляет приглашение
Отправляет приглашение пользователю на встречу.


```shell
curl -X POST -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/connect/invite?PlaceId=1&CreatorId=1&UserId=1&Date=12-12-2017&Type=1&Purpose=1&Subject=1&File='
```


> Ответ: 

```json
{
    "Success": true,
    "Meeting": "LNK[Встреча](#5-7)",
    "Errors": []
}
```

`POST https://api.events.sk.ru/connect/invite` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
PlaceId | Айди места встречи. | Y
CreatorId | Runetid создателя встречи. | Y
UserId | Runetid пользователя, приглашенного на встречу. | Y
Date | Дата встречи. Формат - http://php.net/manual/ru/datetime.createfromformat.php | Y
Type | Тип встречи. 1-закрытая,2-открытая. | Y
Purpose | Предложение. | N
Subject | Тема встречи | N
File | Прилагаемый файл | N


## 8.6. Список встреч
Списк встреч.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/connect/list?UserId=678047&UserId=678047&CreatorId=678047'
```


> Ответ: 

```json
{
    "Success": true,
    "Meetings": [
        "LNK[Встреча](#5-7)"
    ]
}
```

`GET https://api.events.sk.ru/connect/list` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId |  | N
CreatorId |  | N
UserId |  | N
Type | Тип встречи. 1-закрытая,2-открытая. | N
Status | Сатус встречи. 1-открыта. 2-отменена. | N


## 8.7. Места
Места для встреч.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/connect/places'
```


> Ответ: 

```json
{
    "Success": true,
    "Places": [
        "LNK[Место](#5-6)"
    ]
}
```

`GET https://api.events.sk.ru/connect/places` 


## 8.8. Рекомендации
Рекомендации.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/connect/recommendations?UserId=678047'
```


> Ответ: 

```json
{
    "Success": true,
    "Users": [
        "LNK[Пользователь](#5-17)"
    ]
}
```

`GET https://api.events.sk.ru/connect/recommendations` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId пользователя для которого вернутся рекоммендации. | Y


## 8.9. Поиск
Поиск по участникам мероприятия. (Не работает)


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/connect/search?UserId=678047&q=Ruvents'
```


> Ответ: 

```json
{
    "Success": true,
    "Users": [
        "LNK[Пользователь](#5-17)"
    ]
}
```

`GET https://api.events.sk.ru/connect/search` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
RoleId | Роль участника. | N
Attributes | Атрибуты. | N
q | Поисковый запрос | N


## 8.10. Присоединиться к встрече
Присоединиться к встрече.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/connect/signup?UserId=678047&MeetingId=2817'
```


> Ответ: 

```json
{
    "Success": true
}
```

`GET https://api.events.sk.ru/connect/signup` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | Runetid пользователя. | Y
MeetingId | Id встречи. | Y


# 9. Мероприятия

Методы для работы с мероприятиями. Аккаунт API соответствует конкретному мероприятию. Методы описанные в данном разделе работают с мероприятием аккаунта. Но можно получить список мероприятий в методе list.

## 9.1. Смена роли
Меняет роль заданному пользователю.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/event/changerole?UserId=678047&RoleId=6'
```


> Ответ: 

```json
{
    "Success": true
}
```

`GET https://api.events.sk.ru/event/changerole` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
RoleId | Id новой роли. | Y
UserId | UserId пользователя. | Y


## 9.2. Компании
Выбираются все компании, сотрудники которых учавствуют в мероприятии.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/companies'
```


> Ответ: 

```json
{
    "77529": "LNK[Компания](#5-1)"
}
```

`GET https://api.events.sk.ru/event/сompanies` 


## 9.3. Залы
Список залов мероприятия.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/halls'
```


> Ответ: 

```json
[
    "LNK[Зал](#5-8)"
]
```

`GET https://api.events.sk.ru/event/halls` 
### Parameters
Название | Описание
-------- | --------
FromUpdateTime | (Y-m-d H:i:s) - время последнего обновления залов, начиная с которого формировать список.
WithDeleted | Если параметр задан, не пустой и не приводится к false, возвращаются в том числе удаленные залы, иначе только не удаленные.


## 9.4. Информация о мероприятии
Информация о мероприятии.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/info'
```


> Ответ: 

```json
"LNK[Мероприятие](#5-9)"
```

`GET https://api.events.sk.ru/event/info` 
### Parameters
Название | Описание
-------- | --------
FromUpdateTime | (Y-m-d H:i:s) - время последнего обновления залов, начиная с которого формировать список.
WithDeleted | Если параметр задан, не пустой и не приводится к false, возвращаются в том числе удаленные залы, иначе только не удаленные.


## 9.5. Список мероприятий
Список мероприятий за указанный год. Если год не указан - выбираются мероприятия за текущий. Каждое мероприятие выводится без статистики.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/event/list?Year=2017'
```


> Ответ: 

```json
[
    "LNK[Мероприятие](#5-9)"
]
```

`GET https://api.events.sk.ru/event/list` 
### Parameters
Название | Описание | Значение по умолчанию | Обязательно
-------- | -------- | --------------------- | -----------
TitleSearch | Поисковая строка для поиска по названию мероприятий. |  | N
Type | Фильтр по идентификатору или названию типа мероприятия. |  | N
City | Фильтр по названию города проведения мероприятия. |  | N
Year | Год. | текущий год | N
VisibleOnMain | Главные новости, или новости с установленным флагом отображения на титульной странице. |  | N
Limit | Лимит записей. |  | N
Sort | Порядок сортировки: ASC - сначала старые; DESC - сначала новые. |  | N


## 9.6. Отменя участия.
Отмена участия посетителя. Только для создателей мероприятия.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/participationcancel?UserId=111111'
```


> Ответ: 

```json
{
    "success": true
}
```

`GET https://api.events.sk.ru/event/participationcancel` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId пользователя | Y


## 9.7. Цели мероприятия
Цели мероприятия.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/purposes'
```


> Ответ: 

```json
[
    {
        "Id": 3,
        "Title": "Выступление с докладом"
    },
    {
        "Id": 2,
        "Title": "Обмен опытом"
    },
    {
        "Id": 1,
        "Title": "Образование \/ получение новых знаний"
    },
    {
        "Id": 4,
        "Title": "Хантинг"
    }
]
```

`GET https://api.events.sk.ru/event/purposes` 


## 9.8. Регистрация
Регисрация пользователя на мероприятии с заданной ролью.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/register?UserId=678047&RoleId=2'
```


> Ответ: 

```json
{
    "Success": "true"
}
```

`GET https://api.events.sk.ru/event/register` 
### Parameters
Название | Описание
-------- | --------
UserId | Идентификатор пользователя. Обязательно.
RoleId | Идентификатор статуса, который пользователь должен получить на мероприятии. Обязательно.


## 9.9. Роли
Список возможных ролей участника мероприятия.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/roles'
```


> Ответ: 

```json
[
    "LNK[Статус на мероприятии](#5-10)"
]
```

`GET https://api.events.sk.ru/event/roles` 


## 9.10. UserId участников мероприятия
Список UserId,Ролей участников мерроприятия.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/runetids'
```


> Ответ: 

```json
{
    "308": 1,
    "311": 24,
    "314": 24
}
```

`GET https://api.events.sk.ru/event/runetids` 


## 9.11. Статистика
Статистика по мероприятию. Возвращает колличество участников мероприятия, сгруппированные по ролям.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/statistics'
```


> Ответ: 

```json
{
    "Roles": [
        {
            "RoleId": 25,
            "Name": "Эксперт ПК",
            "Priority": 72,
            "Count": 32
        },
        {
            "RoleId": 26,
            "Name": "Видеоучастник",
            "Priority": 15,
            "Count": 4
        }
    ],
    "Total": 36
}
```

`GET https://api.events.sk.ru/event/statistics` 


## 9.12. Типы мероприятия
Список возможных типов мероприятий. Используются для фильтрации в методе eventList.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/types'
```

`GET https://api.events.sk.ru/event/types` 
### Parameters
Название | Тип | Описание | Значение по умолчанию
-------- | --- | -------- | ---------------------
EventCounts | логический | Если установлен в true, то отображается количество мероприятий каждого типа участия. | N


## 9.13. Регистрация
Регисрация пользователя на мероприятии с заданной ролью.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/event/register?UserId=678047&RoleId=2'
```


> Ответ: 

```json
{
    "Success": "true"
}
```

`GET https://api.events.sk.ru/event/unregister` 
### Parameters
Название | Описание
-------- | --------
UserId | Идентификатор пользователя. Обязательно.
Message | Сообщение, которое будет отправлено пользователю


## 9.14. Участники
Список участников мероприятия с заданной ролью.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/event/users?RoleId=1'
```


> Ответ: 

```json
{
    "Users": [
        "LNK[Пользователь](#5-17)"
    ],
    "TotalCount": 1
}
```

`GET https://api.events.sk.ru/event/users` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
RoleId |  | Массив идентификаторов ролей | Y
Phone | Строка | Фильтр по номеру телефона | N


## 9.15. Фотографии участников
Возвращает Фотографии участников одним архивом.

`GET https://api.events.sk.ru/event/usersphotos` 


## 9.16. Добавление цели мероприятия
Добавляет новую цель посещения мероприятия пользователем.


> Ответ: 

```json
{
    "Success": true
}
```

`GET https://api.events.sk.ru/purpose/add` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | Идентификатор участника. | Y
PurposeId | Идентификатор цели посещения мероприятия. | Y


## 9.17. Удаление цели мероприятия
Удаляет цель посещения мероприятия пользователем.


> Ответ: 

```json
{
    "Success": true
}
```

`GET https://api.events.sk.ru/purpose/delete` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | Идентификатор участника. | Y
Purpose Id | Идентификатор цели посещения мероприятия. | Y


# 10. Раздаточные материалы

Разное, что должно работать как стек: если взял, то это значение более никому не достанется

# 11. Инфраструктура



## 11.1. Список местоположений
Список местоположений.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/infrastructure/placeList'
```

`GET https://api.events.sk.ru/infrastructure/placeList` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
TypeId | Фильтр по типу местоположения. | N
OwnerId | Фильтр по владельцу местоположения. | N
Coordinates | Фильтр по координатам местоположений. На данный момент может принимать два возможных значения: isset и unset, то есть возвращать либо только объекты имеющие координаты или только не имеющие. | 
CanHostMeetings | Фильтр возможности проводить встречи. | N


## 11.2. Список типов местоположений



```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/infrastructure/typeList'
```

`GET https://api.events.sk.ru/infrastructure/typeList` 


## 11.3. Объект: Детально
Получение детальной информации по точке.

`POST https://api.events.sk.ru/infrastructure/pointGet` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
ObjectId |  | Идентификатор объекта, представителем которого является точка. | Y
ObjectClass |  | Тип сущности, представителем которой является точка. Возможные значения: <ul><li><b>User</b> - пользователь</li></ul> | Y
LocateRequest | Логический | Инициировать процесс определения текущего местоположения точки посредством запроса разрешения на это её представителя. | 


## 11.4. Объект: Перемещение
Обновление координат и/или времени последней активности точки.

`GET https://api.events.sk.ru/infrastructure/pointMove` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
PointId | Идентификатор точки. Обязателен, если не указан ObjectId. | Y
ObjectId | Идентификатор объекта, представителем которого является точка. | Y
ObjectClass | Тип сущности, представителем которой является точка. Возможные значения: <ul><li><b>User</b> - пользователь</li></ul> | N
SceneId | Сцена (или карта) на которой в данный момент расположен объект. Обязателен, если указаны координаты. | N
Coordinates | Координаты объекта в формате {lat:X, lng:Y} что в диалекте параметров запроса будет выглядеть так: Coordinates[lat]=X&Coordinates[lng]=Y. Если указаны координаты, то параметр SceneId - обязателен. | N
LocateRequestResult | Логический. Указывает, что перемещение точки происходит вследствии запроса на раскрытие её местоположения. Параметр содержит результат запроса: <b>true</b> или <b>false</b> | N
LocateRequestInviterUserId | Идентификатор инициатора запроса на предоставление координат. Обязательно, если задано значение параметра <b>LocateRequestResult</b> | N
LocateRequestInviteeUserId | Идентификатор респондента запроса на предоставление координат. Обязательно, если задано значение параметра <b>LocateRequestResult</b> | N


## 11.5. Объект: Определение координат
Запрос на предоставление координат пользователя.

`POST https://api.events.sk.ru/infrastructure/pointUserLocate` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
InviterUserId | Идентификатор пользователя, который <b>просит</b> предоставить ему координаты пользователя InviteeUserId. | N
InviteeUserId | Идентификатор пользователя, который <b>предоставляет</b> свои координаты пользователю InviterUserId. | N


# 12. Приглашения на мероприятия

Приглашения на мероприятия.

## 12.1. Запросы на участие
Вернет запросы на участие в мероприятии пользователя с заданным UserId


> Ответ: 

```json
{
    "Sender": "LNK[Пользователь](#5-17)",
    "Owner": "LNK[Пользователь](#5-17)",
    "CreationTime": "2017-02-14 14:12:27",
    "Event": "LNK[Мероприятие](#5-9)",
    "Approved": 0
}
```

`GET https://api.events.sk.ru/invite/get` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId пользователя | Y


## 12.2. Создание приглашения
Создает приглашение на участие в мероприятии пользователя UserId


> Ответ: 

```json
{
    "Success": true
}
```

`GET https://api.events.sk.ru/invite/request` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId пользователя | Y


# 13. Встречи



## 13.1. Создание встречи
Создание встречи.


```shell

curl -X POST -H 'ApiKey: XXX' -H 'Hash: XXX' \
    'https://api.events.sk.ru/meeting/meetingCreate' \
    -F PlaceId=1 \
    -F SlotId=1 \
    -F InviteeId=1,2,3

```

`POST https://api.events.sk.ru/meeting/meetingCreate` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
SlotId | <b>Идентификатор временного слота.</b> Cписок доступных временных слотов можно получить при помощи метода <a href='?shell#14-12'>/meeting/slotList</a> | Y
PlaceId | <b>Идентификатор местоположения</b> в котором должна состояться встреча. Список доступных местоположений можно получить при помощи метода <a href='?shell#11-1'>/infrastructure/placeList</a> | Y
InviteeId | <b>Идентификтор(ы) пользователей</b>, пришлашаемых на встречу. | Y


## 13.2. Удаление встречи
Удаление встречи.


```shell
curl -X POST -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/meeting/meetingDelete'
```

`POST https://api.events.sk.ru/meeting/meetingDelete` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
EventId | Идентификатор мероприятия | Y
MeetingId | Идентификатор встречи | Y


## 13.3. Редактирование встречи
Редактирование встречи.


```shell
curl -X POST -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/meeting/meetingEdit'
```

`POST https://api.events.sk.ru/meeting/meetingEdit` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
EventId | Идентификатор мероприятия | Y
MeetingId | Идентификатор встречи | Y
PlaceId | Идентификатор местоположения | N
SlotId | Идентификатор слота | N
Status | Статус встречи | N
Description | Описание встречи | N


## 13.4. Приглашение на встречу
Приглашение на встречу.


```shell
curl -X POST -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/meeting/meetingInvite'
```

`POST https://api.events.sk.ru/meeting/meetingInvite` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
EventId | Идентификатор мероприятия | Y
MeetingId | Идентификатор встречи | Y
UserId | Идентификаторы приглашаемых пользователей | Y


## 13.5. Просмотр встречи
Просмотр встречи.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/meeting/meetingView'
```

`GET https://api.events.sk.ru/meeting/meetingView` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
MeetingId | Идентификатор встречи | Y


## 13.6. Отправить сообщение
Отправить сообщение.


```shell

curl -X POST -H 'ApiKey: XXX' -H 'Hash: XXX' \
    'https://api.events.sk.ru/meeting/messageCreate' \
    -F EventId=1 \
    -F MeetingId=1 \
    -F Text=test

```

`GET https://api.events.sk.ru/meeting/messageCreate` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
EventId | Идентификатор мероприятия | Y
MeetingId | Идентификатор встречи | Y
Text | Текст сообщения | Y
File | Файл | N


## 13.7. Список сообщений
Список сообщений.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX -G'
    'https://api.events.sk.ru/meeting/messageList' -d EventId=1 -d MeetingId=1
```

`GET https://api.events.sk.ru/meeting/messageList` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
EventId | Идентификатор мероприятия | Y
MeetingId | Идентификатор встречи | Y


## 13.8. Принять приглашение на  встречу
Принять приглашение на&nbsp;&nbsp;встречу.


```shell
curl -X POST -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/meeting/participationAccept'
```

`POST https://api.events.sk.ru/meeting/participationAccept` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
EventId | Идентификатор мероприятия | Y
ParticipationId | Идентификатор прилашения | Y


## 13.9. Отклонить приглашение о встрече
Отклонить приглашение о встрече.


```shell
curl -X POST -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/meeting/participationAccept'
```

`POST https://api.events.sk.ru/meeting/participationDecline` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
EventId | Идентификатор мероприятия | Y
ParticipationId | Идентификатор прилашения | Y


## 13.10. Список участий
Список участий указанного пользователя.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/meeting/participationList'
```

`POST https://api.events.sk.ru/meeting/participationList` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId пользователя для которого показать список участий | Y
EventId | Идентификатор мероприятия (если используется мультиаккаунт) | Y


## 13.11. Объект участия
Просмотр объекта учаcтия во встрече.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/meeting/participationView?ParticipationId=1'
```

`GET https://api.events.sk.ru/meeting/participationView` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId пользователя для которого показать список участий | Y
ParticipationId | Идентификатор прилашения | Y


## 13.12. Создание слота
Создание слота.


```shell
curl -X POST -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/meeting/slotCreate'
```

`POST https://api.events.sk.ru/meeting/slotCreate` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
EventId | Идентификатор мероприятия | Y
Type | Тип слота | Y
StartTime | Время начала | Y
FinishTime | Время окончания | Y


## 13.13. Список слотов
Список слотов.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX -G'
    'https://api.events.sk.ru/meeting/slotList' -d EventId=1
```

`GET https://api.events.sk.ru/meeting/slotList` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
EventId | Идентификатор мероприятия | Y


# 14. Microsoft

Спецпроект для Microsoft

## 14.1. Проверка хеша
Проверяет корректность переданного хеша авторизации пользователя


> Ответ: 

```json
{
    "Result": true
}
```

`GET https://api.events.sk.ru/ms/checkfastauth` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId пользователя | Y
AuthHash | Проверяемый хеш | Y


## 14.2. Создание пользователя
Создание нового пользователя


> Ответ: 

```json
{
    "Result": true
}
```

`GET https://api.events.sk.ru/ms/createuser` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId пользователя | Y
AuthHash | Проверяемый хеш | Y


# 15. Материалы Paperless

Методы для работы с механикой Paperless.

## 15.1. Отметка о прикладывания бейджа к устройству
Отметка о прикладывания бейджа к устройству.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/paperless/materials/search'
```

`POST https://api.events.sk.ru/paperless/materials/search` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
BadgeUID | Уникальный UID приложенного RFID-бейджа. | Y
BadgeTime | Время прикладывания RFID-бейджа. | Y
DeviceNumber | Номер устройства. | Y
Process | Если передано true, то сигнал сразу же обрабатывается. | N


## 15.2. Информация по материалу
Информация по партнёрскому материалу Paperless.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/paperless/materials/get?MaterialId=1'
```


> Ответ: 

```json
[
    "LNK[Материал](#5-11)"
]
```

`GET https://api.events.sk.ru/paperless/materials/get` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
MaterialId | Идентификатор материала. | Y


## 15.3. Список материалов
Список партнёрских материалов Paperless.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/paperless/materials/search'
```


> Ответ: 

```json
[
    "LNK[Материал](#5-11)"
]
```

`GET https://api.events.sk.ru/paperless/materials/search` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | RUNET-ID посетителя для выборки доступных ему материалов. | N
RoleId | Один или несколько статусов участия на мероприятии для выборки доступных им материалов. Внимание! Данное условие перекрывает результаты фильтрации по UserId. Совместное использование параметров UserId и RoleId не проектировалось. | N


# 16. Платежный кабинет

Методы для работы с платежным кабинетом.

## 16.1. Активация счета
Активация и отметка счета об оплате


> Ответ: 

```json
"LNK[Заказ](#5-12)"
```

`POST https://api.events.sk.ru/pay/activateorder` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
OrderId | число | Идентификатор заказа. Обязательно. | Y


## 16.2. Добавление
Добавление заказа


> Ответ: 

```json
"LNK[Заказ](#5-12)"
```

`GET https://api.events.sk.ru/pay/add` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
ProductId | Идентификатор товара. | Y
PayerUserId | Идентификатор плательщика. Обязательно. | Y
OwnerUserId | Идентификатор получателя товара. Обязательно. | Y


## 16.3. Формирование счета
Формирование счета


> Ответ: 

```json
"LNK[Заказ](#5-12)"
```

`POST https://api.events.sk.ru/pay/addorder` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
PayerUserId | число | Идентификатор плательщика. Обязательно. | Y
TypeId | число | Тип счёта:<ol><li>Платежная система<li>Счет<li>Квитанция<li>Счет Деньги Mail.Ru | Y


## 16.4. Купон
Активация купона


> Ответ: 

```json
{
    "Discount": "50%"
}
```

`GET https://api.events.sk.ru/pay/coupon` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
CouponCode | Код купона. | Y
ExternalId | Внешний Id. | N
PayerUserId | Плательщик. Обязательно, если не указан ExternalId. | Y
OwnerUserId | Получатель. Обязательно, если не указан ExternalId. | Y
ProductId | Идентификатор товара. | Y


## 16.5. Удаление
Удаление заказа


> Ответ: 

```json
{
    "Success": "true"
}
```

`GET https://api.events.sk.ru/pay/delete` 
### Parameters
Название | Тип | Описание
-------- | --- | --------
OrderItemId | Число | Идентификатор заказа.
PayerUserId | Число | Идентификатор плательщика.


## 16.6. Редактирование
Редактирование позиций заказа


> Ответ: 

```json
{
    "Success": "true"
}
```

`GET https://api.events.sk.ru/pay/edit` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
OrderItemId | Идентификатор заказа. | Y
PayerUserId | Идентификатор плательщика. | Y
ProductId | Идентификатор товара. | Y
OwnerUserId | Идентификатор получателя товара. | Y


## 16.7. Купон
Покупки

`GET https://api.events.sk.ru/pay/filterbook` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
Manager | Идентификатор менеджера. | Y
Params | Параметры поиска. | Y
BookTime | Время зааказа. | N


## 16.8. Товары
Список товаров

`GET https://api.events.sk.ru/pay/filterlist` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
Manager | Идентификатор менеджера. | Y
Params | Параметры поиска. | Y
Filter | Фильтр. | N


## 16.9. Получение информации по заказу
Получение информации по конкретному заказу.


> Ответ: 

```json
LNK[Оплаченный заказ](#5-13)
```

`GET https://api.events.sk.ru/pay/item` 
### Parameters
Название | Описание
-------- | --------
OrderItemId | Идентификатор заказа.


## 16.10. Список оплаченных заказов
Список оплаченных за пользователя заказов. Возвращает массив с купленными пользователем заказами.


> Ответ: 

```json
{
    "Items": [
        "LNK[Оплаченный заказ](#5-13)"
    ]
}
```

`GET https://api.events.sk.ru/pay/items` 
### Parameters
Название | Описание
-------- | --------
OwnerUserId | Идентификатор.


## 16.11. Список оплаченных заказов
Список оплаченных за пользователя заказов. Возвращает массив с купленными пользователем заказами.


> Ответ: 

```json
{
    "Items": [
        "LNK[Оплаченный заказ](#5-13)"
    ]
}
```

`GET https://api.events.sk.ru/pay/itemsbyproduct` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
ProductId | Идентификатор продукта. | Y
PaidOnly | Только оплаченные | N


## 16.12. Заказы
Список заказов


> Ответ: 

```json
{
    "Items": [
        "LNK[Оплаченный заказ](#5-13)"
    ],
    "Orders": [
        "LNK[Заказ](#5-12)"
    ]
}
```

`GET https://api.events.sk.ru/pay/list` 
### Parameters
Название | Описание
-------- | --------
PayerUserId | Идентификатор плательщика.


## 16.13. Список товаров
Список доступных товаров. (Не работает)


> Ответ: 

```json
[
    "LNK[Товар](#5-14)"
]
```

`GET https://api.events.sk.ru/pay/product` 
### Parameters
Название | Описание
-------- | --------
OwnerUserId | Идентификатор владельца.


## 16.14. Товары
Список доступных товаров, отфильтрованы по менеджеру RoomProductManager


> Ответ: 

```json
[
    {
        "Id": "идентификатор",
        "Manager": "строка, название менеджера (участие, питание и другие)",
        "Title": "название товара",
        "Price": "текущая цена",
        "Attributes": "массив ключ-значение с атрибутами товара"
    }
]
```

`GET https://api.events.sk.ru/pay/rifrooms` 


## 16.15. Получение реквизитов карты


`GET https://api.events.sk.ru/pay/cardCredentials` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
UserId | Число | UserId пользователя | Y


## 16.16. Получение второй части реквизитов карты по СМС


`GET https://api.events.sk.ru/pay/cardSendSms` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
UserId | Число | UserId пользователя | Y


## 16.17. Создание продукта



> Ответ: 

```json
{
    "Success": "true"
}
```

`GET https://api.events.sk.ru/pay/productCreate` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
Title | Строка | Название продукта. | Y
Price | Число | Единица измерения. | Y
StartTime | Время | Время начала периода актуальности цены. | Y
EndTime | Время | Время окончания периода актуальности цены. | N
ManagerName | Строка | Менеджер продукта. | N
Unit | Строка | Единица измерения. | N
Description | Строка | Описание. | N
Priority | Число | Приоритет. | N


## 16.18. Товары
Список доступных товаров


> Ответ: 

```json
[
    "LNK[Товар](#5-14)"
]
```

`GET https://api.events.sk.ru/pay/products` 


# 17. Профессиональные интересы



## 17.1. Добавление
Добавляет участнику мероприятия 'проф. интерес'


> Ответ: 

```json
{
    "Success": true
}
```

`GET https://api.events.sk.ru/professionalinterest/add` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | Идентификатор участника. | Y
ProfessionalInterestId | Идентификатор 'проф. интереса'. | Y


## 17.2. Удаление
Удаляет у частника мероприятия 'проф. интерес'


> Ответ: 

```json
{
    "Success": true
}
```

`GET https://api.events.sk.ru/professionalinterest/dell` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | Идентификатор участника. | Y
ProfessionalInterestId | Идентификатор 'проф. интереса'. | Y


## 17.3. Список
Список доступных проф. интересов


> Ответ: 

```json
[
    {
        "Id": 1,
        "Title": "Аналитика"
    }
]
```

`GET https://api.events.sk.ru/professionalinterest/list` 


# 18. РАЭК

РАЭК.

## 18.1. Список комиссий РАЭК
Список комиссий РАЭК.'


> Ответ: 

```json
{
    "Commissions": []
}
```

`GET https://api.events.sk.ru/raec/commissionlist` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
CommissionIdList | Идентификаторы комиссий РАЭК. | N
Purpose Id | Идентификатор цели посещения мероприятия. | Y


## 18.2. Список участников комиссии РАЭК
Список участников комиссии РАЭК.'

`GET https://api.events.sk.ru/raec/commissionusers` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
CommissionId | Идентификатор комиссий РАЭК. | N


# 19. Секции мероприятий

Раздел описывает методы для работы с секциями мероприятий.

## 19.1. Добавление в избранное
Добавление секции в избранное


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/section/addFavorite?UserId=656438&SectionId=4107'
```


> Ответ: 

```json
{
    "Success": "true"
}
```

`GET https://api.events.sk.ru/section/addFavorite` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId участника. | Y
SectionId | Идентификатор секции. | Y


## 19.2. Удаление из избранного
Удаление секции из избранного.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX' 'https://api.events.sk.ru/section/deleteFavorite?UserId=656438&SectionId=4107'
```


> Ответ: 

```json
{
    "Success": "true"
}
```

`GET https://api.events.sk.ru/section/deleteFavorite` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId пользователя. | Y
SectionId | Идентификатор секции. | Y


## 19.3. Список избранных
Список избранных секций.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/section/favorites?UserId=656438'
```


> Ответ: 

```json
[
    "LNK[Секция мероприятия](#5-15)"
]
```

`GET https://api.events.sk.ru/section/favorites` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | Идентификатор. | Y
FromUpdateTime | (Y-m-d H:i:s) - время последнего обновления избранных секций пользователя, начиная с которого формировать список. | N


## 19.4. Информация о секции
Информация о конкретной секции.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/section/info?SectionId=4107'
```


> Ответ: 

```json
"LNK[Секция мероприятия](#5-15)"
```

`GET https://api.events.sk.ru/section/info` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
SectionId | Идентификатор секции. | Y


## 19.5. Секции
Список секций.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/section/list
```


> Ответ: 

```json
[
    "LNK[Секция мероприятия](#5-15)"
]
```

`GET https://api.events.sk.ru/section/list` 
### Parameters
Название | Описание
-------- | --------
FromUpdateTime | (Y-m-d H:i:s) - время последнего обновления секций, начиная с которого формировать список.


## 19.6. Доклады
Список докладов.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/section/reports?SectionId=4109'
```


> Ответ: 

```json
[
    "LNK[Доклад](#5-16)"
]
```

`GET https://api.events.sk.ru/section/reports` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
SectionId | Идентификатор секции. | Y
FromUpdateTime | Время последнего обновления доклада, начиная с которого формировать список. | N


## 19.7. Секции с залами
Список секций с залами и атрибутами.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/section/updated?SectionId=4109'
```


> Ответ: 

```json
[
    "LNK[Доклад](#5-16)"
]
```

`GET https://api.events.sk.ru/section/updated` 


## 19.8. Секции пользователя.
Список секций в которых учавствует пользователь. Секции возвращаются с залами и атрибутами.


```shell
curl -X GET -H 'ApiKey: XXX' -H 'Hash: XXX'
    'https://api.events.sk.ru/section/user?UserId=656438'
```


> Ответ: 

```json
[
    "LNK[Секция мероприятия](#5-15)"
]
```

`GET https://api.events.sk.ru/section/user` 
### Parameters
Название | Описание | Обязательно
-------- | -------- | -----------
UserId | UserId пользователя. | Y


# 20. Пользователи

Загрузка данных пользователя. Работа с пользователями.

## 20.1. Отметка о записи бейджа
Привязывает бейдж к посетителю мероприятия.


> Ответ: 

```json
{
    "Success": true
}
```

`POST https://api.events.sk.ru/user/badge` 
### Parameters
Название | Тип | Описание
-------- | --- | --------
UserId | Число | runetid пользователя. Обязателен.
BadgeId | Число | уникальный идентификатор RFID-бейджа. Обязателен.


## 20.2. Создание
Создает нового пользователя.

`POST https://api.events.sk.ru/user/create` 
### Parameters
Название | Тип | Описание | Значение по умолчанию | Обязательно
-------- | --- | -------- | --------------------- | -----------
Email | Строка | Email. |  | Y
LastName | Строка | Фамилия. |  | Y
FirstName | Строка | Имя. |  | Y
FatherName | Строка | Отчество. |  | 
FullName | Строка | Cтрока с ФИО пользователя из которой будут автоматически выделяться компоненты имени |  | 
Phone | Строка | Телефон. |  | 
Photo | Строка | Ссылка на фотографию. |  | 
Company | Строка | Компания. |  | 
Position | Строка | Должность. |  | 
ExternalId | Строка | Внешний идентификатор пользователя для привязки его профиля к сторонним сервисам. |  | 
Attributes | Массив | Расширенные атрибуты пользователя. |  | 
Description | Строка | Произвольное, многоязычное описание пользователя. |  | 
Visible | Логическое | Видимость пользователя. | true | 
Temporary | Логическое | Временный пользователь. | false | 
DoUnsubscribe | Логическое | Сразу же отписать пользователя от рассылок. | false | 
SubscribedForMailings | Логическое | Позволяет подписать или отписать создаваемого пользователя от EMail рассылок. | true | 
DeviceType | Строка | Тип регистрируемого устройства пользователя. Обязателен, если указан параметр DeviceToken. Возможные значения: iOS, Android. |  | 
DeviceToken | Строка | Уникальный идентификатор устройства для получения push-уведомлений. |  | 


## 20.3. Устройство
Регистрация нового устройства для push-уведомлений.

`POST https://api.events.sk.ru/user/device` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
UserId | Число | runetid пользователя. | Y
DeviceType | Строка | Тип регистрируемого устройства пользователя. Возможные значения: iOS, Android. | Y
DeviceToken | Строка | Уникальный идентификатор устройства для получения push-уведомлений. | Y


## 20.4. Редактирование
Редактирует пользователя.

`POST https://api.events.sk.ru/user/edit` 
### Parameters
Название | Тип | Описание
-------- | --- | --------
Email | Строка | Email.
LastName | Строка | Фамилия.
FirstName | Строка | Имя.
FatherName | Строка | Отчество.
Photo | Строка или Файл | Файл, загружаемый посредством multipart form, или ссылка на фотографию.
Attributes | Массив | Расширенные атрибуты пользователя.
Description | Строка | Произвольное, многоязычное описание пользователя.
ExternalId | Строка | Внешний идентификатор пользователя для привязки его профиля к сторонним сервисам.


## 20.5. Детальная информация
Возвращает данные пользователя, включая информацию о статусе участия в мероприятии.


```php
<?php $user = \RunetID\Api\User::model($api)->getByUserId(UserId); 
```


> Ответ: 

```json
{
    "UserId": "идентификатор",
    "LastName": "фамилия",
    "FirstName": "имя",
    "FatherName": "отчество",
    "CreationTime": "дата регистрации пользователя",
    "Photo": "объект Photo({Small, Medium, Large}) - ссылки на 3 размера фотографии пользователя",
    "Email": "email пользователя",
    "Gender": "пол посетителя. Возможные значения: null, male, female",
    "Phones": "массив с телефонами пользователя, если заданы",
    "Work": "объект с данными о месте работы пользователя",
    "Status": {
        "RoleId": "идентификатор статуса на мероприятии",
        "RoleTitle": "название статуса",
        "UpdateTime": "время последнего обновления"
    }
}
```

`GET https://api.events.sk.ru/user/get` 
### Parameters
Название | Тип | Описание
-------- | --- | --------
UserId | Число | runetid пользователя. Обязателен, если не указан другой параметр.
Email | Строка | email пользователя. Обязателен, если не указан другой параметр.
Participating | Логический | Если указан, то возвращается посетитель только в том случае, если он является участником текущего мероприятия.
Visible | Логический | Если установлен в false, то будет возвращён только скрытый пользователь, если таковой будет найден в соответствии с другими параметрами запроса.
ExternalId | Строка | внешний идентификатор пользователя для привязки его профиля к сторонним сервисам. Обязателен, если не указан другой параметр.
Builders | Список, разделённый запятами | Набор идентификаторов, модифицирующий результат выполнения запроса. Возможные значения: Person, Description, Birthday, Employment, Event, Data, Badge, Contacts, Address, Attributes, ExternalId, AuthData, Photo, DeprecatedData, Participations, Employments, Settings, Favorites, Coordinates


## 20.6. Получение информации по social hash
Возвращает данные пользователя, включая информацию о статусе участия в мероприятии.


> Ответ: 

```json
{
    "UserId": "идентификатор",
    "LastName": "фамилия",
    "FirstName": "имя",
    "FatherName": "отчество",
    "CreationTime": "дата регистрации пользователя",
    "Photo": "объект Photo({Small, Medium, Large}) - ссылки на 3 размера фотографии пользователя",
    "Email": "email пользователя",
    "Gender": "пол посетителя. Возможные значения: null, male, female",
    "Phones": "массив с телефонами пользователя, если заданы",
    "Work": "объект с данными о месте работы пользователя",
    "Status": {
        "RoleId": "идентификатор статуса на мероприятии",
        "RoleTitle": "название статуса",
        "UpdateTime": "время последнего обновления"
    }
}
```

`GET https://api.events.sk.ru/user/getbysocial` 
### Parameters
Название | Тип | Описание
-------- | --- | --------
SocialId | Число | SocialId пользователя. Обязателен
Hash | Строка | Hash из таблицы OAuthSocial


## 20.7. Авторизация
Авторизация, проверка связки Email и Password.


> Ответ: 

```json
{
    "UserId": "идентификатор",
    "LastName": "фамилия",
    "FirstName": "имя",
    "FatherName": "отчество",
    "CreationTime": "дата регистрации пользователя",
    "Photo": "объект Photo({Small, Medium, Large}) - ссылки на 3 размера фотографии пользователя",
    "Email": "email пользователя",
    "Gender": "пол посетителя. Возможные значения: null, male, female",
    "Phones": "массив с телефонами пользователя, если заданы",
    "Work": "объект с данными о месте работы пользователя",
    "Status": "объект с данными о статусе пользователя на мероприятии"
}
```

`GET https://api.events.sk.ru/user/login` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
Email | строка | Email | Y
Credential | Cтрока | Адрес Email, телефон или UserId зарегистрированного пользователя | Y
Password | строка | Пароль | Y
DeviceType | строка | Тип регистрируемого устройства пользователя. Обязателен, если указан параметр DeviceToken. Возможные значения: iOS, Android. | 
DeviceToken | строка | Уникальный идентификатор устройства для получения push-уведомлений. | 


## 20.8. Смена пароля
Позволяет сменить пароль указанного пользователя.

`POST https://api.events.sk.ru/user/passwordChange` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
CurrentPassword | Cтрока | Текущий пароль | Y
NewPassword | Cтрока | Новый пароль | Y


## 20.9. Восстановление пароля
Инициирует отправку письма с инструкциями по восстановлению пароля.

`POST https://api.events.sk.ru/user/passwordRestore` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
Credential | Cтрока | Адрес Email, телефон или UserId зарегистрированного пользователя | Y


## 20.10. Поиск
Поиск пользователей по базе RUNET-ID.


> Ответ: 

```json
{
    "Users": "массив пользователей",
    "NextPageToken": "указатель на следующую страницу"
}
```

`GET https://api.events.sk.ru/user/search` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
Query | Строка | может принимать значения Email, UserId, список UserId через запятую, Фамилия, Фамилия Имя, Имя Фамилия | 
EventId | Число|Строка | Только для собственных мероприятий! Если указан, то поиск происходит только среди посетителей указанного мероприятия. Если равен Current, то поиск происходит только среди посетителей текущего мероприятия. | 
Visible | Число | Если равен 0, поиск осуществляется по скрытым пользователям. | 
Phone | Строка | Фильтр по номеру телефона | N


## 20.11. Установка фотографии
Устанавливает фотографию посетителя из файла изображения или ссылки на него.

`GET https://api.events.sk.ru/user/setphoto` 
### Parameters
Название | Тип | Описание
-------- | --- | --------
Photo | Файл | Файл с фотографией посетителя.
PhotoUrl | Строка | URL адрес фотографии посетителя.


## 20.12. Настройки
Позволяет отписать и подписать пользователя на следующие события: EMail рассылки, Push уведомления, индексацию профиля в поисковых системах. Важное замечание: изменяется состояние только тех полей, которые были переданы.

`POST https://api.events.sk.ru/user/settings` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
SubscribedForMailings | Логический | Отписка от EMail рассылок. | N
SubscribedForPushes | Логический | Отказ от Push уведомлений. | N
AllowProfileIndexing | Логический | Управление запретом индексации профиля в поисковых системах. | N


## 20.13. Избранное: Добавление
Добавляет сущность указанного класса в избранное для указанного пользователя. Не возвращает ошибку при попытке добавления уже существующего объекта.


> Ответ: 

```json
{Success:true}
```

`POST https://api.events.sk.ru/user/favoriteCreate` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
UserId | Число | Уникальный идентификатор пользователя | Y
ObjectClass | Строка | Тип добавляемого объекта. Возможные значения: <ul><li><b>Event</b> - мероприятие</li><li><b>Section</b> - секция программы мероприятия</li><li><b>Place</b> - местоположение (объект инфраструктуры: выставочный стенд, кафе, переговорная комната и т.п.)</li><li><b>Company</b> - компания</li><li><b>Product</b> - продукт</li><li><b>User</b> - пользователь</li></ul> | Y
ObjectId | Число | Идентификатор добавляемого объекта. | Y


## 20.14. Избранное: Удаление
Удаляет сущность указанного класса из избранного для указанного пользователя.


> Ответ: 

```json
{Success:true}
```

`POST https://api.events.sk.ru/user/favoriteDelete` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
UserId | Число | Уникальный идентификатор пользователя | Y
ObjectClass | Строка | Тип добавляемого объекта. Возможные значения: <ul><li><b>Event</b> - мероприятие</li><li><b>Section</b> - секция программы мероприятия</li><li><b>Place</b> - местоположение (объект инфраструктуры: выставочный стенд, кафе, переговорная комната и т.п.)</li><li><b>Company</b> - компания</li><li><b>Product</b> - продукт</li><li><b>User</b> - пользователь</li></ul> | Y
ObjectId | Число | Идентификатор добавляемого объекта. | Y


## 20.15. Избранное: Импорт
<p>Позволяет импортировать избранные данные из разных источников.</p><p><b>Как готовить данные для параметра EncodedCredentials</b>:</p><ul><li><b>Телефоны</b>: убираем все символы не являющиеся цифрами. Если номер телефона содержит только 10 символов, то дописываем к нему 7 что бы получить номер вида 79221234567. Считаем md5 хеш полученного числа и приписываем в его начало латинский символ 'p'.</li><li><b>Email адреса</b>: на всякий случай удаляем пробельные символы в начале и конце строки и переводим все символы в нижний регистр. Считаем md5 хеш полученной строки и дописываем в его начало латинский символ 'e'.</li></ul><p>В результате должны получиться строки 33 символа длиной, первым символом которых могут быть только 'p' или 'e'. Склеиваем строки, используя в качестве разделителя запятую и передаём в параметре EncodedCredentials.</p>

`POST https://api.events.sk.ru/user/favoriteImport` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
UserId | Число | Уникальный идентификатор пользователя для которого импортировать список избранного | Y
EncodedCredentials | Строка | Список номеров телефонов и адресов email через запятую в формате '(p;e)md5(только цифры телефона;email в нижнем регистре)' где p = телефон, e = email | 


## 20.16. Избранное: Поиск
<p>Позволяет отобразить список избранного для указанного пользователя.</p><p><b>Как готовить данные для параметра EncodedCredentials</b>:</p><ul><li><b>Телефоны</b>: убираем все символы не являющиеся цифрами. Если номер телефона содержит только 10 символов, то дописываем к нему 7 что бы получить номер вида 79221234567. Считаем md5 хеш полученного числа и приписываем в его начало латинский символ 'p'.</li><li><b>Email адреса</b>: на всякий случай удаляем пробельные символы в начале и конце строки и переводим все символы в нижний регистр. Считаем md5 хеш полученной строки и дописываем в его начало латинский символ 'e'.</li></ul><p>В результате должны получиться строки 33 символа длиной, первым символом которых могут быть только 'p' или 'e'. Склеиваем строки, используя в качестве разделителя запятую и передаём в параметре EncodedCredentials.</p>

`POST https://api.events.sk.ru/user/favoriteSearch` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
UserId | Число | Уникальный идентификатор пользователя для которого отобразить список избранного | Y
ObjectClass | Строка | Отобразить элементы только указанного типа, но зато с подробной по ним информацией. Возможные значения: <ul><li><b>Event</b> - мероприятие</li><li><b>Section</b> - секция программы мероприятия</li><li><b>Place</b> - местоположение (объект инфраструктуры: выставочный стенд, кафе, переговорная комната и т.п.)</li><li><b>Company</b> - компания</li><li><b>Product</b> - продукт</li><li><b>User</b> - пользователь</li></ul> | 
EncodedCredentials | Строка | Список номеров телефонов и адресов email через запятую в формате '(p;e)md5(только цифры телефона;email в нижнем регистре)' где p = телефон, e = email | 


## 20.17. Избранное: Обращение
Добавляет сущность указанного класса в избранное для указанного пользователя, если она отсутствует в избранном, и удаляет её в случае, если она уже есть.


> Ответ: 

```json
{Success:true}
```

`POST https://api.events.sk.ru/user/favoriteToggle` 
### Parameters
Название | Тип | Описание | Обязательно
-------- | --- | -------- | -----------
UserId | Число | Уникальный идентификатор пользователя | Y
ObjectClass | Строка | Тип добавляемого объекта. Возможные значения: <ul><li><b>Event</b> - мероприятие</li><li><b>Section</b> - секция программы мероприятия</li><li><b>Place</b> - местоположение (объект инфраструктуры: выставочный стенд, кафе, переговорная комната и т.п.)</li><li><b>Company</b> - компания</li><li><b>Product</b> - продукт</li><li><b>User</b> - пользователь</li></ul> | Y
ObjectId | Число | Идентификатор добавляемого объекта. | Y




# 21. Ошибки 

<aside class='notice'>
Описание ошибок
</aside>

Код ошибки | Описание
---------- | --------
400 | Bad Request – Your request sucks.
401 | Unauthorized – Your API key is wrong.
403 | Forbidden.
404 | Not Found.
405 | Method Not Allowed.
406 | Not Acceptable.
410 | Gone.
418 | I’m a teapot.
429 | Too Many Requests.
500 | Internal Server Error.
503 | Service Unavailable.


<script>$(document).ready(function(){$("pre").each(function(a,b){var c=$(b).html();c=c.replace(/LNK\[([^\]\n]+)\]\(([^\)\n]+)\)/g,'<a href="$2">$1</a>'),$(b).html(c)})});</script>