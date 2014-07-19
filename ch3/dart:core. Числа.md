# dart:core. Числа

Библиотека `dart:core` определяет базовый класс числа, класс целого и дробного числа, которые содержат базовые инструменты для работы с числами.

Вы можете конвертировать строку в целое или дробное число с помощью метода `parse()` класса `int` или `double` соответственно:

```java
// Конвертируем строку в целое число
assert(int.parse('42') == 42);
assert(int.parse('0x42') == 66);
// Конвертируем строку в дробное число
assert(double.parse('0.50') == 0.5);
```

Также вы можете использовать метод `parse()` класса `num`, который конвертирует строку в целое число, а если это будет невозможно - то в дробное:

```java
assert(num.parse('42') is int);
assert(num.parse('0x42') is int);
assert(num.parse('0.50') is double);
```

Для того, чтобы указать основание системы счисления целого числа, добавьте параметр `radix`:

```java
assert(int.parse('42', radix: 16) == 66);
```

Используйте метод `toString()` (он опеределен в [Object](http://api.dartlang.org/dart_core/Object.html)) для конвертации целого или дробного числа в строку. Для того, чтобы указать число десятичных символов, используйте метод `toStringAsFixed()`, который определен в `num`. Для указания количества значащих символов после запятой используйте метод `toStringAsPrecision()`.

```java
// Конвертируем целое число в строку
assert(42.toString() == '42');

// Конвертируем дробное число в строку
assert(123.456.toString() == '123.456');

// Укажем количество символов после запятой
assert(123.456.toStringAsFixed(2) == '123.46');

// Укажем количество значащих символов после запятой
assert(123.456.toStringAsPrecision(2) == '1.2e+2');
assert(double.parse('1.2e+2') == 120.0);
```

Для получения дополнительной информации посмотрите документацию для типов [int](http://api.dartlang.org/dart_core/int.html), [double](http://api.dartlang.org/dart_core/double.html) и [num](http://api.dartlang.org/dart_core/num.html). Также почитайте статью "[dart:math - Math and Random](https://www.dartlang.org/docs/dart-up-and-running/contents/ch03.html#ch03-dart-math)".
