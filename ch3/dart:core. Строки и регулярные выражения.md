# Строки и регулярные выражения

Строки в Dart являются неизменной последовательностью символов UTF-16. В статье "[Обзор языка. Встроенные типы данных](http://rudart.in/up-and-running/70/#strings)" содержится дополнительная информация по ним. Вы можете использовать регулярные выражения (объекты RegExp) для поиска внутри строк и замены частей строк.

Класс `String` определяет многие методы для работы со строками, например, `split()`, `contains()`, `startsWith()`, `endsWith()`.

## Поиск внутри строки

Вы можете найти конкретные места вхождения нужного шаблона внтури строки, а также проверить, соответствует ли начало или конец строки определенному шаблону. Например:

```java
// Проверим, содержит ли строка другую строку ('odd')
assert('Never odd or even'.contains('odd'));

// Проверим, начинается ли строка с 'Never'
assert('Never odd or even'.startsWith('Never'));

// Проверим, заканчивается ли строка 'even'
assert('Never odd or even'.endsWith('even'));

// Найдем позицию первого вхождения 'odd' в строку
assert('Never odd or even'.indexOf('odd') == 6);
```

## Извлечение данных из строки

Вы можете получить отдельный символ строки как строку или как число. А точнее, на самом деле вы получите блоки UTF-16. Символы с большим номером, такие как символ скрипичного ключа `'\u{1D11E}'` состоят из двух блоков UTF-16.

Также вы можете извлекать подстроки или разбивать строку на массив подстрок:

```java
// Извлечение подстроки
assert('Never odd or even'.substring(6, 9) == 'odd');

// Разбиваем строку по шаблону (по пробелу в данном случае).
var parts = 'structured web apps'.split(' ');
assert(parts.length == 3);
assert(parts[0] == 'structured');

// Получение блока UTF-16 как строки (по индексу)
assert('Never odd or even'[0] == 'N');

// Используйте split() с пустой строкой в качестве параметра
// для получения списка всех блоков как строк.
for (var char in 'hello'.split('')) {
  print(char);
}

// Получаем все блоки UTF-16 из которых состоит строка.
var codeUnitList = 'Never odd or even'.codeUnits.toList();
assert(codeUnitList[0] == 78);
```

## Перевод строк в строчные или прописные

Вы можете легко конвертировать строку в строку со всеми прописными или со всеми строчными символами:

```java
// В прописные
assert('structured web apps'.toUpperCase() == 'STRUCTURED WEB APPS');

// В строчные
assert('STRUCTURED WEB APPS'.toLowerCase() == 'structured web apps');
```

> **Примечание**
>
> Этот метод работает не со всеми языками.

## Обрезка строк и пустые строки

Для удаления всех ведущих и завершающих пробельных символов используйте метод `trim`. Для того, чтобы проверить, является ли строка пустой (нулевой длины) используйте `isEmpty`.

```java
// Обрезаем строку
assert('  hello  '.trim() == 'hello');

// Проверим, является ли строка пустой
assert(''.isEmpty);

// Строка с одним пробелом не является пустой
assert(!'  '.isEmpty);
```

## Замена части строки

Строки являются неизменяемыми объектами. Это означает, что вы можете создать строку, но не можете ее изменить. Если вы внимательно изучите [документацию по строкам](http://api.dartlang.org/dart_core/String.html), то заметите, что ни один из методов на самом деле не изменяет строку. Например, метод `replaceAll()` возвращает новую строку, без изменения старой.

```java
var greetingTemplate = 'Hello, NAME!';
var greeting = greetingTemplate.replaceAll(new RegExp('NAME'), 'Bob');

assert(greeting != greetingTemplate); // greetingTemplate не изменился.
```

## Построение строки

Вы можете использовать `StringBuffer` для того, чтобы создать строку программно. `StringBuffer` не создает новую строку до тех пор, пока не будет вызван метод `toString()`. Метод `writeAll()` имеет второй необязательный параметр, который определяет разделитель (в примере ниже это пробел):

```java
var sb = new StringBuffer();

sb..write('Используем StringBuffer ')
  ..writeAll(['для', 'эффективного', 'создания', 'строки'], ' ')
  ..write('.');

var fullString = sb.toString();

assert(fullString ==
    'Используем StringBuffer для эффективного создания строки.');
```

## Регулярные выражения

Класс `RegExp` дает похожие возможности с регулярными выражениями в JavaScript. Используйте регулярные выражения для эффективного поиска по строкам.

```java
// Регулярное выражение для одного или нескольких цифровых символов.
var numbers = new RegExp(r'\d+');

var allCharacters = 'llamas live fifteen to twenty years';
var someDigits = 'llamas live 15 to 20 years';

// contains() может принимать регулярное выражение.
assert(!allCharacters.contains(numbers));
assert(someDigits.contains(numbers));

// Заменим каждое совпадение другой строкой.
var exedOut = someDigits.replaceAll(numbers, 'XX');
assert(exedOut == 'llamas live XX to XX years');
```

Также вы можете работать с классом `RegExp` напрямую. Класс `Match` дает доступ к совпадениям с регулярным выражением.

```java
var numbers = new RegExp(r'\d+');
var someDigits = 'llamas live 15 to 20 years';

// Проверим, есть ли совпадения с регулярным выражением.
assert(numbers.hasMatch(someDigits));

// Пройдемся по всем совпадениям.
for (var match in numbers.allMatches(someDigits)) {
  print(match.group(0)); // сначала выведем 15, затем 20
}
```

## Дополнительная информация

Посмотрите [документацию по строкам](http://api.dartlang.org/dart_core/String.html) для получения полного списка методов. Также посмотрите документацию к [StringBuffer](http://api.dartlang.org/dart_core/StringBuffer.html), [Pattern](http://api.dartlang.org/dart_core/Pattern.html), [RegExp](http://api.dartlang.org/dart_core/RegExp.html) и [Match](http://api.dartlang.org/dart_core/Match.html).
