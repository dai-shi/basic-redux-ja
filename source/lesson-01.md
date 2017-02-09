# 環境構築

CodePenを使います。

## アクセス先

<https://codepen.io>

"New Pen"をクリックしてスタート。

## 初期設定

下記の手順を実施してください。

1. "Change View"で好みのレイアウトに変更する
2. "Settings"のJavaScriptを開いてPreprocessorをBabelにする
3. HTMLのエリアに下記を書く
```
<div id="app"></div>
<script src="https://fb.me/react-15.1.0.js"></script>
<script src="https://fb.me/react-dom-15.1.0.js"></script>
<script src="https://npmcdn.com/redux@3.5.2/dist/redux.min.js"></script>
<script src="https://wzrd.in/standalone/deep-equal@latest"></script>
<script src="https://wzrd.in/standalone/deep-freeze@latest"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/immutable/3.8.1/immutable.min.js"></script>
```
を追記する
4. Consoleボタンを押してConsoleタブを表示する
5. HTMLとCSSのエリアは小さくして良い

## サンプルアプリのコード

下記をJavaScriptエリアに入力して、サンプルアプリの基本版を準備しましょう。

```
console.clear(); // この行は常に残しておくとよい

const initialAppState = {
  inputValue: 0,
  resultValue: 0,
  showingResult: false,
};
const appReducer = (state = initialAppState, action) => {
  if (action.type === 'INPUT_NUMBER') {
    return {
      ...state,
      inputValue: state.inputValue * 10 + action.number,
      showingResult: false,
    };
  } else if (action.type === 'PLUS') {
    return {
      ...state,
      inputValue: 0,
      resultValue: state.resultValue + state.inputValue,
      showingResult: true,
    };
  } else {
    return state;
  }
};
const appStore = Redux.createStore(appReducer);

const NumBtn = ({ n, onClick }) => (
  <button onClick={onClick}>{n}</button>
);

const PlusBtn = ({ onClick }) => (
  <button onClick={onClick}>+</button>
);

const Result = ({ result }) => (
  <div>
    結果: <span>{result}</span>
  </div>
);

const App = ({ store }) => {
  const state = store.getState();
  const result = state.showingResult ? state.resultValue : state.inputValue;
  const onNumClick = (number) => () => store.dispatch({ type: 'INPUT_NUMBER', number });
  return (
    <div>
      <div>
        <NumBtn n={1} onClick={onNumClick(1)} />
        <NumBtn n={2} onClick={onNumClick(2)} />
        <NumBtn n={3} onClick={onNumClick(3)} />
      </div>
      <div>
        <NumBtn n={4} onClick={onNumClick(4)} />
        <NumBtn n={5} onClick={onNumClick(5)} />
        <NumBtn n={6} onClick={onNumClick(6)} />
      </div>
      <div>
        <NumBtn n={7} onClick={onNumClick(7)} />
        <NumBtn n={8} onClick={onNumClick(8)} />
        <NumBtn n={9} onClick={onNumClick(9)} />
      </div>
      <div>
        <NumBtn n={0} onClick={onNumClick(0)} />
        <PlusBtn onClick={() => store.dispatch({ type: 'PLUS' })} />
      </div>
        <Result result={result}/>
    </div>
  );
};

const render = () => ReactDOM.render(<App store={appStore} />, document.getElementById('app'));
render();
appStore.subscribe(render);
```

上記はReact/Reduxで加算しかできない電卓アプリを作ったものです。
本教材ではこのアプリの機能を解説、拡張していきます。

## 補足

- ブラウザをリロードすると設定が戻ってしまう場合があるので、その場合は上記手順をやり直す
- 左上のロゴマークを押してKeyboard Shortcutsを参照できる
- [jshintの設定のヒント](https://github.com/jsbin/jsbin/issues/2792#issuecomment-235769959)
