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

入力中の数値をinputValueに保持し、
計算結果の数値をresultValueに保持し、
showingResultに計算結果を表示するかのフラグを保持します。
たとえば、"1"を入力するとinputValueは1になり、
さらに"2"を入力するとinputValueは12になります。
この状態で"+"を押すとresultValueに12が入りinputValueは0に戻ります。
続いて"3"を入力するとinputValueは3になり、resultValueは12のままです。
最後にもう一度"+"を押すとresultValueは15になり、"12+3"が計算できたことになります。

他にも様々な実現形態のstateが考えられます。ポイントはアプリに一つの集中したstateであることです。Single source of truthとも言われます。

## なぜ一箇所で管理するか

Reactの場合は、すべてのコンポーネントがstatelessになります。
副作用がまったくないため、動作の予測がしやすくなります。
また、stateを切り替えるだけでViewの動作を確認することができます。
Redux DevToolsを使うとタイムマシン風のリプレイが可能です。

## 補足

今回の例におけるapplication stateは一階層のobjectですが、それに限定する必要はありません。下記のようにobjectやarrayが入れ子になっても問題ありません。少し複雑なアプリを想定するとそのようになることが通常です。

```
let state = {
  name: {
    first: 'Ebisu',
    last: 'JS',
  },
  items: [1, 3, 7, 11],
};
```
