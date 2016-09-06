# Application state

Reduxはapplication stateを管理するための仕組みを提供します。
application stateとはアプリケーションの状態であり、
単一のデータ構造で表現します。
つまり、JavaScript内で状態を保持する変数を分散して配置せず、
一箇所に集中して管理します。

## application stateの例

例えば、電卓アプリでの状態は、入力中の数字と計算結果の数字があります。
また、どちらの数字を表示しているかのフラグがあります。
これを実現する一例として、下記のようなstateが考えられます。

```
let state = {
  inputValue: 0,
  resultValue: 0,
  showingResult: false,
};
```

他にも様々な実現形態のstateが考えられます。ポイントはアプリに一つの集中したstateであることです。Single source of truthとも言われます。

## stateを更新する

さて、上記の例のstateを更新してみましょう。例えば、task01をcompletedに変更しましょう。Reduxでは、stateはimmutableでなければいけません。immutableにすることで変更を簡単に検知するためです。

まず始めに、immutableにできていない例です。

```
let state = {
  taskList: [
    { name: 'task01', completed: false },
    { name: 'task02', completed: false },
    { name: 'task03', completed: false },
    { name: 'task04', completed: true },
  ],
};
state.taskList[0].completed = true; // mutation
```

上記のようにすると、元のstateオブジェクトが書き換わってしまいます。

immutableにするには下記のようにします。

```
let state = {
  taskList: [
    { name: 'task01', completed: false },
    { name: 'task02', completed: false },
    { name: 'task03', completed: false },
    { name: 'task04', completed: true },
  ],
};
state = Object.assign({}, state, {
  taskList: [
    Object.assign({}, state.taskList[0], { completed: true }),
    ...state.taskList.slice(1),
  ],
});
```

少しややこしいですね。この手間をかけさせることがReduxの本質とも言えます。しかし、このような修正を行う関数やライブラリが充実すればそれほど困らないかもしれません。本教材は本質を学ぶことを重視し、あえてライブラリなどは使わないこととします。

## 課題

1. task02をcompletedに変更する
2. (難問) task05を追加する
3. (難問) task03を削除する
