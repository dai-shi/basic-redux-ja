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

## なぜ一箇所で管理するか

Reactで説明するとすべてのコンポーネントがstatelessになります。
副作用がまったくないため、動作の予測がしやすくなります。
また、stateを切り替えるだけでViewの動作を確認することができます。
Redux DevToolsを使うとタイムマシン風のリプレイが可能です。

## immutable.jsを使う場合

let state = Immutable.Map({
  inputValue: 0,
  resultValue: 0,
  showingResult: false,
});

[immutable.js](https://facebook.github.io/immutable-js/)はfacebookが開発したライブラリで、疑似的なobjectをimmutableに扱えるようにするものです。
immutable.jsを使うとimmutabilityが保証されるので、
JavaScript objectを単純に使う場合より便利です。
一方、immutable.jsで提供されている機能以外のことを行おうとすると難しくなるかもしれないため、どちらを選択するかは注意が必要です。

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