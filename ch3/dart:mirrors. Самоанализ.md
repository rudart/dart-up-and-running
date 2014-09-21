# dart:mirrors. Самоанализ

Используйте отражения для анализа структуры работающего приложения. Вы можете исследовать классы, библиотеки, экземпляры классов и т.д.

Примеры в этой статье будут работать с классом `Person`, код которого приведен ниже:

```java
class Person {
  String firstName;
  String lastName;
  int age;
  
  Person(this.firstName, this.lastName, this.age);

  String get fullName => '$firstName $lastName';

  void greet(String other) {
    print('Hello there, $other!');
  }
}
```

## Class Mirrors

Отразите тип, чтобы получить его отражение класса `ClassMirror`.

```java
ClassMirror mirror = reflectClass(Person);

assert('Person' == MirrorSystem.getName(mirror.simpleName));
```

Вы также можете вызвать `runtimeType` для получения типа экземпляра.

```java
var person = new Person('Bob', 'Smith', 33);
ClassMirror mirror = reflectClass(person.runtimeType);

assert('Person' == MirrorSystem.getName(mirror.simpleName));
```

После того, как вы получили `ClassMirror`, вы можете обращаться к конструкторам класса, его полям и т.д. Вот пример получения списка конструкторов класса:

```java
showConstructors(ClassMirror mirror) {
  var constructors = mirror.declarations.values
      .where((m) => m is MethodMirror && m.isConstructor);
  
  constructors.forEach((m) {
    print('У конструктора ${m.simpleName} '
          '${m.parameters.length} параметров.');
  });
}
```

Пример получения списка полей класса:

```java
showFields(ClassMirror mirror) {
  var fields = mirror.declarations.values.where((m) => m is VariableMirror);

  fields.forEach((VariableMirror m) {
    var finalStatus = m.isFinal ? 'final' : 'not final';
    var privateStatus = m.isPrivate ? 'private' : 'not private';
    var typeAnnotation = m.type.simpleName;

    print('The field ${m.simpleName} is $privateStatus and $finalStatus '
          'and is annotated as $typeAnnotation.');
  });
}
```

Для получения всего списка методов `ClassMirror`, обратитесь к [документации](http://api.dartlang.org/dart_mirrors/ClassMirror.html).

## Instance Mirrors

Отразите объект, чтобы получить объект с его отражением типа `InstanceMirror`.

```java
var p = new Person('Bob', 'Smith', 42);
InstanceMirror mirror = reflect(p);
```

Если у вас есть `InstanceMirror` и вы хотите получить отражение объекта, используйте `reflectee`.

```java
var person = mirror.reflectee;
assert(identical(p, person));
```