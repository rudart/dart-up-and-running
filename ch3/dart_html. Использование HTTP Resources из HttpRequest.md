# dart:html. Использование HTTP Resources из HttpRequest

Ранее известный как `XMLHttpRequest`, класс `HttpRequest` предоставляет вашим приложениям доступ к Http-ресурсам. Традиционно AJAX-приложения очень широко используют HttpRequest. Используя `HttpRequest`, можно динамически подгружать данные JSON или любой другой ресурс с сервера.

В следующих примерах предполагается что все ресурсы загружаются с того же сервера на котором размещен сам скрипт. Для обеспечения безопасности, объекты класса `HttpRequest` не могут свободно подгружать ресурсы размещенные на серверах отличающихся от сервера на котором размещено само приложение. Если Вам нужно получить ресурсы которые находятся на стороннем сервере, Вам необходимо либо использовать технику JSONP или настроить CORS на удаленных серверах.

## Получение данных с сервера

Статичный метод `getString()` объекта `HttpRequest` это самый простой способ получить данные с сервера. Используя метод `then()` после `getString()` можно указать функцию которая получит данные для обработки.

```java
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

Указанная функция(в примере выше это `processString`) сработает когда данные будут успешно извлечены. В этом случае файл JSON загружен динамически. Информация о JSON API находится в разделе под названием ["кодирование и декодирование JSON"](https://www.dartlang.org/docs/dart-up-and-running/contents/ch03.html#ch03-json).

Метод `catchError()` добавленный после вызова `then()` позволяет назначать обработчик ошибок:

```java
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

Если Вам нужен доступ к `HttpRequest`, а не только к полученным данным в виде строки Вы можете использовать статичный метод `request()` вместо `getString()`. Вот пример чтения XML данных:

```java
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

Основная цепочка вызовов для использования всего разнообразия API объекта `HttpRequest` выглядит следующим образом:

1. Создание объекта `HttpRequest`
2. Обозначение метода запроса `GET` или `POST` к данным по URL
3. Прикрепление обработчика событий
4. Отправка запроса

Например:

```java
import 'dart:html';
...
var httpRequest = new HttpRequest()
    ..open('POST', dataUrl)
    ..onLoadEnd.listen((_) => requestComplete(httpRequest))
    ..send(encodedData);
```

## Отправка данных на сервер

Объект `HttpRequest` способен отправлять запросы на сервер, используя метод HTTP POST. К примеру, Вы можете динамически отправлять данные в обработчик формы. Еще одним распространенным приемом может быть отправка данных в формате JSON в RESTful-сервисы.

Для передачи данных обработчику формы требуется предоставить пары имя-значение в формате URI строки. (Информация о классе URI находится в ["URI"](http://rudart.in/up-and-running/152/) разделе). Также, для отправки форм, Вы должны установить заголовок `Content-Type` в `application/x-www-form-urlencode`.

```java
import 'dart:html';

String encodeMap(Map data) {
  return data.keys.map((k) {
    return '${Uri.encodeComponent(k)}=${Uri.encodeComponent(data[k])}';
  }).join('&');
}

loadEnd(HttpRequest request) {
  if (request.status != 200) {
    print('Ох нет, тут ошибка ${request.status}');
  } else {
    print('Данные были отправлены');
  }
}

main() {
  var dataUrl = '/registrations/create';
  var data = {'dart': 'fun', 'editor': 'productive'};
  var encodedData = encodeMap(data);

  var httpRequest = new HttpRequest();
  httpRequest.open('POST', dataUrl);
  httpRequest.setRequestHeader('Content-type', 
                               'application/x-www-form-urlencoded');
  httpRequest.onLoadEnd.listen((e) => loadEnd(httpRequest));
  httpRequest.send(encodedData);
}
```