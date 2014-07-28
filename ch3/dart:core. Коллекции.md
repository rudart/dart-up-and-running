#dart:core. Коллекции
Dart изначально содержит API основных коллекций - списков (List), наборов (Set) и хешей (Map).

## Списки (Lists)
В обзоре языка использовался [литерал для создания списка](http://rudart.in/up-and-running/70/#lists), но для этого можно использовать и конструктор `List()`. Также класс List определяет несколько методов для добавления и удаления элементов из списков.

```java
// Использование конструктора List.
var vegetables = new List();

// Или простое использование литерала.
var fruits = ['apples', 'oranges'];

// Добавление в список.
fruits.add('kiwis');

// Добавление нескольких элементов в список.
fruits.addAll(['grapes', 'bananas']);

// Получение длины списка (количество содержащихся элементов).
assert(fruits.length == 5);

// Удаления элемента.
var appleIndex = fruits.indexOf('apples');
fruits.removeAt(appleIndex);
assert(fruits.length == 4);

// Удаление всех элементов в списке.
fruits.clear();
assert(fruits.length == 0);
```

Для поиска индекса объекта в списке используйте метод `indexOf()`:

```java
var fruits = ['apples', 'oranges'];

// Доступ к элементам списка по индексу
assert(fruits[0] == 'apples');

// Найдем индекс элемента массива
assert(fruits.indexOf('apples') == 0);
```

Сортировка списка производится с помощью метода `sort()`. В качестве параметра метода `sort()` вы можете передать функцию сортировки, которая будет сравнивать два объекта. Эта функция должна возвращать значение `< 0`, если первый объект меньше второго; `0` - если первый и второй объект одинаковы; и `> 0` - если первый объект больше второго. В следующем примере используется функция `compareTo()`, которая jопределена в интерфейсе [Comparable](http://api.dartlang.org/dart_core/Comparable.html) и реализуется классом `String`:

```java
var fruits = ['bananas', 'apples', 'oranges'];

// Сортировка списка.
fruits.sort((a, b) => a.compareTo(b));
assert(fruits[0] == 'apples');
```

Тип списка может быть параметризованым, благодаря чему можно указывать какой тип элементов должен содержать каждый конкретный список:

```java
// Этот список должен содержать только строки.
var fruits = new List<String>();

fruits.add('apples');
var fruit = fruits[0];
assert(fruit is String);

// Создается ошибка статического анализа (static analysis warning), число не является строкой.
fruits.add(5);  // Плохо: Выдаст ошибку в режиме проверки
```

За полным списком методов определённых для списка в [документации API](http://api.dartlang.org/dart_core/List.html)


## Наборы (Set)
В Dart наборы представляют собой неупорядоченный набор уникальных элементов. Поскольку элементы не упорядочены, нет возможности получать их по индексу (позиции).

```java
var ingredients = new Set();
ingredients.addAll(['gold', 'titanium', 'xenon']);
assert(ingredients.length == 3);

// Добавление уже имеющегося элемента, не дает результата.
ingredients.add('gold');
assert(ingredients.length == 3);

// Удаление элемента из набора.
ingredients.remove('gold');
assert(ingredients.length == 2);
```

Методы `contains()` и `containsAll()` служат для проверки существования элемента в наборе.
 
 ```java
 var ingredients = new Set();
 ingredients.addAll(['gold', 'titanium', 'xenon']);
 
 // Проверка, содержится ли элемент в наборе.
 assert(ingredients.contains('titanium'));
 
 // Проверка существования всех элементов в наборе.
 assert(ingredients.containsAll(['titanium', 'xenon']));
```
 
 Метод `intersection()` возвращает ряд, который содержит элементы находящиеся в двух других рядах.
 
 ```java
 var ingredients = new Set();
 ingredients.addAll(['gold', 'titanium', 'xenon']);
 
 // Создание ряда из элементов содержащихся в двух других рядах.
 var nobleGases = new Set.from(['xenon', 'argon']);
 var intersection = ingredients.intersection(nobleGases);
 assert(intersection.length == 1);
 assert(intersection.contains('xenon'));
 ```
 
Полный список методов в [документации API](http://api.dartlang.org/dart_core/Set.html)

## Хеш (Map)

Карта, широко известный как словарь или хэш, это неупорядоченный набор пар ключ-значение. В хеше, для облегчения поиска, значение привязывается к ключу. В отличие от JavaScript, объекты Dart не являются хешами.
Для определения хешей можно использовать простой синтаксис литерала, или по традиции использовать конструктор:

```java
// Хеши часто используют строки в качестве ключей.
var hawaiianBeaches = {
  'oahu'       : ['waikiki', 'kailua', 'waimanalo'],
  'big island' : ['wailea bay', 'pololu beach'],
  'kauai'      : ['hanalei', 'poipu']
};

// Хеши могут быть созданы с помощью конструктора.
var searchTerms = new Map();

// Хеши являются параметризованными типами; Вы можете указать, какого типа
// должны быть ключи и значения.
var nobleGases = new Map<int, String>();
```

Для добавления, получения и изменения элементов в хеше, используется обычный синтаксис скобок. Для удаления ключа и его значения используется метод `remove()`.

```java
var nobleGases = { 54: 'xenon' };

// Получения значения по ключу.
assert(nobleGases[54] == 'xenon');

// Проверка, содержится ли ключ в хеше.
assert(nobleGases.containsKey(54));

// Удаления ключа и его значения.
nobleGases.remove(54);
assert(!nobleGases.containsKey(54));
```

Вы можете получить все значения или все ключи хеша:

```java
var hawaiianBeaches = {
  'oahu' : ['waikiki', 'kailua', 'waimanalo'],
  'big island' : ['wailea bay', 'pololu beach'],
  'kauai' : ['hanalei', 'poipu']
};

// Получение всех ключей в виде неупорядоченной коллекции (Iterable).
var keys = hawaiianBeaches.keys;

assert(keys.length == 3);
assert(new Set.from(keys).contains('oahu'));

// Получение всех значений в виде неупорядоченного списка (Iterable).
var values = hawaiianBeaches.values;
assert(values.length == 3);
assert(values.any((v) => v.contains('waikiki')));
```

Для проверки содержится ли ключ в хеше, используется метод `containsKey()`. Так как значения в хеше не могут быть пустыми (`null`), Вы не можете полагаться на простую проверку значения ключа на `null` для определения существования ключа.

```java
var hawaiianBeaches = {
  'oahu' : ['waikiki', 'kailua', 'waimanalo'],
  'big island' : ['wailea bay', 'pololu beach'],
  'kauai' : ['hanalei', 'poipu']
};

assert(hawaiianBeaches.containsKey('oahu'));
assert(!hawaiianBeaches.containsKey('florida'));
```

Метод `putIfAbsent()` используется для добавления значения тогда, и только тогда, когда ключа еще не существует в хеше. Нужно предоставить функцию которая вернет значение.

```java
var teamAssignments = {};
teamAssignments.putIfAbsent('Catcher', () => pickToughestKid());
assert(teamAssignments['Catcher'] != null);
```

Полный список методов содержится в [документации API](http://api.dartlang.org/dart_core/Map.html)

### Дополнительные методы коллекций

Наброы, списки и хеши содержат общие функциональные возможности которые встречаются во многих коллекциях. Некоторые из этих общих функций определяются классом Iterable, который реализуют списки и наборы.


> **Примечание**
> 
> Хоть хеши и не реализуют интерфейс Iterable, вы можете пройтись циклом по его ключам или значениям.

Используйте `isEmpty` для проверки отсутствия элементов в наборе, списке или хеше (т.е. является ли набор, список или хеш пустым).

```java
var teas = ['green', 'black', 'chamomile', 'earl grey'];
assert(!teas.isEmpty);
```

Для того что бы применить функцию к каждому элементу списка, набора или хеша можно использовать метод `forEach()`:

```java
var teas = ['green', 'black', 'chamomile', 'earl grey'];

teas.forEach((tea) => print('I drink $tea'));
```

Когда вы вызываете `forEach()` для хеша, функция должна принимать два аргумента (ключ и значение):

```java
hawaiianBeaches.forEach((k, v) {
  print('I want to visit $k and swim at $v');
  // I want to visit oahu and swim at [waikiki, kailua, waimanalo], etc.
});
```

Iterables предоставляет метод `map()`, который возвращает все результаты в одном объекте:

```java
var teas = ['green', 'black', 'chamomile', 'earl grey'];

var loudTeas = teas.map((tea) => tea.toUpperCase());
loudTeas.forEach(print);
```


> **Примечание**
> 
> Объект возвращаемый методом `map()` типа Iterable это так называемое "ленивое вычисление" (lazily evaluated): функция не вызывается пока вы не выберете элемент из возвращенного объекта.

Для того что бы заставить функцию вызываться сразу для каждого элемента используется `map().toList()` или `map().toSet()`:

```java
var loudTeaList = teas.map((tea) => tea.toUpperCase()).toList();
```

Используйте метод класса Iterable, `where()` , чтобы получить все элементы, которые соответствуют условию. Методы `any()` и `every()` проверяют некоторые элементы или все элементы на соответствие условию.

```java
var teas = ['green', 'black', 'chamomile', 'earl grey'];

// Ромашковый чай не содержик коффеин
bool isDecaffeinated(String teaName) => teaName == 'chamomile';

// Использование where() для поиска элементов которые возвращают true
// из предоставленной функции.
var decaffeinatedTeas = teas.where((tea) => isDecaffeinated(tea));
// или тоже самое teas.where(isDecaffeinated)

// Применение any() для проверки, содержится ли хотя бы одни элемент в коллекции
// соответствующий условию.
assert(teas.any(isDecaffeinated));

// Использования every() для проверки каждого элемента в коллекции на 
// оответствие условию.
assert(!teas.every(isDecaffeinated));
```

Что бы посмотреть полный список методов, обратитесь к документации [Iterable API](http://api.dartlang.org/dart_core/Iterable.html), а так же наборов, списков и хешей.