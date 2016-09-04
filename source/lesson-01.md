# 環境構築

JS Binを使います。

## アクセス先

<https://jsbin.com>

## 初期設定

下記の手順を実施してください。

1. 左上の「×」を押して画面を広くする
2. HTMLの`<head>`タグの中に
```
<script src="https://npmcdn.com/redux@3.5.2/dist/redux.min.js"></script>
<script src="https://wzrd.in/standalone/deep-freeze@latest"></script>
```
を追記する
3. HTMLボタンを押してHTMLタブを消す
4. Consoleボタンを押してConsoleタブを表示する
5. JavaScriptボタンを押してJavaScriptタブを表示する
6. 青字のJavaScriptの右にある三角マークを押して、ドロップダウンから「ES6/Babel」を選択する (このエリアをJavaScriptエリアと呼ぶ)

## 動作確認のコード

下記をJavaScriptエリアに入力して、ConsoleエリアのRunボタンを押すとReduxの関数の一覧が表示されることを確認しましょう。

```
console.log(Object.keys(Redux));
```

## 補足

- ブラウザをリロードすると設定が戻ってしまう場合があるので、その場合は上記手順をやり直す
- warningsが出ることがあるが、そこまで気にしない
- errorsが出ると動かないので、修正する
- 左上のロゴマークを押してKeyboard Shortcutsを参照できる
