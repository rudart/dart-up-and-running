# dart:html. Использование HTTP Resources из HttpRequest
Ранее известный как XMLHttpRequest, класс HttpRequest предоставляет вашим приложениям доступ к Http ресурсам. Традиционно AJAX приложения очень широко используют HttpRequest. Используя HttpRequest можно динамически подгружать данные JSON или любой другой ресурс с сервера.

В следующих примерах предпологается что все ресурсы загружаются с тогоже сервера на котором размещен сам скрипт. Для обеспечения безопасности, объекты класса HttpRequest не могут свободно подгружать ресурсы размещенные на серверах отличающихся от сервера на котором размещено само приложение. Если Вам нужно получить ресурсы которые находятся на стороннем сервере, Вам необходимо либо использовать технику JSONP или настроить CORS на удаленных серверах.

## Получение данных с сервера
Статичный метод getString() объекта HttpRequest это самый простой способ получить данные с сервера. Используюя метод then() после getString() можно указать функцию которая получит данные для обработки.

```
import 'dart:html';
import 'dart:async';

// Файл в формате JSON расположенные в том же месте где и сам скрипт.
var uri = 'data.json';

main() {
  // Чтение файла  JSON.
  HttpRequest.getString(uri).then(processString);
}

processString(String jsonText) {
  parseText(jsonText);
}
```
Указанная функция(в примере выше это processString) сработает когда данные будут успешно извлечены. В этом случае файл JSON загружен динамически. Информация о JSON API находится в разделе под названием ["кодирование и декодирование JSON"](https://www.dartlang.org/docs/dart-up-and-running/contents/ch03.html#ch03-json).

Метод catchError() добавленый после вызова then() позволяет назначать обработчик ошибок:
```
...
HttpRequest.getString(uri)
    .then(processString)
    .catchError(handleError);
...
handleError(error) {
  print('Ой-ой, произошла ошибка.');
  print(error.toString());
}
```

Если Вам нужно передать в HttpRequest не только данные в виде строки Вы можете использовать статичный метод request() вместо getString(). Вот пример чтения XML данных:
```
import 'dart:html';
import 'dart:async';

// Файл в формате XML расположенные в том же месте где и сам скрипт.
var xmlUri = 'data.xml';

main() {
  // Чтение XML файла.
  HttpRequest.request(xmlUri)
      .then(processRequest)
      .catchError(handleError);
}

processRequest(HttpRequest request) {
  var xmlDoc = request.responseXml;
  try {
    var license = xmlDoc.querySelector('license').text;
    print('License: $license');
  } catch(e) {
    print('$xmlUri XML не правильного формата.');
  }
}
...
```

Вы также можете использовать полный набор API для более интересных случаев. Например, вы можете установить произвольные заголовки.
Основная цепочка вызовов для использования всего разнообразия API объекта HttpRequest выглядит следующим образом:

-   Создание объекта HttpRequest
-   Обозначение метода запроса GET или POST к данным по URL
-   Прикрепление обработчика событий
-   Отправка запроса

На пример:
```
import 'dart:html';
...
var httpRequest = new HttpRequest()
    ..open('POST', dataUrl)
    ..onLoadEnd.listen((_) => requestComplete(httpRequest))
    ..send(encodedData);
```