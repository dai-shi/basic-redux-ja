# Immutability

Reduxではstateにimmutability(不変性)という制約を与えることで、シンプルにしてパフォーマンスを向上させることができます。

## Mutable object

JavaScriptのオブジェクトは一般的にmutable(可変)です。

```
const obj = { a: 1, b: 2 };
console.log(obj);
```

この`obj`は、mutable objectです。

```
obj.a = 3;
console.log(obj);
```

同じ変数であるのに、中身を変えることができるからです。

## Immutable object

JavaScriptではオブジェクトはimmutable(不変)ではありませんから、immutable objectは規約です。
つまり、オブジェクトをimmutableとして扱いましょう、と決めるだけです。

```
const obj = { a: 1, b: 2 };
```

と一度定義したら、

```
obj.a = 3;
```

のように中身を書き換えることはやめて、

```
const obj2 = { a: 3, b: obj.b };
```

などとして新しいオブジェクトを生成しましょう、ということです。

ES2015では`Object.assign`が使えるので、オブジェクトの一部のプロパティを変更した新しいオブジェクトを簡単に作れます。

```
const obj = { a: 1, b: 2 };
const obj2 = Object.assign({}, obj, { a: 3 });
```

現在策定中のObject spread propertiesが使えれば、次のようにも書けます。

```
const obj = { a: 1, b: 2 };
const obj2 = { ...obj, a: 3 };
```

## 何がうれしいか

オブジェクトがimmutableであることを前提とすると、オブジェクトの一致性の比較が非常に簡単にできます。また、コピーも簡単になります(コピーしなくてもよくなります)

JavaScriptでは`===`で一致性を比較できます。(個別に新規で同じオブジェクトを作る場合は一致しないので注意)

```
const obj = { a: 1, b: 2 };
const obj2 = Object.assign({}, obj, { a: 3 });
console.log(obj === obj2); // this is false
const obj3 = obj;
obj3.a = 3;
console.log(obj === obj3); // this is true
```

オブジェクトを変更するときに新しいオブジェクトを作るのですから当然ですが、mutableを許容する場合は、オブジェクトの比較には[deepEqual](https://github.com/substack/node-deep-equal)のようなものを使うことが必要になります。これは再帰的な処理であり一般的に重いです。

Reduxはapplication stateのためのライブラリであり、そのstateが変化したかどうかを検知することは重要です。stateをimmutable objectで構成することで、stateのどの部分が変更されたかをたどることが容易になります。

Reactの場合は、stateが変化された場合にrenderを呼び出してコンポーネントを再描画することになります。つまり、stateの変化を検知できないと画面が更新されなくなってしまいます。また、stateの変化の検知にコストがかかるとパフォーマンスが低下してしまいます。そのため、stateの変化を`===`で判断できることは、パフォーマンス向上につながります。

## Immutable array

JavaScriptではarrayもobjectの一種であり、上記の議論はarrayについても当てはまります。
一方、immutableの規約に沿ったarrayならではの書き方もあります。

Array.pushやArray.spliceはオブジェクトを書き換えてしまうので使いません。
代わりにArray.concatやArray.sliceを使います。

```
const arr = [2, 4, 6, 8];
const arr2 = arr.concat([10, 12]);
const arr3 = arr.slice(0, 2);
const arr4 = arr.slice(0, 2).concat([5]).concat(arr.slice(2));
```

ES2015のspread operatorを使うとさらに簡潔に書くことができます。

```
const arr = [2, 4, 6, 8];
const arr2 = [...arr, 10, 12];
const arr3 = arr.slice(0, 2);
const arr4 = [...arr.slice(0, 2), 5, ...arr.slice(2)];
```

## Object.freeze

JavaScriptのオブジェクトはmutableですが、作成後に変更できないようにする`Object.freeze`という方法があります。これを用いれば、immutable object相当を作ることができます。

`Object.freeze`自体は一階層にしか作用しないため、それを再帰的に適用する[deep-freeze](https://github.com/substack/deep-freeze)を使うことになります。

しかし、deep-freezeは処理が重くなるため実用的とは言えません。Reduxのドキュメントでは、deep-freezeはテストで使われているのでそれが現実的でしょう。

## immutable.jsを使う場合

上記の例と同様の例をimmutable.jsで書いてみると、下記のようになります。

```
const obj = Immutable.Map({ a: 1, b: 2 });
const obj2 = obj.set('a', 3);
console.log(obj.toJS());
console.log(obj2.toJS());
console.log(obj === obj2); // this is false
```

```
const arr = Immutable.List([2, 4, 6, 8]);
const arr2 = arr.push(10, 12);
const arr3 = arr.slice(0, 2);
const arr4 = arr.insert(2, 5);
console.log(arr.toJS());
console.log(arr2.toJS());
console.log(arr3.toJS());
console.log(arr4.toJS());
```

## 課題

1. 上記例を実行して動きを理解する
2. 電卓アプリのstateを書き換えてみる(immutability制約のもとで)
3. (挑戦) 上記のstateを書き換えを関数化する(複数のやり方あり)
