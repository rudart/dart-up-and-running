# Вспомогательные классы

Ядро содержит различные вспомогательные классы, помогающие при сортировке, отображении значений, в итерациях.

## Сравнение объектов

Реализуйте интерфейс [Comparable](http://api.dartlang.org/dart_core/Comparable.html) для того, чтобы показать, что объект может сравниваться с другим объектом (обычно это используется при сортировке). Метод `compareTo()` должен возвращать значение:

* `< 0`, если сравниваемый объект меньше того, с которым его сравнивают;
* `0`, если объекты равны;
* `> 0`, если сравниваемый объект больше того, с которым его сравнивают.

```java
class Line implements Comparable {
  final length;
  const Line(this.length);
  int compareTo(Line other) => length - other.length;
}

main() {
  var short = const Line(1);
  var long = const Line(100);
  assert(short.compareTo(long) < 0);
}
```

## Реализация карты ключей

Каждый объект в Dart автоматически получает целочисленный хэш-код, который может быть использован как ключ в карте (Map). Тем не менее, вы можете переопределить геттер `hashCode` для генерации собственного хэш-кода. Если вы переопределите геттер `hashCode`, то нужно будет также перекрыть оператор `==`. Объекты равны (при сравнении через `==`) тогда, когда у них одинаковый `hashCode`. Хэш код не должен быть уникальным, но он должен быть хорошо распределен.

```java
class Person {
  final String firstName, lastName;

  Person(this.firstName, this.lastName);

  // Переопределим хэш-код по алгоритму из Effective Java, Глава 11.
  int get hashCode {
    int result = 17;
    result = 37 * result + firstName.hashCode;
    result = 37 * result + lastName.hashCode;
    return result;
  }

  // Также мы должны переопределить ==, когда переопределяем hashCode.
  bool operator==(other) {
    if (other is! Person) return false;
    Person person = other;
    return (person.firstName == firstName && person.lastName == lastName);
  }
}

main() {
  var p1 = new Person('bob', 'smith');
  var p2 = new Person('bob', 'smith');
  var p3 = 'not a person';
  assert(p1.hashCode == p2.hashCode);
  assert(p1 == p2);
  assert(p1 != p3);
}
```

## Итерации

Классы [Iterable](http://api.dartlang.org/dart_core/Iterable.html) и [Iterator](http://api.dartlang.org/dart_core/Iterator.html) поддерживают циклы `for-in`. Расширьте (если это возможно) или реализуйте интерфейс Iterable всякий раз, когда вам нужно создать класс, который будет предоставлять итератор для использования в цикле `for-in`.

```java
class Process {
  // представление процесса...
}

class ProcessIterator implements Iterator<Process> {
  Process current;
  bool moveNext() {
    return false;
  }
}

// Выдуманный класс, который будет проходиться по всем процессам.
// Расширяет подкласс Iterable.
class Processes extends IterableBase<Process> {
  final Iterator<Process> iterator = new ProcessIterator();
}

main() {
  // Iterable-объекты могут использоваться в for-in.
  for (var process in new Processes()) {
    // что-то делаем с процессом.
  }
}
```


