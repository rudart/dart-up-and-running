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