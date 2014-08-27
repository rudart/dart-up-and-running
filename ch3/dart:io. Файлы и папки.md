# dart:io. Файлы и папки

I/O библиотека позволяет запускаемым в командной строке приложениям читать, записывать файлы и просматривать папки. У вас есть два варианта для чтения содержимого файла: все сразу, или организовать потоковую передачу. Чтение файла занимает память для хранения всего содержимого. Если файл очень большой или при его чтении содержимое каким-то образом обрабатывается Вы должны воспользовать потоковой передачей данных так, как описанно в разделе "[Потоковая передача содержимого файла](https://www.dartlang.org/docs/dart-up-and-running/contents/ch03.html#ch03-streaming-file-contents)".

## Чтение текстовых файлов
C помощью метода *readAsString()* Вы можете прочитать все содержимое файла закодированного в формате UTF-8. Если Вам необходимы определенные строки, Вы можете воспользовать методом *readAsLines()*. В обоих случаях возвращяется объект Future, который предоставляет содержимое файла в виде одной или нескольких строк.

```
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
Побайтовое чтение файла, как в примере ниже, позволяет помещать все байты в список целых чисел. Вызов функции *readAsBytes()* возвращяет объект Future, который вернет резултат когда он станет доступным.

```
import 'dart:io';

main() {
  var config = new File('config.txt');

  config.readAsBytes().then((List<int> contents) {
    print('Весь файл содержит ${contents.length} количество байтов');
  });
}
```

## Обработка ошибок
Для перехвата ошибки, что бы они не приводили к необработанным исключениям, Вы можете зарегистрировать обработчик *catchError*:

```
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
Используейте Stream для быстрого чтения файла. Метод *Listen()* определяет обработчик который будет вызван при поступлении данных (если они есть). Когда поток данных завержится, запустится метод onDone().

```
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
Для записи данных в файл Вы можете использовать [IOSink](http://api.dartlang.org/dart_io/IOSink.html). Объект IOSink можно получить воспользовавшись методом *openWrite()* класса File. Режим в котором обычно работает метод этого объекта *FileMode.WRITE*, полностью переписывает имеющиеся данные в файле.

```
var logFile = new File('log.txt');
var sink = logFile.openWrite();
sink.write('FILE ACCESSED ${new DateTime.now()}\n');
sink.close();
```

Для того что бы данные в файле не переписывались а добавлялись в конце, используется параметр *mode*  - *FileMode.APPEND*:

```
var sink = logFile.openWrite(mode: FileMode.APPEND); 
```

Для записи бинарного код, используется * add(List<int> date)*.

## Просмотр файлов в каталоге
Просмотр файлов и каталогов в папке является асинхронной операцией. Метод List() возвращяет объект типа Stream, к нему можно подключать обработчики событий (метод Listen() ) для получения информации когда в папке находится файл или каталог.

```
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

## Остальные общие функции
Классы File и Directory содержат и другие функциональные возможности, включая следующие:

- Создание файла или каталога: *Create()* для File и Directory
- Удаление файла или каталога: *Delete()* для File и Directory
- Получение длины файла: *Length()* для File
- Получение случайного доступа к файлу: *Open ()* для File

Полный список API для [File](http://api.dartlang.org/io/File.html) и [Directory](http://api.dartlang.org/io/Directory.html)