# Immutability

ReduxではstateにImmutability(不変性)という制約を与えることで、シンプルにしてパフォーマンスを向上させることができます。

## Mutable object

JavaScriptのオブジェクトは一般的にmutableです。

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

JavaScriptではオブジェクトはimmutableではありませんから、immutable objectは規約です。
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

などとして新しいオブジェクトを生成しましょう。ということです。

TODO

## 何がうれしいか

TODO

## Immutable array

TODO

## おまけ: Object.freeze

TODO
