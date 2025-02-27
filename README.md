# Отправка комментария к фотографии профиля ВКонтакте (jump_tages)

Данный проект демонстрирует, как с помощью **Postman** и **API ВКонтакте** отправить комментарий к фотографии профиля пользователя [jump_tages](https://vk.com/jump_tages).

---

## Шаг 1. Создать приложение ВКонтакте (Standalone)

1. Перейти на [страницу для разработчиков ВКонтакте](https://vk.com/editapp?act=create).
2. Создать новое приложение типа **Standalone** (для тестовых запросов — самый удобный вариант).
3. Скопировать `ID приложения` (App ID).

---

## Шаг 2. Получить `access_token`

Чтобы работать с методами ВК, нужен токен доступа:

1. Сформируйте ссылку (замените `YOUR_APP_ID` на ваш реальный App ID):
https://oauth.vk.com/authorize? client_id=YOUR_APP_ID &display=page &redirect_uri=https://oauth.vk.com/blank.html &scope=photos,offline &response_type=token &v=5.131
2. Откройте эту ссылку в браузере, разрешите доступ к фотографиям.
3. После авторизации в адресной строке вы увидите `access_token=...`. Скопируйте токен (до первого `&`).
   
Токен **не** стоит публиковать в открытом доступе, чтобы никто не получил доступ к вашему аккаунту.


## Шаг 3. Узнать `user_id` профиля jump_tages

1. Откройте **Postman** и создайте новый **GET**-запрос:
https://api.vk.com/method/users.get ?user_ids=jump_tages &access_token=ВАШ_ACCESS_TOKEN &v=5.131
2. Нажмите **Send**.  
3. В ответе будет нечто вроде:
```json
{
  "response": [
    {
      "id": 189704458,
      "first_name": "Jump",
      "last_name": "Tages"
    }
  ]
}
Значит, user_id = 189704458.

## Шаг 4. Получить photo_id фотографии профиля
Создайте ещё один GET-запрос:
https://api.vk.com/method/photos.get
    ?owner_id=189704458
    &album_id=profile
    &access_token=ВАШ_ACCESS_TOKEN
    &v=5.131
В ответе найдите массив items. Пример:
{
  "response": {
    "count": 1,
    "items": [
      {
        "id": 303057922,
        "album_id": -6,
        "owner_id": 189704458,
        ...
      }
    ]
  }
}
Здесь photo_id = 303057922.
## Шаг 5. Отправить комментарий к фото
Создайте третий GET-запрос:
https://api.vk.com/method/photos.createComment
    ?owner_id=189704458
    &photo_id=303057922
    &message=Привет+из+Postman
    &access_token=ВАШ_ACCESS_TOKEN
    &v=5.131
В случае успеха в ответе вернётся:
{
  "response": 91
}
Это ID созданного комментария (91).
Шаг 6. Проверка
Зайдите на страницу jump_tages.
Откройте фото с id = 303057922.
Убедится, что комментарий (например, «Привет из Postman») появился.


