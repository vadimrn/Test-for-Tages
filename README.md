# Отправка комментария к фотографии профиля ВКонтакте (jump_tages)

Данный проект выполнен в рамках тестового задания, как с помощью **Postman** и **API ВКонтакте** отправить комментарий к фотографии профиля пользователя [jump_tages](https://vk.com/jump_tages).

---

Шаг 1. Создал приложение ВКонтакте (Standalone)
Перешёл на страницу для разработчиков ВКонтакте: vk.com/editapp?act=create.
Создал новое приложение типа Standalone.
Скопировал ID приложения (App ID).

Шаг 2. Получил access_token
Чтобы работать с методами ВКонтакте, мне понадобился токен доступа:

1. Сформировал ссылку (заменив YOUR_APP_ID на реальный App ID):
   
https://oauth.vk.com/authorize?
    client_id=YOUR_APP_ID
    &display=page
    &redirect_uri=https://oauth.vk.com/blank.html
    &scope=photos,offline
    &response_type=token
    &v=5.131
2. Открыл эту ссылку в браузере и разрешил доступ к фотографиям.
3. После авторизации в адресной строке увидел access_token=... и скопировал токен (до первого &).
   
Шаг 3. Узнал user_id профиля jump_tages
1. В Postman создал новый GET-запрос:
   
   https://api.vk.com/method/users.get
    ?user_ids=jump_tages
    &access_token=ВАШ_ACCESS_TOKEN
    &v=5.131
2. Нажал Send.
3. В ответе получил примерно:

 {
  "response": 
      {
      "id": 189704458,
      "first_name": "Jump",
      "last_name": "Tages"
    }
  ]
}
Значит, user_id = 189704458.

Шаг 4. Получил photo_id фотографии профиля
1. Создал ещё один GET-запрос:

   https://api.vk.com/method/photos.get
    ?owner_id=189704458
    &album_id=profile
    &access_token=ВАШ_ACCESS_TOKEN
    &v=5.131
2. В ответе нашёл массив items. Пример:
   {
  "response": {
    "count": 1,
    "items": 
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

Шаг 5. Отправил комментарий к фото

1.Создал третий GET-запрос:

https://api.vk.com/method/photos.createComment
    ?owner_id=189704458
    &photo_id=303057922
    &message=Привет+из+Postman
    &access_token=ВАШ_ACCESS_TOKEN
    &v=5.131

2. Если запрос успешен, в ответе получаю что-то вроде
   {
  "response": 91
}
Это ID созданного комментария (91).

Шаг 6. Проверил результат
Перешёл на страницу jump_tages.
Открыл фото с id = 303057922.
Убедился, что мой комментарий (например, «Привет из Postman») появился.
