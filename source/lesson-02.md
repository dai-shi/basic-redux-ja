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

オブジェクトがimmutableであることを前提とすると、オブジェクトの一致性の比較が非常に簡単にできます。(また、コピーも簡単になります)

JavaScriptでは`===`で一致性を比較できます。(個別に新規で同じオブジェクトを作る場合は除く)

```
const obj = { a: 1, b: 2 };
const obj2 = Object.assign({}, obj, { a: 3 });
console.log(obj === obj2);
const obj3 = obj;
obj3.a = 3;
console.log(obj === obj3);
```

オブジェクトを変更するときに新しいオブジェクトを作るのですから当然ですが、mutableを許容する場合は、オブジェクトの比較には[deepEqual](https://github.com/substack/node-deep-equal)のようなものを使うことが必要になります。これは再帰的な処理であり一般的に重いです。

Reduxはapplication stateのためのライブラリであり、そのstateが変化したかどうかを検知することは重要です。stateをimmutable objectで構成することで、stateのどの部分が変更されたかをたどることが容易になります。

## Immutable array

JavaScriptではarrayもobjectの一種であり、上記の議論はarrayについても当てはまります。
一方、immutableの規約に沿ったarrayならではの書き方もあります。

Array.pushやArray.spliceはオブジェクトを書き換えてしまうので使いません。
代わりにArray.concatやArray.sliceを使います。

```
const arr = [2, 4, 6, 8];
const arr2 = arr.concat([10, 12]);
const arr3 = arr.slice(0, 3);
const arr4 = arr.slice(0, 2).concat([5]).concat(arr.slice(2));
```

ES2015のspread operatorを使うとさらに簡潔に書くことができます。

```
const arr = [2, 4, 6, 8];
const arr2 = [...arr, 10, 12];
const arr3 = arr.slice(0, 3);
const arr4 = [...arr.slice(0, 2), 5, ...arr.slice(2)];
```

## おまけ: Object.freeze

TODO
https://github.com/substack/deep-freeze
