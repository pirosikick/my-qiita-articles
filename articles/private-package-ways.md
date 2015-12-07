仮
==

## （言いたいこと・書きたいこと）

- 会社でNode.jsで何かを作る時に全てOSSで済めば良いが、やっぱりオープンにできない部分があると思う。
- それらをどう管理するかのまとめ
- 本当はsinopiaについて調べて終わりにしようと思ったが既に書いている人が居たので、内容をもりました。
    - http://qiita.com/Quramy/items/2d019474bdcf692f7c1a

## sinopia

[SinopiaでPrivateなnpmレポジトリを作成する方法](http://qiita.com/Quramy/items/2d019474bdcf692f7c1a)

### ソースコードリーディングメモ

- lib/cli.js
  - `sinopia`が実行されたら最初に呼ばれる
  - loggerの初期化、設定ファイル読み込み、サーバの起動など
- lib/index.js
  - expressの初期化
- lib/config.js
  - config.ymlの管理
- lib/config-path.js
  - config.ymlのパスを返す
  - 下記順で存在するかチェックし、無ければ1で`conf/default.yml`をベースに作成
    1. `$XDG_CONFIG_HOME/sinopia/config.yml` or `$HOME/.config/sinopia/config.yml`
    2. `$APPDATA/sinopia/config.yml`
    3. `./sinopia/config.yml`
    4. `./config.yml`
- lib/auth.js
  - 認証周り
  - プラガブル
- lib/local-data.js
  - .sinopia-data.jsonの管理
  - sync()で.sinopia-data.jsonに書き出し
- lib/logger.js
  - bunyan使っている
  - setupで設定から初期化

- config.web.enable
    - falseの場合、web UI無し
- config.users
