# middlewareについて

reduxの魅力の一つにmiddlewareによる機能拡張があります。
middlewareは、reducerを実行する前にactionを書き換えたり、
actionを発行したりすることができます。
middlewareの適用には[applyMiddleware](http://redux.js.org/docs/api/applyMiddleware.html)を使います。

middlewareは自分で作成することもできますが、
既に様々なものが公開されており、再利用することができます。

ReduxのドキュメントにはLoggingのmiddlewareの例が紹介されています。
middlewareで多くのケースで必要になるのは、API呼び出しなどの同期処理であり、
これには、[redux-thunk](https://github.com/gaearon/redux-thunk)や[redux-saga](https://github.com/yelouafi/redux-saga)が有名です。

[http://redux.js.org/docs/advanced/Middleware.html]
