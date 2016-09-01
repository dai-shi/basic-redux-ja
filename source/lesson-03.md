# Application state

Reduxはapplication stateを管理するための仕組みを提供します。application stateとはアプリケーションの状態であり、単一のデータ構造で表現します。つまり、JavaScript内で状態を保持する変数を分散して配置せず、一箇所に集中して管理します。

## application stateの例

例えば、TODOアプリでタスクのリストを状態として持つ場合、下記のようなstateが考えられます。

```
let state = {
  taskList: [
    { name: 'task01', completed: false },
    { name: 'task02', completed: false },
    { name: 'task03', completed: false },
    { name: 'task04', completed: true },
  ],
};
```

これに加えて、表示中のタスクを状態として持つ場合は、その状態も同じstateで保持します。

```
let state = {
  taskList: [
    { name: 'task01', completed: false },
    { name: 'task02', completed: false },
    { name: 'task03', completed: false },
    { name: 'task04', completed: true },
  ],
  showingTaskName: 'task02',
};
```

これは一例にすぎず、他にも様々なstateが考えられます。ポイントはアプリに一つの集中したstateであることです。Single source of truthとも言われます。

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
