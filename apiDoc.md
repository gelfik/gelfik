
API BASE URL = http://127.0.0.1:8000/api

### Авторизация 
* url = /users/login
* method = POST
* parametr = {email: "email", password: "password"} 
* Авторизация требуется для создания, редактирвоания и удаления опроса. А так же возможно прохождение опроса не анонимно. Ниже будут помечены запросы, которые требуют авторизацию. После успешной авторизации будет выдан токен и userID.

Авторизация в запросах осущестлвяется посредством добавления заголовка ко всем запросам, которые требуют этого.
Пример: {Authorization: 'Token {LoginToken}'}

## Управление опросами - все запросы требуют авторизации
### Создание опроса 
* url = /survey/create
* method = POST
* parametr = {name: "name", description: "description", startDate: "startDate", endDate: "endDate"}
* *Авторизация*. После создания, будет получен созданый опрос, с пустым списокм вопросов, а так же уникальный id опроса, для последующего управления созданным опросом.

### Получение данных опроса 
* url = /survey/manage/{surveyID}
* method = GET
* *Авторизация*. Будут получены последнии данный опроса со списоком вопросов.

### Редактирование данных опроса 
* url = /survey/manage/{surveyID}
* method = PUT
* parametr = {name: "name", description: "description", endDate: "endDate"}
* *Авторизация*. После отправки запроса, он будет получен, как, был бы получен при запросе GET

### Удаление опроса 
* url = /survey/manage/{idSurvey}
* method = DELETE
* *Авторизация*. Опрос будет удален. При успешном удалении, код ответа 204.

## Управление вопросами - все запросы требуют авторизации
### Создание вопроса
* url = /survey/{surveyID}/create
* method = POST
* parametr
  * {ask: "Вопрос?", answerInput: "текстовый ответ на вопрос"}
  * {ask: "Вопрос?", answerListData: [{"answer": "Ответ 1", "validStatus": true}, {"answer": "Ответ 2", "validStatus": false}]}
  * {ask: "Вопрос?", answerListData: [{"answer": "Ответ 1", "validStatus": true}, {"answer": "Ответ 2", "validStatus": true}], isMulti: true}
* *Авторизация*. В ответ будет получен созданый вопрос. При создании вопроса с выбором, то нужно минимум 2 овтета и должен быть один верный ответ и только, но если параметр isMulti установить в true, то можно указать несколько верныхъ ответов.

### Получение данных вопроса 
* url = /survey/{surveyID}/manage/{askID}
* method = GET
* *Авторизация*. Будут получены последнии данный вопроса.

### Редактирование вопроса
* url = /survey/{surveyID}/manage/{askID}
* method = PUT
* parametr
  * {ask: "Вопрос?", answerInput: "текстовый ответ на вопрос"}
  * {ask: "Вопрос?"}
  * {ask: "Вопрос?", answerListData: [{"answer": "Ответ 1", "validStatus": true}, {"id": {answerID}, "answer": "Ответ 2", "validStatus": false}]}
  * {ask: "Вопрос?", answerListData: [{"id": {answerID}, "answer": "Ответ 1", "validStatus": true}, {"id": {answerID}, "answer": "Ответ 2", "validStatus": false}, {"answer": "Ответ 3", "validStatus": false}]}
  * {ask: "Вопрос?", answerListData: [{"id": {answerID}, "answer": "Ответ 1", "validStatus": true}, {"answer": "Ответ 2", "validStatus": true}], isMulti: true}
* *Авторизация*. При редактировании ответа с выбором, для сохранения старых ответов, обязательно нужно передавать данные совместно с answerID. Так же можно отедельно отправлять параметры (ask, answerInput, answerListData). При отправке answerListData нужно обязательно отправить минимум 2 варианта ответа.

### Удаление вопроса 
* url = /survey/{surveyID}/manage/{askID}
* method = DELETE
* *Авторизация*. Вопрос будет удален. При успешном удалении, код ответа 204.

## Прохождение опросов
### Получить список всех доступных опросов 
* url = /survey/list
* method = GET

### Получить данные выбранного опроса по id
* url = /survey/{surveyID}
* method = GET

### Получить список пройденых опросов определенного юзера по его id
* url = /survey/result/{userID}
* method = GET

### Прохождение опроса
* url = /result/answer
* method = PUT
* parametr
  * {"survey": {surveyID}, "isAnon": false, "user": {userID}, "resultList": [{"ask": {askID}, "answerInput": "TextAnswer"}, {"ask": {askID}, "answerList": [{"id": answerListAnswerID}]}, {"ask": {askID}, "answerList": [{"id": answerListAnswerID}, {"id": answerListAnswerID}]}]}
  * {"survey": {surveyID}, "isAnon": true, "resultList": [{"ask": {askID}, "answerInput": "TextAnswer"}, {"ask": {askID}, "answerList": [{"id": answerListAnswerID}]}, {"ask": {askID}, "answerList": [{"id": answerListAnswerID}, {"id": answerListAnswerID}]}]}
* Что бы пройти анонимно тест, достаточно отправить параметр "isAnon" со значением "true", либо отрпавить параметр "user" с уникальным id пользователя. Число ответов и вопросов в тесте должно совпадать, иначе выйдет уведомление об ошибке.
