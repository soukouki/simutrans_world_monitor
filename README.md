# simutrans world monitor

simutransとdiscordを連携させるシステムです．[simutrans OTRP](https://github.com/teamhimeh/simutrans/blob/OTRP-distribute/documentation/OTRP_v13_information.md) **v29_5以降** が必要です．

pythonプログラムとSquirrel AI Playerから成り立っています．pythonプログラムはDiscordからの入力をテキストファイルに書き出し，Squirrel側でそれを読んで処理を行う仕組みです．

## 導入方法

CLIの使用を前提とします．

1. `git clone https://github.com/teamhimeh/simutrans_world_monitor` でコードをcloneします．
2. Python3とpipを導入し， `pip install discord.py watchdog` でライブラリを導入します．
3. DiscordでBOTアカウントを作成し，BOTをサーバーに招待します．
   参考: https://discordpy.readthedocs.io/ja/latest/discord.html
4. `python/config.template.py` をコピーして`python/config.py` を作成し，BOTのアクセストークン，メッセージをやり取りするテキストチャンネルのID（Discordで確認できます），Simutransの動作ディレクトリを記入します．
5. `sqai_hm_monitor` ディレクトリを，Simutransの動作ディレクトリ下の`ai` ディレクトリにコピーします．

## 起動方法

1. Pythonプログラムを起動します．`python` ディレクトリに移動した上で， `python monitor.py` を実行します．
   このとき，Simutrans動作ディレクトリに`file_io` ディレクトリが作成され，ファイルが配置されます．正常に起動すると，Discordの指定したチャンネルに，`おはようさん．?をつけてなんでもいうてな．` というメッセージが送信されます．
2. Simutransを起動し，AIプレイヤーを追加します．Scripted AI'sを選択し，左側の有効化ボタンを押します．スクリプトは`sqai_hm_monitor` を選択してください．
   ![画像](images/add_script_player.png)

Simutrans本体より先にPythonプログラムを起動するようにしてください．

## 使用方法

Discordの指定したチャンネルにコマンドを投げることで情報を取り出せます．

### 駅の待機客を取得

書式： `?待機,<駅名>,エントリ数`

例：`?待機,東京駅,3`

応答例：

```
東京駅の待機客は 51人/1216人 やね．
21人 ... 座間駅
19人 ... 山科駅
8人 ... 松本駅
```

指定された停車場の待機客およびその行き先を返します．エントリ数は省略可能で，デフォルト値は5です．省略形のコマンドは `?待機,<駅名>` です．

### プレイヤー番号一覧を取得

書式：`?プレイヤー`

ゲームに参加しているプレイヤー一覧と，その番号を返します．ここで返される番号は，プレイヤーを指定するコマンドで使用します．

### 待機客が定員オーバーしている駅を取得

書式：`?赤棒,<プレイヤー番号>`

例：`?赤棒,1`

応答例：

```
こく☆てつ の赤棒駅はこれや！
上平塚駅 ... 507/480人
内子西停 ... 72/64人
```

指定したプレイヤーに所属する停車場で，いわゆる赤棒が立っている駅とその待機客を返します．

## 追加予定

路線単位のデッドロック検知とか．

常時監視型の通知機能も用意する予定です．

## ライセンス

MITライセンスです．