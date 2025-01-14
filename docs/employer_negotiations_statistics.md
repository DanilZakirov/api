# Статистика по работе с откликами

* [Статистика работодателя](#employer-stats)
* [Статистика менеджера](#manager-stats)

Для получения информации необходимо авторизоваться под работодателем.
Для пользователя без авторизации или для неправильно авторизованного пользователя вернется ответ `403 Forbidden`.
Статистика менеджера доступна самому менеджеру, а также менеджерам с [типом](employer_managers.md#dict) `main_contact_person`.

<a name="employer-stats"></a>
## Статистика работодателя

### Запрос

```
GET /employers/{employer_id}/negotiations_statistics
```

 где `employer_id` - идентификатор работодателя, который можно узнать в
 [информации о текущем пользователе](https://api.hh.ru/openapi/redoc#tag/Informaciya-o-menedzhere/paths/~1me/get).

### Ответ

Успешный ответ приходит с кодом `200 OK` и содержит тело:

```json
{
    "employer_statistics": {
        "received": 20,
        "viewed_percent": 23,
        "viewed_percent_change": 10,
        "replied_percent": 0,
        "replied_percent_change": -15,
        "average_reply_time": 1.0,
        "politeness": {
            "index": 62,
            "index_change": -1,
            "hint": "Ваш Индекс вежливости выше среднего — это хороший показатель, и все же он может быть лучше!",
            "description": "Индекс вежливости видят соискатели после отклика на вакансию. Компании с низким Индексом теряют доверие соискателей и рискуют пропустить подходящих кандидатов.",
            "article_url": "https://hh.ru/article/23734"
        }
    }
}
```

<a name="employer_statistics_field"></a>
#### Объект `employer_statistics`

Содержит статистику по откликам компании.

| Имя | Тип | Описание |
|-----|-----|----------|
| received | number | количество откликов на вакансии работодателя, полученных за последние `30 дней` (далее период) |
| viewed_percent | number или null | процент прочитанных откликов на вакансии работодателя за период или `null` при отсутствии откликов |
| viewed_percent_change | number или null | изменение viewed_percent по сравнению с предыдущим периодом или `null` при отсутствии откликов |
| replied_percent | number или null | процент откликов на вакансии работодателя, перемещенных в любую другую [коллекцию](employer_negotiations.md#term-collection) с отправкой сообщения за период или `null` при отсутствии откликов |
| replied_percent_change | number или null | изменение replied_percent по сравнению с предыдущим периодом или `null` при отсутствии откликов |
| average_reply_time | number или null | среднее время в днях между получением отклика на вакансии работодателя и отправкой сообщения или `null` при отсутствии отправленных сообщений |
| politeness | object или null | [Объект `politeness`](#employer_politeness_field) или `null`, если [индекс вежливости](https://hh.ru/article/23734) компании рассчитать нельзя, например,  пока нет ни одной вакансии или отклика |

<a name="employer_politeness_field"></a>
#### Объект `politeness` для компании

Содержит информацию об [индексе вежливости](https://hh.ru/article/23734) компании. 

| Имя | Тип | Описание |
|-----|-----|----------|
| index | number | значение индекса вежливости компании |
| index_change | number | изменение индекса вежливости компании по сравнению с предыдущим периодом |
| hint | string | краткое описание текущего значения индекса вежливости компании |
| description | string | краткое описание понятия индекса вежливости |
| article_url | string | url статьи про индекс вежливости |


### Ошибки

* `404 Not found` - Работодатель не найден, или у пользователя нет прав

<a name="manager-stats"></a>
## Статистика менеджера

### Запрос

```
GET /employers/{employer_id}/managers/{manager_id}/negotiations_statistics
```

где:

* `employer_id` - идентификатор работодателя, который можно узнать в
  [информации о текущем пользователе](https://api.hh.ru/openapi/redoc#tag/Informaciya-o-menedzhere/paths/~1me/get).
* `manager_id` - идентификатор менеджера.

### Ответ

Пример ответа:

```json
{
    "manager_statistics": {
        "received": 20,
        "viewed_percent": 23,
        "viewed_percent_change": 10,
        "replied_percent": 0,
        "replied_percent_change": -15,
        "average_reply_time": 1.0,
        "politeness": {
            "index": 19,
            "index_change": 0,
            "hint": "У вас низкий Индекс вежливости, но вы можете его повысить!",
            "description": "Индекс вежливости видят соискатели после отклика на вакансию. Компании с низким Индексом теряют доверие соискателей и рискуют пропустить подходящих кандидатов.",
            "article_url": "https://hh.ru/article/23734"
        }
    }
}
```

<a name="manager_statistics_field"></a>
#### Объект `manager_statistics`

Содержит статистику по откликам менеджера.

| Имя | Тип | Описание |
|-----|-----|----------|
| received | number | количество откликов на вакансии менеджера, полученных за последние `30 дней` (далее период) |
| viewed_percent | number или null | процент прочитанных откликов на вакансии менеджера за период или `null` при отсутствии откликов |
| viewed_percent_change | number или null | изменение viewed_percent по сравнению с предыдущим периодом или `null` при отсутствии откликов |
| replied_percent | number или null | процент откликов на вакансии менеджера, перемещенных в любую другую [коллекцию](employer_negotiations.md#term-collection) с отправкой сообщения за период или `null` при отсутствии откликов |
| replied_percent_change | number или null | изменение replied_percent по сравнению с предыдущим периодом или `null` при отсутствии откликов |
| average_reply_time | number или null | среднее время в днях между получением отклика на вакансии менеджера и отправкой сообщения или `null` при отсутствии отправленных сообщений |
| politeness | object или null | [Объект `politeness`](#manager_politeness_field) или `null`, если [индекс вежливости](https://hh.ru/article/23734) менеджера рассчитать нельзя, например, у него пока нет ни одной вакансии или отклика |

<a name="manager_politeness_field"></a>
#### Объект `politeness` для менеджера

Содержит информацию по [индексу вежливости](https://hh.ru/article/23734) менеджера. 

| Имя | Тип | Описание |
|-----|-----|----------|
| index | number | значение индекса вежливости менеджера |
| index_change | number | изменение индекса вежливости менеджера по сравнению с предыдущим периодом |
| hint | string | краткое описание текущего значения индекса вежливости менеджера |
| description | string | краткое описание понятия индекса вежливости |
| article_url | string | url статьи про индекс вежливости |

### Ошибки

* `404 Not found` - Работодатель или менеджер не найден, или у пользователя нет прав.
