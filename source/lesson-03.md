# Application state

Reduxはapplication stateを管理するための仕組みを提供します。application stateとはアプリケーションの状態であり、単一のデータ構造で表現します。つまり、JavaScript内で状態を保持する変数が分散して配置せず、一箇所に集中して管理します。

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

