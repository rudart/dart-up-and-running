# URI

Класс [Uri](http://api.dartlang.org/dart_core/Uri.html) предоставляет функции кодирования и декодирования строк для использования их в URI (также известны как *URL*). Эти функции ищут специальные символы URI (например, `&` или `=`). Также класс Uri разбирает и предоставляет компоненты URI - хост, порт, схему и тому подобное.

## Кодирование и декодирование URI

Для кодирования и декодирования всех символов *кроме* тех, которые являются специальными в URI (такие как `/`, `:`, `&`, `#`), используйте методы `encodeFull()` и `decodeFull()`. 

```java
main() {
  var uri = 'http://example.org/api?foo=some message';
  var encoded = Uri.encodeFull(uri);
  assert(encoded == 'http://example.org/api?foo=some%20message');

  var decoded = Uri.decodeFull(encoded);
  assert(uri == decoded);
}
```

Обратите внимание, что только пробел между `some` и `message` был закодирован.

## Кодирование и декодирование компонентов URI

Для кодирования и декодирования всех символов строки, которые имеют особое значение в URI, включая (но не ограничиваясь) `/`, `&` и `:`, используйте методы `encodeComponent()` и `decodeComponent()`.

```java
main() {
  var uri = 'http://example.org/api?foo=some message';
  var encoded = Uri.encodeComponent(uri);
  assert(encoded == 'http%3A%2F%2Fexample.org%2Fapi%3Ffoo%3Dsome%20message');

  var decoded = Uri.decodeComponent(encoded);
  assert(uri == decoded);
}
```

Обратите внимание, что все спецсимволы были закодированы. Например, `/` был закодирован в `%2F`.

## Парсинг URI

Если у вас есть объект URI или строка с URI, то вы можете получить части этого URI, например `path`. Для создания URI из строки, используйте статический метод `parse()`.

```java
main() {
  var uri = Uri.parse('http://example.org:8080/foo/bar#frag');

  assert(uri.scheme   == 'http');
  assert(uri.host     == 'example.org');
  assert(uri.path     == '/foo/bar');
  assert(uri.fragment == 'frag');
  assert(uri.origin   == 'http://example.org:8080');
}
```

Для более подробной информации посмотрите [документацию по API Uri](http://api.dartlang.org/dart_core/Uri.html).

## Построение URI

Вы можете создать URI из отдельных частей, используя конструктор `Uri()`.

```java
main() {
  var uri = new Uri(scheme: 'http', host: 'example.org',
                    path: '/foo/bar', fragment: 'frag');
  assert(uri.toString() == 'http://example.org/foo/bar#frag');
}
```