# dart:async. Future
Объекты будущего значения могут встречаться во всех библиотеках Dart, часто возвращяя будущее значение в вызвавший асинхронный метод.

## Основы использования
Метод then() позволяет запланировать какой код будет выполняться после того как будущее значение будет готово. К примеру HttpRequest.getString() вернет будущее значение, так как HTTP запросы могут занимать некоторое время. Использование then(), позволяет выполнять код когда будущее значение станет известным:
```
HttpRequest.getString(url)
  .then((String result) {
    print(result); });
  // Обработка ошибок здесь.
```
Обработка любых ошибок или исключений объектов значение которых будет доступно в будущем происходит с помощью метода catchError().
```
HttpRequest.getString(url)
  .then((String result) {  // функция обратного вызова (callback function)
    print(result); })
  .catchError((e) {
    // Обрабатать или проигнорировать ошибку.
  });
```
Шаблон then().catchError() это асинхронный аналог try-catch.

> **Примечание**
> 
>Важно быть уверенным, что catchError() применяется к результату метода then() - не к результату будущего значения. Иначе catchError() может обработать ошибки только для результата полученного в будущем. Не из обработчика зарегистрированного для then().

## Формирование цепочки нескольких асинхронных методов
Метод then() возвращает будущее значение, и обеспечивает удобный способ запуска нескольких асинхронных функций в определенном порядке. Если функция then() возвращает функцию обратного вызова

Если функция обратного вызова ( callback() ) возвращается методом then() с переданным объектом будущего значения, then() возвращает эквивалент будущего значения. Если callback возвращается с объектом другого типа, then() создаст новый объект Future который будет содержать это значение.

```
Future result = costlyQuery();

return result.then((value) => expensiveWork())
             .then((value) => lengthyComputation())
             .then((value) => print('done!'))
             .catchError((exception) => print('DOH!'));
```
В предыдущем примере методы выполняются в следующем порядке:
1. *costlyQuery()*
2. *expensiveWork()*
3. *lengthyComputation()*

## Ожидание получения нескольких будущих значений
Иногда ваш алгоритм должен вызвать много асинхронных функций и ждать их все, прежде чем продолжить. Используйте статичный метод Future.wait() что бы обеспечить продолжение выполнение кода, только после получение результата вычисления будущего значения.

```
Future deleteDone = deleteLotsOfFiles();
Future copyDone = copyLotsOfFiles();
Future checksumDone = checksumLotsOfOtherFiles();

Future.wait([deleteDone, copyDone, checksumDone]).then((List values) {
  print('Done with all the long steps');
});
```