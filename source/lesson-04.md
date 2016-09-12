# Action

actionはstateを変化させる操作に相当します。
actionはReducerの入力の一つになります。
Reduxではactionはtypeプロパティを持つオブジェクトです。

## actionの例

電卓アプリの例でactionを考えてみましょう。

まず、計算を実行する操作として、"+"ボタンを押すがあります。
これに相当するactionを例えば次のように定義します。

```
const action = { type: 'PLUS' };
```

次に、数字ボタンを押す操作を考えます。

```
const action1 = { type: 'INPUT_NUMBER', number: 1 };
const action2 = { type: 'INPUT_NUMBER', number: 2 };
const action3 = { type: 'INPUT_NUMBER', number: 3 };
```

このようにactionには任意のプロパティを追加することができます。

## Action Creator

actionを作る関数をAction Creatorとして用意する場合もあります。
必須ではありません。小規模関発ではない方が分かりやすいかもしれません。

パラメータを受け取ってactionを返す関数です。例えば、下記のようになります。

```
const doPlus = () => ({
  type: 'PLUS',
});

const inputNumber = (number) => ({
  type: 'INPUT_NUMBER',
  number,
});

console.log(doPlus());
console.log(inputNumber(3));
console.log(inputNumber(4));
console.log(inputNumber(5));
```
