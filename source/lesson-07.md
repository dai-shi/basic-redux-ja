# Reducer合成

Reduxライブラリには、storeを作る機能以外にも、
Reducerを合成する機能があります。

## Reducerが一つの場合

Reducerは一つの関数です。もし一つだけですべてのactionに対応しようとすると、

```
const reducer = (state, action) {
  if (action.type === '...') {
    return ...;
  } else if (action.type === '...') {
    return ...;
  } else if (action.type === '...') {
    return ...;
  } else if (action.type === '...') {
    return ...;
  } else if (action.type === '...') {
    return ...;
  } else if (action.type === '...') {
    return ...;
  } else if (action.type === '...') {
...
...
...
```

のようになってしまいます。これを避けるために、処理を分割する必要がでてきます。

## combineReducers

自力で分割することもできますが、Reduxライブラリは`combineReducers`という関数を提供してくれます。

combineReducersを使うにはstateの形が少し限定されます。

```
const state = {
  key1: ...,
  key2: ...,
  key3: ...,
};
```

上記のようにstateがオブジェクトである必要があります。一般的にはstateは必ずしもオブジェクトである必要はなく、配列でも数値でもよいのです。

このとき、`key1`, `key2`, `key3`の値、すなわちstateの一部分を更新するreducerをそれぞれ分けて作成します。

```
const reducer1 = ...;
const reducer2 = ...;
const reducer3 = ...;
```

`reducer1`がstateの`key1`の値を更新するreducerであるとします。他も同様です。この分割された3つのreducerを合成して一つにするには次のようにします。

```
const recuder = Redux.combineReducers({
  key1: reducer1,
  key2: reducer2,
  key3: reducer3,
});
```

`combineReducers`で合成されたreducerはこれまでと同ように使います。すなわち、stateとactionを渡すと新しいstateを返します。

`reducer1`などのサブreducerはすべてのactionで呼ばれるため、自分のstateを変化させないactionが来た場合でもstateを(変化させないで)返す必要があります。

## 具体例

```
const counterReducer = (state = 0, action) => {
  if (action.type === 'INCREMENT') {
    return state + 1;
  } else {
    return state;
  }
};
const textReducer = (state = '', action) => {
  if (action.type === 'SET_TEXT') {
    return action.text;
  } else {
    return state;
  }
};
const reducer = Redux.combineReducers({
  counter: counterReducer,
  text: textReducer,
});
const action1 = { type: 'INCREMENT' };
const action2 = { type: 'SET_TEXT', text: 'abc' };
```

## 課題

1. 上記の具体例の動作を確認する
