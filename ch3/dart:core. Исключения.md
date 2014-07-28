# Исключения

Ядро Dart содержит определения множества полезных исключений и ошибок.

Две наиболее распространенных ошибки:

* **[NoSuchMethodError](http://api.dartlang.org/dart_core/NoSuchMethodError.html)** - бросается, когда объект не содержит метода, который пытаются вызвать.
* **[ArgumentError](http://api.dartlang.org/dart_core/ArgumentError.html)** - бросается, когда метод получил неожиданный аргумент.

Выброс исключений, характерных для конкретного приложения, помогает понять, что за ошибка произошла. Вы можете определить собственные исключения, реализуя интерфейс Exception:

```java
class FooException implements Exception {
  final String msg;
  const FooException([this.msg]);
  String toString() => msg == null ? 'FooException' : msg;
}
```

Для более подробной информации посмотрите статью "[Обзор языка. Исключения](http://rudart.in/up-and-running/93/)" и [документацию по API исключений](http://api.dartlang.org/dart_core/Exception.html).

