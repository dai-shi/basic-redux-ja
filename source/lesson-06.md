# Store

application stateを管理する場所がstoreです。storeは内部にreducerを持ち、actionを入力として受け取り、stateを出力します。

## storeを作成する

```
const initialState = {
  inputValue: 0,
  resultValue: 0,
  showingResult: false,
};
const reducer = (state = initialState, action) => {
  if (action.type === 'INPUT_NUMBER') {
    return { ...state, inputValue: state.inputValue * 10 + action.number; };
  } else {
    return state;
  }
};
const store = Redux.createStore(reducer);
```

reducerを作成して、それをもとにstoreを作成します。

reducerにはstateの初期値を設計する必要もあります。
ES2015のdefault parametersを使うと簡単に書けます。

## storeにactionを発行する

上記のコードに続いて、

```
const action = { type: 'INPUT_NUMBER', number: 1 };
store.dispatch(action);
```

とすることでactionを発行します。
これによりreducerが呼ばれて、stateが更新されます。

## storeからstateを取り出す

上記のコードに続いて、

```
const state = store.getState();
```

とするとstateを取得できます。
これはその時点のstateであり変化しないものなので、
どこかに保存しておいても構いません。
実際に、Redux DevToolsではstateを保存して再度プレイバックすることができます。

## storeにリスナーを登録する

stateを取得したいのは更新されたタイミングです。そこでリスナーを登録して更新を通知してもらう方法が用意されています。

```
store.subscribe(() => {
  const state = store.getState();
  console.log('got state', state);
});

store.dispatch({ type: 'INPUT_NUMBER', number: 1 });
store.dispatch({ type: 'INPUT_NUMBER', number: 2 });
store.dispatch({ type: 'INPUT_NUMBER', number: 3 });
```

Reactの場合は、例えば単純には次のようにすることになります。

```
let globalState = {}; // not suitable for real development
store.subscribe(() => {
  globalState = store.getState();
  render(); // assuming we defined this to render all React components
});
```

## 課題

1. 上記コードの動作を確認する
2. "+"ボタン用の機能を追加したreducerに拡張して動作を確認する
3. (挑戦) ReactのコンポーネントでinputValueを表示する
