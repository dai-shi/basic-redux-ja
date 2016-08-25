# Action

Reducerの入力はstateとactionです。actionはstateを変化させる操作に相当します。
Reduxではactionはtypeプロパティを持つオブジェクトであるというだけです。

## actionの例

様々なactionの例を見てみましょう。ここにあるものは典型的なものだけであり、発想を豊かにすれば、様々な可能性があります。

```
const action01 = { type: 'INCREMENT' };
const action02 = { type: 'COUNT_UP', count: 3 };
const action03 = { type: 'CHANGE_TEXT', text: 'foo' };
const action04 = { type: 'CLICK_POINT', x: 321, y: 123 };
const action05 = { type: 'UPDATE_DOCUMENT', doc: { title: 'abc', content: '...' };
```

## Reducer再び

COUNT_UPにも対応させたreducerを見てみましょう。

```
let state = { counter: 0 };
const action = { type: 'COUNT_UP', count: 3 };

const reducer = (state, action) => {
  if (action.type === 'INCREMENT') {
    return Object.assign({}, state, { counter: state.counter + 1 });
  } else if (action.type === 'COUNT_UP') {
    return Object.assign({}, state, { counter: state.counter + action.count });
  } else {
    return state;
  }
};

state = reducer(state, action);
console.log(state);
```

## Action Creator

Actionを作る関数をAction Creatorとして用意する場合もあります。必須ではありません。小規模関発ではない方が分かりやすいかもしれません。

パラメータを受け取ってactionを返す関数です。例えば、下記のようになります。

```
const countUp = (count) => ({
  type: 'COUNT_UP',
  count,
});

console.log(countUp(3));
```

## 課題

1. 「いいね」ボタンを置した際のactionを創作してみる
2. (難問) stateを定義して上記actionを処理するreducerを書く
3. (難問) reducerの動作テストをする
