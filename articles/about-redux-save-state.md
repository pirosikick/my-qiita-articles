ReduxでlocalStorageにstateのスナップショットを保存する
=====================================================

localStorageにstateのスナップショットを保存するRedux middlewareを作りました。

https://www.npmjs.com/package/redux-save-state

## 使い方

第1引数に保存先キー名を指定して、applyMiddlewareに渡す。

```js
import {createStore, applyMiddleware} from "redux";
import saveState from "redux-save-state/localStorage";
// combineReducersされたreducer達
import reducers from "./reducers";

// 保存先キー名
const key = "app-state-snapshot";

const store = applyMiddleware(saveState(key))(createStore)(reducers);

// React Componentでdispatchが呼ばれるたびに保存
store.dispatch(action);

// stateをJSON.stringifyした文字列が保存される
console.log(localStorage[key]);
```

### オプション

#### `filter`

stateの一部を保存したい時や、stateがImmutableのMap等の場合に、そのままJSON.stringifyに渡せないことがあると思う。
そういう場合は、filterに関数を指定するとその関数の返り値をJSON.stringifyした文字列がlocalStorageに保存される。

```js
// 特定のキーだけ保存する
const filter = state => {
  return { someKey: state.someKey };
};
const store = applyMiddleware(saveState(key, { filter }))(createStore)(reducers);
```

#### `debounce`

何も指定がない場合、dispatchされるたびにlocalStorageに保存される。
パフォーマンス的に気になる場合は、debounceオプションを指定し、
dispatch実行時から遅延してlocalStorageへの保存を実行することができる。

```js
// 300msの間にdispatchされなかったら保存
const debounce = 300;
const store = applyMiddleware(saveState(key, { debounce }))(createStore)(reducers);

store.dispatch(action); // 保存されない
store.dispatch(action); // 保存されない
store.dispatch(action); // 保存されない

// 300msの間にdispatchが発生しなかった場合に
// 3度目のdispatch時のstateが保存される
```

バグや要望とはGithubのIssueでお願いしますー

https://github.com/pirosikick/redux-save-state/issues
