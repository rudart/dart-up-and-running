# dart:io. HTTP-клиенты и сервера

Библиотека `dart:io` содержит классы, которыми можно воспользоваться для доступа к ресурсам HTTP и запуска HTTP-серверов.

## HTTP-сервер

Класс [HttpServer](http://api.dartlang.org/dart_io/HttpServer.html) предоставляет низкоуровневые функции для создания web-сервера. Вы можете настраивать обработку запросов, управлять заголовками, передавать данные в потоке и делать еще много других вещей.

В следующем примере web-сервер может возвращать только простую информацию в текстовом формате. Этот сервер прослушивает порт `8888` адреса `127.0.0.1` (`localhost`), отвечает на запросы по пути `/languages/dart`. Остальные запросы обрабатываются `default`-обработчиком, который возвращает в ответ код `404` (not found).

```java
import 'dart:io';

main() {
  dartHandler(HttpRequest request) {
    request.response.headers.contentType = new ContentType('text', 'plain');
    request.response.write('Dart is optionally typed');
    request.response.close();
  };

  HttpServer.bind('127.0.0.1', 8888).then((HttpServer server) {
    server.listen((request) { 
      print('Got request for ${request.uri.path}');
      if (request.uri.path == '/languages/dart') {
        dartHandler(request);
      } else {
        request.response.write('Ничего не найдено');
        request.response.close();
      }
    });
  });
}
```

Увидеть более полноценный HTTP-сервер можно в [Chapter 5, Walkthrough: Dartiverse Search](https://www.dartlang.org/docs/dart-up-and-running/contents/ch05.html). Там же представлен пример *Dartiverse Search* который реализует web-сервер используя `dart:io` и такие пакеты как [http_server](https://pub.dartlang.org/packages/http_server) и [router](http://pub.dartlang.org/packages/route).

## HTTP-клиент

Класс [HttpClient](http://api.dartlang.org/dart_io/HttpClient.html) поможет подключиться к HTTP-ресурсам из Вашего Dart-приложения на стороне сервера или из командной строки. Вы можете отправлять заголовки, использовать HTTP-методы, а так же читать и записывать данные. Класс `HttpClient` не работает в браузерных приложениях. Для программирования приложений которые будут запускаться в браузере, используйте класс [HttpRequest](http://rudart.in/up-and-running/185/). Вот пример использования `HttpClient`:

```java
import 'dart:io';
import 'dart:convert';

main() {
  var url = Uri.parse('http://127.0.0.1:8888/languages/dart');
  var httpClient = new HttpClient();
  httpClient.getUrl(url)
    .then((HttpClientRequest request) {
      print('Есть запрос');
      return request.close();
    })
    .then((HttpClientResponse response) {
      print('Есть ответ');
      response.transform(UTF8.decoder).toList().then((data) {
        var body = data.join('');
        print(body);
        httpClient.close();
      });
    });
}
```