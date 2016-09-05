# 環境構築

JS Binを使います。

## アクセス先

<https://jsbin.com>

## 初期設定

下記の手順を実施してください。

1. 左上の「×」を押して画面を広くする
2. HTMLの`<body>`タグの中に
```
<div id="app"></div>
<script src="https://fb.me/react-15.1.0.js"></script>
<script src="https://fb.me/react-dom-15.1.0.js"></script>
<script src="https://npmcdn.com/redux@3.5.2/dist/redux.min.js"></script>
<script src="https://wzrd.in/standalone/deep-freeze@latest"></script>
```
を追記する
3. HTMLボタンを押してHTMLタブを消す
4. Consoleボタンを押してConsoleタブを表示する
5. JavaScriptボタンを押してJavaScriptタブを表示する
6. 青字のJavaScriptの右にある三角マークを押して、ドロップダウンから「ES6/Babel」を選択する (このエリアをJavaScriptエリアと呼ぶ)

## サンプルアプリのコード

下記をJavaScriptエリアに入力して、サンプルアプリの骨組みを準備しましょう。

```
const NumBtn = ({ n }) => (
  <button>{n}</button>
);
const PlusBtn = () => (
  <button>+</button>
);
const Result = () => (
  <div>
    結果: <span></span>
  </div>
);
const App = () => (
  <div>
    <div><NumBtn n={1} /><NumBtn n={2} /><NumBtn n={3} /></div>
    <div><NumBtn n={4} /><NumBtn n={5} /><NumBtn n={6} /></div>
    <div><NumBtn n={7} /><NumBtn n={8} /><NumBtn n={9} /></div>
    <div><NumBtn n={0} /><PlusBtn /></div>
    <Result />
  </div>
);
const render = () => ReactDOM.render(<App />, document.getElementById('app'));
render();
```

加算しかできない電卓アプリを考えてください。
上記はReactで表示部分を作ったものです。
本教材ではこのアプリの機能部分をReduxで作っていきます。

## 補足

- ブラウザをリロードすると設定が戻ってしまう場合があるので、その場合は上記手順をやり直す
- 左上のロゴマークを押してKeyboard Shortcutsを参照できる
