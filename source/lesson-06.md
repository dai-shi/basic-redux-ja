# Store

application stateを管理する場所がstoreです。
storeはreducerとmiddleware(appendix参照)によって構成され、
内部にstateを持ちます。
storeにactionを入力すると、reducerによってstateが更新されます。
また、storeからstateを取り出すことができます。

## storeを作成する

```
const initialState = {
  inputValue: 0,
  resultValue: 0,
  showingResult: false,
};
const reducer = (state = initialState, action) => {
  if (action.type === 'INPUT_NUMBER') {
    return { ...state, inputValue: state.inputValue * 10 + action.number };
  } else {
    return state;
  }
};
const store = Redux.createStore(reducer);
```

reducerを作成して、それをもとにstoreを作成します。

reducerはstateの初期値にも対応する(stateが与えられない場合初期化する)必要もあります。
ES2015のdefault parametersを使うと上記のように簡単に書けます。

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

Reactの場合は、subscribeしてrenderを呼び出すことで画面を更新します。
このときコンポーネントにstoreを渡すようにしてコンポーネントは
渡されたstoreのみを使って処理するようにします。
このようにすると、グローバル変数を使うことなく、
モジュール性を高めることができます。

## 課題

1. 上記コードの動作を確認する (React版ではない方)
2. "+"ボタン用の機能を追加したreducerに拡張して動作を確認する
