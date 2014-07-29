# dart:core. Дата и время

DateTime объекты представляют собой момент времени. Часовой пояс либо UTC, либо местная временная зона.

Для создания объектов DateTime существуют несколько конструкторов:

```java
// Получение текущей даты и времени.
var now = new DateTime.now();

// Создание нового объекта DateTime с местным часовым поясом.
var y2k = new DateTime(2000);   // January 1, 2000

// Задание месяца и дня.
y2k = new DateTime(2000, 1, 2); // January 2, 2000

// Задание даты в формате UTC.
y2k = new DateTime.utc(2000);   // January 1, 2000, UTC

// Задания даты и времени в миллисекундах с Unix эпохи.
y2k = new DateTime.fromMillisecondsSinceEpoch(946684800000, isUtc: true);

// Преобразование ISO 8601 даты.
y2k = DateTime.parse('2000-01-01T00:00:00Z');
```

Свойство `millisecondsSinceEpoch` возвращает количество миллисекунд, прошедших с момента начала “Unix epoch” - January 1, 1970, UTC:

```java
var y2k = new DateTime.utc(2000);           // 1/1/2000, UTC
assert(y2k.millisecondsSinceEpoch == 946684800000);
var unixEpoch = new DateTime.utc(1970); // 1/1/1970, UTC
assert(unixEpoch.millisecondsSinceEpoch == 0);
```

Класс Duration используется для вычилсления разницы между двумя датами и смещении даты вперед или назад:

```java
var y2k = new DateTime.utc(2000);

// Добавление одного года.
var y2001 = y2k.add(const Duration(days: 366));
assert(y2001.year == 2001);

// Удаление 30 дней.
var december2000 = y2001.subtract(const Duration(days: 30));
assert(december2000.year == 2000);
assert(december2000.month == 12);

// Вычисление разницы между двумя датами.
// Возвращается объект разницы.
var duration = y2001.difference(y2k);
assert(duration.inDays == 366); // y2k был високосным годом.
```

> **Предупреждение**
> 
> Использование Duration для сдвига DateTime времени по дням может быть проблематичным, в связи с временными сдвигами (переходом на летнее вреня, на пример). Используйте UTC дни если Вам необходимо сдвинуть дни.

Для полного списка методов обратитесь к API [DateTime](http://api.dartlang.org/dart_core/DateTime.html) и [Duration](http://api.dartlang.org/dart_core/Duration.html).