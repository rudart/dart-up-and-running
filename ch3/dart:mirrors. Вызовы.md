# dart:mirrors. Вызовы

Когда у вас есть `InstanceMirror`, Вы можете выполнять методы, вызывать геттеры и сеттеры. Для полного списка методов посмотрите [документацию к InstanceMirror](http://api.dartlang.org/dart_mirrors/InstanceMirror.html).

## Вызов методов

Используйте метод `invoke()` класса `InstanceMirror` для вызова методов объекта. Первый параметр указывает метод, который должен быть выполнен, а второй - список позиционных аргументов метода (подробнее о функциях и методах можно почитать в [этой статье](http://rudart.in/up-and-running/75/)). Необязательный третий параметр позволяет указать именованные параметры.

```java
var p = new Person('Bob', 'Smith', 42);
InstanceMirror mirror = reflect(p);

mirror.invoke(#greet, ['Shailen']);
```

## Вызов геттеров и сеттеров

Используйте методы `getField()` и `setField()` класса `InstanceMirror` для установки значений свойствам объекта.

```java
var p = new Person('Bob', 'Smith', 42);
InstanceMirror mirror = reflect(p);

// Получим значение свойства.
var fullName = mirror.getField(#fullName).reflectee;
assert(fullName == 'Bob Smith');
  
// Установим значение свойства.
mirror.setField(#firstName, 'Mary');
assert(p.firstName == 'Mary');
```