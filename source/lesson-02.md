# ES2015について

Reduxでコーディグする際はES2015を使うことがほぼ前提となります。もちろん、ES5でも書けますが、ReduxのチュートリアルでもES2015を想定しています。pure functionを書くのに便利になっているためです。ES2015を網羅的に解説するのは多すぎますが、よく使う文法について紹介します。

## constとlet

ES2015では`var`は基本的に使わず、代わりに`const`と`let`を使うことが推奨されます。
 
`const`は再代入しない変数の宣言に用います。`let`は再代入が必要な変数の宣言に用います。再代入とimmutabilityは別物ですので注意が必要です。

```
const x = 1;
let y = 2;
y = 3;

const z = { a: 4, b: 5 };
z.a = 6;
```

## object shorthand

ES2015ではオブジェクトのプロパティ名と値の変数名が同じ場合、省略記法を使うことができます。

```
const foo = 'abc';
const bar = { foo }; // same as { foo: foo }
```

## destructuring object

ES2015ではdestructuring assignmentという簡便な記法が使えます。特にオブジェクトについての記法を紹介します。変数への代入だけでなく、関数のパラメータ宣言でも用います。

```
const obj = { first: 'Redux', last: 'JS' };
const { first, last } = obj;
// first === 'Redux', last === 'JS'

function printName({ first, last }) { console.log(first, last); }
printName(obj);

// same as
// function printName(name) { console.log(name.first, name.last); }
```

## アロー関数について

`() => ...`という記法はアロー関数と呼ばれます。

アロー関数に関する詳細は、例えば下記を参照してください。  
<https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/arrow_functions>

`this`に関するスコープの話を置いておくと、アロー関数は通常の関数の省略記法と考えることができます。

```
const f1 = function(x) { console.log(x); };
const f2 = (x) => { console.log(x); };
// f1 and f2 is almost the same
```

アロー関数は`return`文が一つだけの場合は、文そのものを省略できます。

```
const f3 = (x) => { return (x + 1); };
const f4 = (x) => (x + 1);
// f3 and f4 is the same
```

おまけ：カッコの省略も可能ですが、好みの問題でもあります。

```
const f5 = x => x + 1;
```

## Object.assign

ES2015の`Object.assign`を使うと、オブジェクトをマージすることができます。

```
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
Object.assign(obj1, obj2);
// obj1 is { a: 1, b: 3, c: 4} and obj2 is not changed
```

`Object.assign`を使えば、オブジェクトの一部のプロパティを変更した新しいオブジェクトを簡単に作れます。

```
const obj3 = { a: 1, b: 2 };
const obj4 = Object.assign({}, obj3, { a: 5 });
// obj4 is { a: 5, b: 2 } and obj3 is not changed
```

現在策定中のObject spread propertiesが使えれば、次のようにも書けます。

```
const obj3 = { a: 1, b: 2 };
const obj4 = { ...obj3, a: 5 };
```

本教材では簡単に書ける最後の書式を使います。
