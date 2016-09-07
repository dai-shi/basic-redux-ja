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



## immutable.jsを使う場合



## 課題

1. 
2. 
3. 
