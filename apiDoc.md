
API BASE URL = http://127.0.0.1:8000/api

### Авторизация 
* url = /users/login
* method = POST
* parametr = {email: "email", password: "password"} 
* Авторизация требуется для создания, редактирвоания и удаления опроса. А так же возможно прохождение опроса не анонимно. Ниже будут помечены запросы, которые требуют авторизацию.

# Управление опросами
## Опросы
### Создание опроса 
* url = /survey/create
* method = POST
* parametr = {name: "name", description: "description", startDate: "startDate", endDate: "endDate"}
* *Авторизация*. После создания, будет получен созданый опрос, с пустым списокм вопросов, а так же уникальный id опроса, для последующего управления созданным опросом.

### Получение данных опроса 
* url = /survey/manage/{idSurvey}
* method = GET
* *Авторизация*. Будут получены последгнии данный опроса со списоком вопросов.

### Редактирование данных опроса 
* url = /survey/manage/{idSurvey}
* method = PUT
* parametr = {name: "name", description: "description", endDate: "endDate"}
* *Авторизация*. После отправки запроса, он будет получен, как, был бы получен при запросе GET

### Удаление опроса 
* url = /survey/manage/{idSurvey}
* method = DELETE
* *Авторизация*. Опрос будет удален. При успешкном удалении, код ответа 204.

## Вопросы
### Удаление опроса 
* url = /survey/{idSurvey}/create
* method = POST
* parametr
  * {ask: "Вопрос?", answerInput: "текстовый ответ на вопрос"}
  * {ask: "Вопрос?", answerListData: [{"answer": "Ответ 1", "validStatus": true}, {"answer": "Ответ 2", "validStatus": false}]}
  * {ask: "Вопрос?", answerListData: [{"answer": "Ответ 1", "validStatus": true}, {"answer": "Ответ 2", "validStatus": true}], isMulti: true}
* *Авторизация*. В ответ будет получен созданый вопрос. При создании вопроса с выбором, то нужно минимум 2 овтета и должен быть один верный ответ и только, но если параметр isMulti установить в true, то можно указать несколько верныхъ ответов.

### Получить список всех доступных опросов 
* url = /survey/list
* method = GET
