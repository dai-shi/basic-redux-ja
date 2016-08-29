# Store

application stateを管理する場所がstoreです。storeは内部にreducerを持ち、actionを入力として受け取り、stateを出力します。

## storeを作成する

```
const counterReducer = (state = 0, action) => {
  if (action.type === 'INCREMENT') {
    return state + 1;
  } else {
    return state;
  }
};
const reducer = Redux.combineReducers({
  counter: counterReducer,
});
const store = Redux.createStore(reducer);
```

storeにはstateを初期化する機能はありませんので、
reducerがstateの初期化も担当することになります。
単にreducerにstateが渡されてこなかったときに初期値を設定すればよいです。

## storeにactionを発行する

上記のコードに続いて、

```
const action = { type: 'INCREMENT' };
store.dispatch(action);
```

とすることでactionを発行します。これによりreducerが呼ばれて、stateが更新されます。

## storeからstateを取り出す

上記のコードに続いて、

```
const state = store.getState();
```

とするとstateを取得できます。これはその時点のstateであり変化しないものなので、どこかに保存しておいても構いません。実際、Redux DevToolsではstateを保存して再度プレイバックすることができます。

## storeにリスナーを登録する

stateを取得したいのは更新されたタイミングです。そこでリスナーを登録して更新を通知してもらう方法が用意されています。

```
store.subscribe(() => {
  const state = store.getState();
  console.log('got state', state);
});

store.dispatch({ type: 'INCREMENT' });
store.dispatch({ type: 'INCREMENT' });
store.dispatch({ type: 'INCREMENT' });
```

## 課題

1. 上記コードの動作を確認する
2. サブreducerを追加する
3. (難問) stateの結果をDOMに表示する、HTMLの`<button>`でactionを発行する
