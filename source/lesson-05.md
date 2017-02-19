# Reducer

ReducerはReduxの名前のもとにもなっているものですが、
そもそもreduceというのは関数型プログラミングで古くからあるものです。
MapReduceなどが有名です。

Reduxにおけるreducerはstateとactionを入力とし、新しいstateを返す関数です。
このときのstateはすべてimmutabilityの制約のもとで扱わなければいけません。
actionはstateを変化させる操作に相当します。
また、stateを変化させることができるのはreducerのみで、
reduderは副作用を起こしてはいけないという制約があります。

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

`reducer`が存在するとして呼び出すコードは下記です。

```
state = reducer(state, action1);
if (deepEqual(state, { inputValue: 1, resultValue: 0, showingResult: false })) {
  console.log('OK1');
} else {
  console.log('NG1');
}

state = reducer(state, action1);
if (deepEqual(state, { inputValue: 11, resultValue: 0, showingResult: false })) {
  console.log('OK2');
} else {
  console.log('NG2');
}
```

このコードはテストコードになっており正常に動作すると、"OK"が表示されるはずです。

とりあえず、ダミーのreducerをテストコードの前に追加してみましょう。

```
const reducer = (state, action) => state; // ダミーのreducer
```

これは何もしないreducerです。
この状態ではテストは通らず、"NG"が表示されるでしょう。

電卓の動きを考えながら進めましょう。

```
const action0 = { type: 'INPUT_NUMBER', number: 0 };
```

というactionも追加で考えます。0を押す操作に相当するactionです。
続けて、reducerを呼び出すコードが下記です。

```
state = reducer(state, action0);
if (deepEqual(state, { inputValue: 110, resultValue: 0, showingResult: false })) {
  console.log('OK3');
} else {
  console.log('NG3');
}

state = reducer(state, action0);
if (deepEqual(state, { inputValue: 1100, resultValue: 0, showingResult: false })) {
  console.log('OK4');
} else {
  console.log('NG4');
}
```

同じようにreducerはまだダミーなので"NG"になります。

さて、これを実現するreducerを書いてみましょう。
次のようになります。

```
const reducer = (state, action) => {
  if (action.type === 'INPUT_NUMBER') {
    return { ...state, inputValue: state.inputValue * 10 + action.number };
  } else {
    return state;
  }
};
```

`return {}`で新しいオブジェクトを返しています。
immutabilityの制約のもとではオブジェクトを修正することはできません。

`...state`と書くことで、変更しないプロパティは引き継ぐことができます。

このreducerはまだresultを考慮していないので機能としては不十分ですが、
上記の期待する動作は満たします。

すべて"OK"になるか動作を確認しましょう。

### immutabilityのテスト

上記のテストはimmutabilityの制約を守れているかは確認していません。
immutabilityの確認をするには`deepFreeze`を使います。

```
const state = ...;
deepFreeze(state);

const action0 = ...;
deepFreeze(action0);

const action1 = ...;
deepFreeze(action1);
```

上記のようにstateやactionを作成したらすぐにdeepFreezeで修正を禁止します。
このようにするとreducerがstateやactionを書き換えていないことを確認できます。

上記のテストコードにすべて追加して正しく動作する(すべて"OK"となる)ことを確認しましょう。

一般的にdeepFreezeは処理が重いため、テストコードのみに用い、本番環境では用いません。
本教材では、学習のため、すべてのstateとactionにdeepFreezeをつけて構いません。


### "+"ボタンのaction

"+"ボタンのactionは単純に次のように定義できます。

```
const actionPlus = { type: 'PLUS' };
deepFreeze(actionPlus);
```

## 課題

1. actionPlusを使って期待する動作をテストとして書く
2. 上記テストを通るようにreducerを実装する (正解の一つがサンプルアプリの`appReducer`です)
3. (難問) immutabilityの制約を破るようにreducerを書いて、エラーがでるか確認する

## 補足

Codepenでは通常はdeepFreezeのエラーが無視されてしまうため、
エラーを表示するには下記のように全体をtry-catchで囲うとよいでしょう。

```
try {

  //------------------
  // ここにすべて書く
  //------------------

} catch (e) {
  console.error(e.message);
}
```
