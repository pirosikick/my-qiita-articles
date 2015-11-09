Reduxで大量に発生するactionを間引く
==================================

scrollイベントに関連するactionなど、大量に発生するactionを間引く、Redux middlewareを作りました。

https://www.npmjs.com/package/redux-throttle-actions

## インストール

```
$ npm install --save redux-throttle-actions
```

## 使い方

第1引数は間引きたいAction Typeを配列もしくは文字列で指定します。文字列の場合は、
内部で配列にして処理します。
第2引数は間引きたい秒数をmillisecondsで指定します。

```js
import {createStore, applyMiddleware} from "redux";
import throttleActions from "redux-throttle-actions";
// combineReducersされたreducer達
import reducers from "./reducers";
import {someType} from "./constants/actionTypes";

// someTypeが実行頻度が100msに一度になるように間引く
const throttleSomeType = throttleActions(someType, 100);
const store = applyMiddleware(throttleSomeType)(createStore)(reducers);

// In view
store.dispatch({ type: someType, ... }); // dispatchされる
store.dispatch({ type: someType, ... }); // 間引かれて、dispatchされない
store.dispatch({ type: someType, ... }); // 間引かれて、dispatchされない

// 一度目のdispatchから100ms経過

store.dispatch({ type: someType, ... }); // dispatchされる
```

内部で[lodashのthrottle関数](https://lodash.com/docs#throttle)を使っており、
第3引数はthrottle関数の第3引数に渡される。（options.leadingとoptions.trailing）

```js
// そのまま内部で使っている_.throttleの第3引数に渡される
const options = { leading: true, trailing: true };
const throttleSomeType = throttleActions(someType, 100, options);
```

バグや要望とはGithubのIssueでお願いしますー

https://github.com/pirosikick/redux-save-state/issues
