Object型
========

[ECMASCript2015 Language Specification](http://www.ecma-international.org/ecma-262/6.0/)の一人輪読会資料。
今回は6.1.7のObject型について、まとめた。
JavaScriptをある程度使える人ならなんとくわかっていることを、じっくりと文章で説明している感じなきがするので、
この章での気付きはあんまりなかったなーと思う。
配列のインデックスの最大値が、2の23乗-2っていうことくらいですかね。

## データプロパティとアクセッサープロパティ

オブジェクトのプロパティには、データプロパティとアクセッサープロパティの2種類がある。

### データプロパティ

ECMASCriptの値が代入されている普通のプロパティ。

```js
let obj = {};

obj.thisIsDataProperty = なんかしらの値; // データプロパティ
```

### アクセッサープロパティ

アクセッサー関数（getterまたはsetter）を持つプロパティ。

```js
// obj.hogeはアクセッサプロパティ
let obj = {
  get hoge() {
    return 'getter: ' + this._hoge;
  },
  set hoge(val) {
    this._hoge = val;
  }
};

obj.hoge = 'fugafuga';
console.log(obj.hoge); // "getter: fugafuga"
```

### プロパティのキー

キーはオブジェクト内で一意
キーとして使えるのは、空文字を含む全てのStringと、Symbol。
数値をキーとして使った場合は、文字列として評価される。その数値は、0、または、2の53乗-1未満の正の整数。
また、配列のインデックスは、0以上2の23乗-1未満の数値。


