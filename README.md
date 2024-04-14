# Сервис url-shortener
Сервис предоставляет пользователям возможность делать короткие ссылки. Переходя по короткой ссылке, созданной в сервисе, браузер перенаправляется на оригинальный адрес.

## Запуск приложения
1. Запустите spring boot приложение командой `./gradlew bootRun`
2. Отправьте запрос на сокращение ссылки с помощью утилиты командной строки curl. Тело запроса содержится в файле [create-req.json](./create-req.json). 
Выполните команду `curl -X POST -d "@create-req.json" -H "Content-Type: application/json" http://localhost:8080/make_shorter`
В ответ придет примерно такой JSON: 
    ```
    {
        "shortUrl":"http://localhost:8080/hnh",
        "secretKey": "abcfjiedis"
    }
    ```
    Где `shortUrl` - адрес, переход по которому, осуществит перенаправление запроса на оригинальный адрес, `secretKey` - ключ для операций администрирования.
3. Откройте короткий URL `http://localhost:8080/hnh` в браузере и запрос будет перенаправлен на оригинальный URL из [create-req.json](./create-req.json).

4. Отправьте `GET` HTTP-запрос с полученным ранее `secretKey` на адрес `http://localhost:8080/admin/abcfjiedis`, в ответ придет статистика использования короткого URL в подобном JSON:
    ```
    {
        "creationDate": "2022-06-26T22:06:11.876+00:00",
        "usageCount": 1
    }
    ```
5. Отправьте `DELETE` HTTP-запрос на `http://localhost:8080/admin/abcfjiedis` чтобы удалить созданное ранее правило перенаправления.

## Задание
Изучите код приложения и напишите (1) модульные и (2) интеграционные (API) тесты на все опубликованные точки HTTP API (см. java package `com.example.shortener.controllers`). 

Необходимо добиться покрытия кода тестами минимум на `70%`. Процент покрытия можно узнать в отчете, формируемом при запуске тестов командой `./gradlew clean test`. Отчет создается в `build/reports/jacoco/index.html` (открыть в браузере). Критерии оценки покрытия заданы в `build.gradle` и их можно автоматически провалидировать командой `./gradlew clean check`.

Требования:
1. Напишите модульные тесты как минимум на RandomKeyGen;
2. Напишите интеграционные тесты на `CreateRedirectController`, `RedirectController`, `AdminController`;
3. Используйте Mockito для исключения внешнего сетевого вызова из `UrlValidator` в тестовых сценариях. См. [Mockito в Spring](https://www.baeldung.com/java-spring-mockito-mock-mockbean) и [Подробнее о Mockito](https://habr.com/ru/articles/444982/)
4. При обнаружении ошибок, зафиксируйте их в отчете о тестировании в разделе Issues->[New issue](./issues/new)
5. Исправьте обнаруженные ошибки и добейтесь "зеленых" тестов.