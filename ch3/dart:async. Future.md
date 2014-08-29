# dart:async. Future

Объекты `Future` могут встречаться во всех библиотеках Dart. Часто такие объекты возвращают асинхронные методы. Когда `Future` *выполнится*, то его значение доступно для использования.

## Основы использования

Метод `then()` позволяет задать код, который запустится после того, как `Future` выполнится. К примеру, `HttpRequest.getString()` возвращает объект `Future`, так как HTTP запросы могут занимать некоторое время. Использование `then()`, позволяет выполнять код когда будущее значение станет известным:

```java
HttpRequest.getString(url)
  .then((String result) {
    print(result); 
  });
  // Обработка ошибок здесь.
```

Используйте метод `catchError()` для обработки любых ошибок или исключений, которые могут возникнуть в объекте `Future`.

```java
HttpRequest.getString(url)
  .then((String result) {  // функция обратного вызова (callback function)
    print(result); 
  })
  .catchError((e) {
    // Обрабатать или проигнорировать ошибку.
  });
```

Шаблон `then().catchError()` это асинхронный аналог `try-catch`.

> **Примечание**
> 
>Обязательно вызовите `catchError()` для результата `then()`, а не для результата оригинального `Future`. `catchError()` обрабатывает ошибки только из оригинального `Future`, а ошибки обработчика `then()` он не обрабатывает, поэтому для обработчика `then()` нужно таке предустмотреть свой `catchError()`.

## Формирование цепочки нескольких асинхронных методов

Метод `then()` возвращает `Future` и обеспечивает удобный способ запуска нескольких асинхронных функций в определенном порядке. Если функция-обработчик `then()` возвращает объект `Future`, тогда `then()` (в котором зарегистрирован этот обработчик) вернет тот объект `Future`, который вернул обработчик. Если функция-обработчик `then()` вернет объект другого типа, тогда `then()` вернет объект `Future`, который будет содержать это значение. 

```java
Future result = costlyQuery();

return result.then((value) => expensiveWork())
             .then((value) => lengthyComputation())
             .then((value) => print('done!'))
             .catchError((exception) => print('DOH!'));
```

В предыдущем примере методы выполняются в следующем порядке:
1. `costlyQuery()`
2. `expensiveWork()`
3. `lengthyComputation()`

## Ожидание выполнения нескольких Future

Иногда ваш код должен вызвать много асинхронных функций и подождать, пока они все выполнятся, прежде чем продолжить. Используйте статичный метод `Future.wait()`, чтобы обеспечить продолжение выполнение кода, только после того, когда все объекты `Future` вернут значение.

```java
Future deleteDone = deleteLotsOfFiles();
Future copyDone = copyLotsOfFiles();
Future checksumDone = checksumLotsOfOtherFiles();

Future.wait([deleteDone, copyDone, checksumDone]).then((List values) {
  print('Done with all the long steps');
});
```