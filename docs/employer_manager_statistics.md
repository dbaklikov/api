# Статистика реакции работодателя на отклики

Для получения информации необходимо авторизоваться под работодателем.
Для пользователя без авторизации или для неправильно авторизованного пользователя вернется ответ `403 Forbidden`.

### Запрос

`GET /employers/{employer_id}/manager_statistics`

где `employer_id` - идентификатор работодателя, который можно узнать в
[информации о текущем пользователе](me.md#employer-info).

### Ответ

Содержание ответа меняется в зависимости от [типа текущего менеджера](employer_managers.md#dict):

* менеджерам с типом `main_contact_person` доступна статистика работодателя и статистика всех менеджеров
* менеджерам с другими типами доступна собственная статистика

Пример ответа для менеджера с типом `main_contact_person`:
```json
{
    "employer_statistics": {
        "received": 500,
        "read_percent": 57.0,
        "read_percent_change": 50.0,
        "replied_percent": 43.0,
        "replied_percent_change": -30.0,
        "average_reply_time": 2.0
    },
    "manager_statistics": [
        {
            "manager_id": "3726333",
            "received": 50,
            "read_percent": 23.0,
            "read_percent_change": 0.0,
            "replied_percent": 48.0,
            "replied_percent_change": 5.0,
            "average_reply_time": 1.0
        },
        {
            "manager_id": "3785302",
            "received": 50,
            "read_percent": 23.0,
            "read_percent_change": 0.0,
            "replied_percent": 48.0,
            "replied_percent_change": 5.0,
            "average_reply_time": 1.0
        }
    ] 
}
```

Пример ответа для менеджера с другим типом:
```json
{
    "manager_statistics": [
        {
            "manager_id": "3726333",
            "received": 50,
            "read_percent": 23.0,
            "read_percent_change": 0.0,
            "replied_percent": 48.0,
            "replied_percent_change": 5.0,
            "average_reply_time": 1.0
        }
    ]
}

```

Объект `employer_statistics` и объекты из списка `manager_statistics` включают следующие поля:

| Имя | Тип | Описание |
| --- | --- | -------- |
| received | number | количество откликов, полученных за последние 30 дней |
| read_percent | number | процент прочитанных менеджером откликов соискателей |
| read_percent_change | number | изменение read_percent по сравнению с предыдущими 30 днями |
| replied_percent | number | процент полученных откликов соискателей, перемещенных в любую другую [коллекцию](employer_negotiations.md#term-collection) с отправкой сообщения |
| replied_percent_change | number | изменение replied_percent по сравнению с предыдущими 30 днями |
| average_reply_time | number | среднее время с момента отклика до перемещения в другую коллекцию с сообщением в днях |

Объекты из списка `manager_statistics` также включают поле `manager_id` — идентификатор менеджера.

### Ошибки

* `404 Not found` — Работодатель не найден, или у пользователя нет прав
