#Кодирование и декодирование символов UTF-8

Используя `UTF8.decode()` можно декодировать байты в кодировке UTF-8 в строку Dart:

```
import 'dart:convert' show UTF8;

main() {
  var string = UTF8.decode([0xc3, 0x8e, 0xc3, 0xb1, 0xc5, 0xa3, 0xc3, 0xa9,
                            0x72, 0xc3, 0xb1, 0xc3, 0xa5, 0xc5, 0xa3, 0xc3,
                            0xae, 0xc3, 0xb6, 0xc3, 0xb1, 0xc3, 0xa5, 0xc4,
                            0xbc, 0xc3, 0xae, 0xc5, 0xbe, 0xc3, 0xa5, 0xc5,
                            0xa3, 0xc3, 0xae, 0xe1, 0xbb, 0x9d, 0xc3, 0xb1]);
  print(string); // 'Îñţérñåţîöñåļîžåţîờñ'
}
```

Для конвертации потока UTF-8 байтов в Dart строку указывается `UTF8.decoder` метода Steam `transform()`:

```
inputStream
  .transform(UTF8.decoder)
  .transform(new LineSplitter())
  .listen(
    (String line) { 
      print('Read ${line.length} bytes from stream');
    });
``` 

Для кодирования строки Dart в виде списка UTF-8 байтов используется `UTF8.encoder`:

```
import 'dart:convert' show UTF8;

main() {
  List<int> expected = [0xc3, 0x8e, 0xc3, 0xb1, 0xc5, 0xa3, 0xc3, 0xa9, 0x72,
                        0xc3, 0xb1, 0xc3, 0xa5, 0xc5, 0xa3, 0xc3, 0xae, 0xc3,
                        0xb6, 0xc3, 0xb1, 0xc3, 0xa5, 0xc4, 0xbc, 0xc3, 0xae,
                        0xc5, 0xbe, 0xc3, 0xa5, 0xc5, 0xa3, 0xc3, 0xae, 0xe1,
                        0xbb, 0x9d, 0xc3, 0xb1];

  List<int> encoded = UTF8.encode('Îñţérñåţîöñåļîžåţîờñ');

  assert(() {
    if (encoded.length != expected.length) return false;
    for (int i = 0; i < encoded.length; i++) {
      if (encoded[i] != expected[i]) return false;
    }
    return true;
  });
}
```

##Остальные возможности
Библиотека ` dart:convert`ы предоставляет возможность	конвертировать байты в форматах  ASCII и ISO-8859-1 (Latin1). За подробной информацией можно обратиться к [API документации библиотеки dart:convert](http://api.dartlang.org/dart_convert.html)