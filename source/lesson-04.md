# Reducer

ReducerはReduxの名前のもとにもなっているものですが、そもそもreduceというのは関数型プログラミングで古くからあるものです。MapReduceなどが有名です。

Reduxにおけるreducerはstateとactionを入力とし、新しいstateを返す関数です。このときのstateはすべてimmutableとして扱わなければいけません。actionはstateを変化させる操作に相当します。

## 簡単な例

```
let state = { counter: 0 };
```

という状態を考えましょう。このカウンタを増加させるactionをINCREMENTとします。

```
const action = { type: 'INCREMENT' };
```

このように、actionはtype属性を持ったオブジェクトで表現します。

`reducer`に期待する動作は下記です。

```
let state = { counter: 0 };
const action = { type: 'INCREMENT' };
state = reducer(state, action);
// state is { counter: 1 }
```

さて、ここでimmutabilityを忘れていると、

```
const reducer = (state, action) => {
  if (action.type === 'INCREMENT') {
    state.counter++;
  }
  return state;
};
```

などとしてしまうかもしれません。しかしこれはstateのオブジェクトを書き換えているので正しくありません。

```
const reducer = (state, action) => {
  if (action.type === 'INCREMENT') {
    return Object.assign({}, state, { counter: state.counter + 1 });
  } else {
    return state;
  }
};
```

のようにするのが正しいです。immutabilityを忘れないようにしましょう。

上記の例で3行目は、`Object.assign`を使わず、

```
    return { counter: state.counter + 1 };
```

でよいと考えるかもしれません。今回の例ではそれでも問題とはなりませんが、application stateには様々な情報が含まれますので、実際には必要な部分だけを更新し、それ以外の部分は変更しないようにする必要があります。よって、`Object.assign`は必須になります。

## テスト

reducerが正しく動いているかはテストすることができます。

```
const state = { counter: 0 };
const action = { type: 'INCREMENT' };
const newState = reducer(state, action);
if (newState.counter === 1) {
  console.log('OK');
} else {
  console.log('NG');
}
```

このようにすると正しく動いていれば、OKと表示されるはずです。

また、immutabilityのテストもできます。

```
const state = { counter: 0 };
const action = { type: 'INCREMENT' };
deepFreeze(state);
deepFreeze(action);
const newState = reducer(state, action);
if (newState.counter === 1) {
  console.log('OK');
} else {
  console.log('NG');
}
```

このようにするとreducerがstateやactionを書き換えていないことが分かります。

## 課題

1. INCREMENTのテストパターンを増やして動作を確認する
2. (難問) DECREMENTを実装する
3. (難問) reducer内でわざとstateを書き換えテストが失敗することを確認する
