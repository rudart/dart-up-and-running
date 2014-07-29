# dart:math. Math и Random

Библиотека Math предоставляет полезный функционал, например, синус и косинус, максимум и минимум; константы, например, `pi` и `e`. Большинство функций в библиотеке Math выполнены как функции верхнего уровня.

Для использования библиотеки Math в приложении, импортируйте `dart:math`. Следующие примеры будут использовать префикс `math`, чтобы отделить функции верхнего уровня и константы библиотеки Math:

```java
import 'dart:math' as math;
```

## Тригонометрия

Библиотека Math предоставляет базовые тригонометрические функции:

```java
// Косинус
assert(math.cos(math.PI) == -1.0);

// Синус
var degrees = 30;
var radians = degrees * (math.PI / 180);
// radians = 0.52359.
var sinOf30degrees = math.sin(radians);

// Обрежем десятичные знаки до двух.
assert(double.parse(sinOf30degrees.toStringAsPrecision(2)) == 0.5);
```

> **Примечание**
> 
> Эти функции использую радианы, а не градусы!

## Максимум и минимум

Библиотека Math содержит методы `max()` и `min()`:

```java
assert(math.max(1, 1000) == 1000);
assert(math.min(1, -1000) == -1000);
```

## Математические константы

Найдите ваши любимые константы - `pi`, `e` и другие, в библиотеке Math:

```java
// Изучите библиотеку Math для получения других констант
print(math.E);     // 2.718281828459045
print(math.PI);    // 3.141592653589793
print(math.SQRT2); // 1.4142135623730951
```

## Случайные числа

Генерация случайных чисел осуществляется с помощью класса [Random](http://api.dartlang.org/dart_math/Random.html). При необходимости можно передать верхнюю границу для генерации:

```java
var random = new math.Random();
random.nextDouble(); // От 0.0 до 1.0: [0, 1)
random.nextInt(10);  // От 0 до 9.
```

Также можно генерировать случайные логические значения:

```java
var random = new math.Random();
random.nextBool();  // true или false
```

## Дополнительная информация

Для получения полного списка методов посмотрите [API библиотеки Math](http://api.dartlang.org/dart_math/index.html). Также изучите документацию для [num](http://api.dartlang.org/dart_core/num.html), [int](http://api.dartlang.org/dart_core/int.html) и [double](http://api.dartlang.org/dart_core/double.html).