---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: single
classes: wide
title: アクアリウムデータ保持者ガイド
permalink: /docs/how-to/aquarium_owner
sidebar:
  nav: "docs"
---

本書はデータ保持者を対象としてアクアリウムの機能や手順を説明する事を目的としています。

## 概要

データ保持者は以下の操作を行えます。
* [ログイン](/docs/how-to/aquarium_owner#ログイン)
* [ユーザ情報設定](/docs/how-to/aquarium_owner#ユーザ情報設定)
* [ダッシュボード](/docs/how-to/aquarium_owner#アクアリウムダッシュボード)
* [実験を実行する](/docs/how-to/aquarium_owner#新規実験)
* [実験を管理する](/docs/how-to/aquarium_owner#実験管理)

## ログイン
アクアリウムはブラウザから使用できます。
ブラウザで開始するには、アクアリウムにログインします。
アクアリウムのURLが分からない場合はシステム管理者に問い合わせてください。

### ログイン
アクアリウムにログインするにはログイン画面からユーザー名・パスワードを入力します。
ユーザー名・パスワードはシステム管理者から提供されます。

![ログイン](/assets/images/aquarium/login.png)

#### テナントについて

テナントはアクアリウムの最小の利用単位です。
テナント毎にリソースの使用量などが決められています。
ユーザは複数のテナントに所属することができます。

利用するテナントを切り替えるには、右上のユーザ名が表示されているリンクを押し、切り替えたいテナントを選択します。

![テナント切替](/assets/images/aquarium/tenant.png)

## ユーザ情報設定

利用者は右上のプルダウンメニューより[ユーザ情報設定]を選択することで、各種ユーザ設定情報の確認・変更が行えます。

### Gitトークンの設定

アクアリウムでは学習に用いる学習スクリプトなどを、システム管理者によって設定されたGitリポジトリサービスから取得します。

Gitリポジトリサービスへのアクセスに認証が必要な場合、ユーザ情報設定画面にて [Git Token]タブを選択し、認証情報を設定します。

### レジストリトークンの設定

アクアリウムでは、学習環境としてDockerコンテナを使用します。
このコンテナはシステム管理者によって設定されたDockerレジストリサービスから取得します。

Dockerレジストリサービスへのアクセスに認証が必要な場合、ユーザ情報設定画面にて [Registry Token]タブを選択し、認証情報を設定します。

## アクアリウムダッシュボード

ダッシュボードからは、アクアリウムの各機能を実行することができます。

![ダッシュボード](/assets/images/aquarium/dashboard_owner.png)

## データセット管理

データセット管理画面では、アクアリウムの実験で使用するデータセットの作成、表示、更新を行います。

### データセット一覧

テンプレート一覧を表示するには、ダッシュボードまたはメニューから、データセットを選択します。
選択すると、作成済みデータセットの一覧が表示されます。

![データセット一覧](/assets/images/aquarium/dataset-list.png)

### データセット作成

データセットを作成するには、データセット一覧画面から「新しいデータセット」を選択します。

* パソコンから画像をアップロードする場合

![データセット作成1](/assets/images/aquarium/dataset-create1.png)

|種類　|説明　|
|---|---|
|データセット名|データセットの名前。|
|パソコンから画像をアップロード|複数のファイルを登録できる。データ形式は任意。|

* KAMONOHASHIからデータを選択する場合

![データセット作成2](/assets/images/aquarium/dataset-create2.png)

|種類　|説明　|
|---|---|
|データセット名|データセットの名前。|
|KAMONOHASHIからデータを選択|KAMONOHASHIに登録済みのデータを追加する。|

### データセット表示

データセットを表示するには、データセット一覧画面から表示するデータセットを選択します。

![データセット表示](/assets/images/aquarium/dataset-detail.png)

|種類　|説明　|
|---|---|
|データセットバージョン|表示・更新したいデータセットのバージョン|
|メモ|データセットの説明など補足情報。|
|ファイル一覧|データセットに含まれるファイルの一覧。画像ファイルの場合、ファイルを選択するとプレビューが表示される。|

### データセット更新

データセットを更新するには、データセット表示画面からアップロードを選択します。
続いて、データセットにファイルを追加する方法を選択します。

* パソコンから画像をアップロードする場合

![データセット更新1](/assets/images/aquarium/dataset-edit1.png)

|種類　|説明　|
|---|---|
|パソコンから画像をアップロード|複数のファイルを追加できる。データ形式は任意。|
|アップロードメモ|データセットの説明など補足情報。|

* KAMONOHASHIからデータを選択する場合

![データセット更新2](/assets/images/aquarium/dataset-edit2.png)

|種類　|説明　|
|---|---|
|KAMONOHASHIからデータを選択|KAMONOHASHIに登録済みのデータを追加する。|
|アップロードメモ|データセットの説明など補足情報。|

### データセット削除

データセットを削除するには、データセット表示画面からデータセットバージョン削除またはデータセット削除を選択します。

|種類　|説明　|
|---|---|
|データセットバージョン削除|データセットの選択したバージョンを削除する。|
|データセット削除|このデータセット全体を削除する。|

## 新規実験

新規に実験を開始するには、ダッシュボードまたはメニューから、新規実験を選択します。
選択すると、実験に使用するテンプレートの選択画面が表示されます。

![実験開始1](/assets/images/aquarium/experiment-create1.png)

テンプレートを選択すると、実験の設定登録画面が表示されます。

![実験開始2](/assets/images/aquarium/experiment-create2.png)

|種類　|説明　|
|---|---|
|実験名|この実験の名前。|
|データセットの選択|この実験で使用するデータセットの選択画面を表示。|
|テンプレートバージョン|この実験で使用するテンプレートのバージョン。|

データセットの選択画面では、一覧からデータセットを選択します。
選択ボタンを押下すると、データセットのバージョン選択画面に移ります。

![実験開始3](/assets/images/aquarium/experiment-create3.png)

データセットのバージョン選択画面では、一覧からデータセットのバージョンを選択します。
選択ボタンを押下すると、実験の設定登録画面に戻ります。

![実験開始4](/assets/images/aquarium/experiment-create4.png)

「実験を開始する」を選択すると、実験を開始します。

## 実験管理

実験管理画面では、実験の表示・削除を行います。また、推論もこの画面から行います。

### 実験一覧

実験一覧を表示するには、ダッシュボードまたはメニューから、実験履歴を選択します。
選択すると、実験一覧が表示されます。

![実験一覧](/assets/images/aquarium/experiment-list.png)

### 実験表示

実験の詳細を表示するには、実験一覧から画面から表示する実験を選択します。

![実験詳細](/assets/images/aquarium/experiment-detail.png)

|種類　|説明　|
|---|---|
|詳細をグラフで見る|この実験結果を基にtensorboardを起動する。|
|データセット詳細|この実験で使用したデータセットの表示画面に移動する。|
|テンプレート詳細|この実験で使用したテンプレートの表示画面に移動する。|

### 推論一覧

この実験の推論一覧を表示するには、実験履歴から推論を選択します。
選択すると、推論一覧が表示されます。

![推論一覧](/assets/images/aquarium/experiment-eval-list.png)

|種類　|説明　|
|---|---|
|詳細をグラフで見る|この推論結果を基にtensorboardを起動する。|
|削除する|この推論を削除する。|

### 推論実行

推論を実行するには、推論一覧から「推論を実行」を選択します。
選択すると、推論の設定画面が表示されます。

![推論実行](/assets/images/aquarium/experiment-eval-create.png)

|種類　|説明　|
|---|---|
|名前|この推論の名前。|
|データセットを選択|この推論で使用するデータセットの選択画面を表示。|

### 実験削除

実験を削除するには、実験表示画面から実験削除を選択します。

### デバッグ情報

デバッグ画面では、実験のデバッグを行うユーザ向けにデバッグ情報を表示します。

![実験デバッグ](/assets/images/aquarium/experiment-debug.png)

