# dart:io. Файлы и папки

Библиотека I/O позволяет приложениям, запускаемым в командной строке, читать, записывать файлы и просматривать папки. Есть два способа чтения содержимого файла: прочитать весь файл за раз, или организовать потоковую передачу. Чтение файла за раз занимает память для хранения всего содержимого. Если файл очень большой или при его чтении содержимое каким-то образом обрабатывается Вы должны воспользоваться потоковой передачей данных так, как описано в разделе "[Потоковая передача содержимого файла](#_5)".

## Чтение текстовых файлов

C помощью метода `readAsString()` Вы можете прочитать все содержимое файла, закодированного в формате UTF-8. Если Вам необходимо прочесть файл построчно, то можете воспользоваться методом `readAsLines()`. В обоих случаях возвращается объект `Future`, который предоставляет содержимое файла в виде одной или нескольких строк.

```java
import 'dart:io';

main() {
  var config = new File('config.txt');

  // Все содержимое файла записывается в виде одной строки.
  config.readAsString().then((String contents) {
    print('Весь файл содержит ${contents.length} символов');
  });

  // Каждая строка будет помещена на свое место.
  config.readAsLines().then((List<String> lines) {
    print('Весь файл содержит ${lines.length} строк');
  });
}
```

## Чтение бинарных файлов

Побайтовое чтение файла, как в примере ниже, позволяет помещать все байты в список целых чисел. Вызов функции `readAsBytes()` возвращает объект Future, который вернет результат когда он станет доступным.

```java
import 'dart:io';

main() {
  var config = new File('config.txt');

  config.readAsBytes().then((List<int> contents) {
    print('Весь файл содержит ${contents.length} количество байтов');
  });
}
```

## Обработка ошибок

Для перехвата ошибки, чтобы они не приводили к необработанным исключениям, Вы можете зарегистрировать обработчик `catchError`:

```java
import 'dart:io';

main() {
  var config = new File('config.txt');
  config.readAsString().then((String contents) {
    print(contents);
  }).catchError((e) {
    print(e);
  });
}
```

## Потоковая передача содержания файла

Используйте Stream для быстрого чтения файла. Метод `listen()` определяет обработчик, который будет вызван при поступлении данных (если они есть). Когда поток данных завершится, запустится метод `onDone()`.

```java
import 'dart:io';
import 'dart:convert';
import 'dart:async';

main() {
  var config = new File('config.txt');
  Stream<List<int>> inputStream = config.openRead();

  inputStream
    .transform(UTF8.decoder)
    .transform(new LineSplitter())
    .listen(
      (String line) { 
        print('Получено ${line.length} символов из потока');
      },
      onDone: () { print('теперь файл закрыт'); },
      onError: (e) { print(e.toString()); });
}
```

## Запись содержимого в файл

Для записи данных в файл Вы можете использовать [IOSink](http://api.dartlang.org/dart_io/IOSink.html). Объект `IOSink` можно получить воспользовавшись методом `openWrite()` класса `File`. Режим, в котором обычно работает метод этого объекта - **FileMode.WRITE** - полностью переписывает имеющиеся данные в файле.

```java
var logFile = new File('log.txt');
var sink = logFile.openWrite();
sink.write('FILE ACCESSED ${new DateTime.now()}\n');
sink.close();
```

Для того, чтобы данные в файле не переписывались а добавлялись в конце, установите параметр `mode`  в значение **FileMode.APPEND**:

```java
var sink = logFile.openWrite(mode: FileMode.APPEND); 
```

Для записи двоичных данных, используйте `add(List<int> date)`.

## Просмотр файлов в каталоге

Просмотр файлов и каталогов в папке является асинхронной операцией. Метод `list()` возвращает объект `Stream`, к нему можно подключать обработчики событий (метод `listen()`) для получения информации когда в папке находится файл или каталог.

```java
import 'dart:io';
import 'dart:async';

main() {
  var dir = new Directory('/tmp');
  var contentsStream = dir.list(recursive:true);
  contentsStream.listen(
    (FileSystemEntity f) {
      if (f is File) {
        print('Файл найден ${f.path}');
      } else if (f is Directory) {
        print('Найден каталог ${f.path}');
      }
    },
    onError: (e) { print(e.toString()); }
  );
}
```

## Другие полезные функции

Классы `File` и `Directory` содержат и другие возможности, включая (но не ограничиваясь) следующие:

- Создание файла или каталога: `create()` для `File` и `Directory`
- Удаление файла или каталога: `delete()` для `File` и `Directory`
- Получение длины файла: `length()` для `File`
- Получение случайного доступа к файлу: `open()` для `File`

Полный список API для [File](http://api.dartlang.org/io/File.html) и [Directory](http://api.dartlang.org/io/Directory.html)