# react-reduxについて

Redux自体はReactとは依存関係があるものではなく、Redux単体で動くものです。
例えば、Reduxと通常のJavaScript/DOMを用いてWebアプリケーションを作ることもできます。

ReduxとReactを組み合わせて使う場合でも、自力でstoreの受け渡しを行えば、動作します。
しかしながら、パフォーマンスを上げるには、Reactのレンダリングを制御する必要があります。
具体的には、storeへのsubscribeでアプリ全体を再レンダリングする方法をやめる必要があります。

[react-redux](https://github.com/reactjs/react-redux)はそれを手助けするライブラリです。
Providerとconnectという機能を提供し、ReactとReduxを比較的シームレスにつないでくれます。
ただし、この機能を使うだけでパフォーマンスが上がるわけではなく、store.dispatch()を呼び出すconnectの動きを理解してうまく使う必要があります。

現状では、react-redux以外に著名なbindingライブラリはないですが、
将来的には別のライブラリも登場するかもしれません。
