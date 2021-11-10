
API BASE URL = http://127.0.0.1:8000/api

### Авторизация 
* url = /users/login
* method = POST
* parametr = {email: "email", password: "password"} 
* Авторизация требуется для создания, редактирвоания и удаления опроса. А так же возможно прохождение опроса не анонимно. Ниже будут помечены запросы, которые требуют авторизацию.

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

### Получить список всех доступных опросов 
* url = /survey/list
* method = GET
