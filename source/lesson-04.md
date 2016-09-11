# Reducer

ReducerはReduxの名前のもとにもなっているものですが、
そもそもreduceというのは関数型プログラミングで古くからあるものです。
MapReduceなどが有名です。

Reduxにおけるreducerはstateとactionを入力とし、新しいstateを返す関数です。
このときのstateはすべてimmutabilityの制約のもとで扱わなければいけません。
actionはstateを変化させる操作に相当します。

## 電卓アプリの例

```
let state = {
  inputValue: 0,
  resultValue: 0,
  showingResult: false,
};
```

という状態を考えましょう。ここで、1のボタンを押したactionを考えます。

```
const action1 = { type: 'INPUT_NUMBER', number: 1 };
```

`reducer`に期待する動作は下記です。

```
state = reducer(state, action1);
// state is { inputValue: 1, resultValue: 0, showingResult: false }
state = reducer(state, action1);
// state is { inputValue: 11, resultValue: 0, showingResult: false }
```

電卓の動きを考えながら進めましょう。

```
const action0 = { type: 'INPUT_NUMBER', number: 0 };
```

というactionも追加で考えます。続けて、reducerを呼び出すと、

```
state = reducer(state, action0);
// state is { inputValue: 110, resultValue: 0, showingResult: false }
state = reducer(state, action0);
// state is { inputValue: 1100, resultValue: 0, showingResult: false }
```

のようになると期待されます。

これを実現するreducerは次のようになります。

```
const reducer = (state, action) => {
  if (action.type === 'INPUT_NUMBER') {
    const newValue = state.inputValue * 10 + action.number;
    return { ...state, inputValue: newValue };
  } else {
    return state;
  }
};
```

これはまだresultを考慮していないので、機能としては不十分ですが、
上記の期待する動作は満たします。

## テスト

reducerが正しく動いているかはテストすることができます。
上記の期待する動作について結果が正しいか調べます。
実際はアサーションライブラリを用いますが、今回はconsole.logで判定結果を表示します。

```
let state = {
  inputValue: 0,
  resultValue: 0,
  showingResult: false,
};
const action0 = { type: 'INPUT_NUMBER', number: 0 };
const action1 = { type: 'INPUT_NUMBER', number: 1 };

state = reducer(state, action0);
if (state.inputValue === 0 && state.resultValue === 0 && state.showingResult === false) {
  console.log('OK');
} else {
  console.log('NG');
}

state = reducer(state, action1);
if (state.inputValue === 1 && state.resultValue === 0 && state.showingResult === false) {
  console.log('OK');
} else {
  console.log('NG');
}

state = reducer(state, action1);
if (state.inputValue === 11 && state.resultValue === 0 && state.showingResult === false) {
  console.log('OK');
} else {
  console.log('NG');
}

state = reducer(state, action0);
if (state.inputValue === 110 && state.resultValue === 0 && state.showingResult === false) {
  console.log('OK');
} else {
  console.log('NG');
}
```

このようにすると正しく動いていれば、OKと表示されるはずです。

また、immutabilityのテストもできます。

```
const state = ...;
const action = ...;
deepFreeze(state);
deepFreeze(action);
state = reducer(state, action);
```

このようにするとreducerがstateやactionを書き換えていないことを確認できます。

## 課題

1. "+"ボタンの期待する動作をテストとして書く
2. 上記テストを通るようにreducerを実装する
3. (挑戦) immutable.jsを用いて同じ機能を実現する
