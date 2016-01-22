# Q

[Q](http://documentup.com/kriskowal/q/) — библиотека [промисов](PROMISE.md), предоставляющая гораздо больше возможностей, чем нативная реализация Promise.

Список возможностей:

- Отложенные значения (deferred, устаревший стиль): `Q.defer`
- Поддержка множественных промисов: `Q.all, Q.any, Q.spread`
- Создание, разрешение и отклонение промисов: `Q(value), Q.when(value), Q.reject(value)`
- Преобразование свойств в промисы: `Q.get, Q.set`
- Преобразование функций в промисы: `Q.fcall, Q.invoke`
- Преобразование node-коллбэков в промисы: `Q.nfcall, Q.ninvoke`
- Преобразование таймаутов: `Q.timeout`
- Поддерживает интерфейс notify (уведомление о пргрессе длительной асинхронной операции)
- Поддерживает цепной вызов и сквозной вызов ([tapping](https://github.com/kriskowal/q/wiki/API-Reference#promisetaponfulfilled))

Пример для [Node.js](NODEJS.md) приложения:

```js
// Традиционная реализация
function authenticate (req, res, next) {
  User.findOne({ id: req.id }, function (err, user) {
    if (err) {
      return next('not found')
    }

    user.save(function (err, response) {
      // ...
    });
  });
}

// С помощью Q
function authenticate(req, res, next) {
  return Q(req.id)
    .then(function (id) {
       return Q.nfcall(User, 'findOne', id);
    })
    .then(function (user) {
       return Q.ninvoke(user, 'save');
    })
    .then(function (saved) {
       return res.send(201);
    })
    .catch(function (err) {
      return next(err);
    });
}
```
