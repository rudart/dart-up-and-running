# dart:mirrors. Символы

Механизм отражений представляет имена деклараций Dart (классов, полей и т.п.) через объекты класса [Symbol](http://api.dartlang.org/dart_core/Symbol.html). Символы работают по всему коду и даже там, где имена были изменены из-за минимизации.

Используйте литерал символа (`#`), если вам заранее известно его имя. Таким образом, при многократном использовании одного символа идет обращение к одному каноническому экземпляру. Если имя символа изменяется во время работы программы, тогда используйте конструктор `Symbol`:

```java
import 'dart:mirrors';

// Если имя известно во время компиляции
const className = #MyClass;

// Если имя символа меняется
var userInput = askUserForNameOfFunction();
var functionName = new Symbol(userInput);
```

В ходе минимизации компилятор может заменить имя символа другим (более коротким) именем. Для перевода символа обратно в строку используйте метод `MirrorSystem.getName()`. Этот метод вернет верное имя, несмотря на то, что код был минимизирован:

```java
import 'dart:mirrors';

const className = #MyClass;
assert('MyClass' == MirrorSystem.getName(className));
```