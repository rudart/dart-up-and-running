#Кодирование и декодирование JSON

JSON строку в объект Dart можно декодировать с помощью *JSON.decode()*:

```
import 'dart:convert' show JSON;

main() {
  // Примечание: Используйте двойные кавычки ("), не ординарные ('),
  // внутри JSON строки. Это строка JSON, не Dart.
  var jsonString = '''
  [
    {"score": 40},
    {"score": 80}
  ]
  ''';

  var scores = JSON.decode(jsonString);
  assert(scores is List);

  var firstScore = scores[0];
  assert(firstScore is Map);
  assert(firstScore['score'] == 40);
}
```

Кодирование объектов Dart в строку JSON происходит с помощью JSON.encode():

```
import 'dart:convert' show JSON;

main() {
  var scores = [
    {'score': 40},
    {'score': 80},
    {'score': 100, 'overtime': true, 'special_guest': null}
  ];

  var jsonText = JSON.encode(scores);
  assert(jsonText == '[{"score":40},{"score":80},'
                     '{"score":100,"overtime":true,'
                     '"special_guest":null}]');
}
```

Только объекты типа int, double, String, bool, null, List, или Map (с строковыми ключами) могут напрямую кодироваться в JSON. List и Map кодируются рекурсивно.

Для тех объектов которые напрямую в JSON не кодируются есть два пути кодировки. Использовать *encode()* вместе со вторым аргументом: функцией, которая возвращает объект, который является непосредственно кодируемым. И второй способ, не передавать второго аргумента и тогда будет вызван метод *toJson()* самого объекта.