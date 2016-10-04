# middlewareについて

reduxの魅力の一つにmiddlewareによる機能拡張があります。
middlewareは、reducerを実行する前にactionを書き換えたり、
actionを発行したりすることができます。
middlewareの適用には[applyMiddleware](http://redux.js.org/docs/api/applyMiddleware.html)を使います。

middlewareは自分で作成することもできますが、
既に様々なものが公開されており、再利用することができます。

ReduxのドキュメントにはLoggingのmiddlewareの例が紹介されています。
多くのケースでmiddlewareが必須になるのはAPI呼び出しなどの非同期処理であり、
これには[redux-thunk](https://github.com/gaearon/redux-thunk)が有名ですが、
最近では様々なmiddlewareが提案されています。
- [redux-saga](https://github.com/yelouafi/redux-saga)
- [redux-observable](https://redux-observable.js.org)
- [redux-logic](https://github.com/jeffbski/redux-logic)
- [redux-loop](https://github.com/redux-loop/redux-loop)

middlewareについては[http://redux.js.org/docs/advanced/Middleware.html](公式ページの解説)を一度見ておくとよいでしょう。
