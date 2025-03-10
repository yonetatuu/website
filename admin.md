---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: single
classes: wide
title: Admin Guide
permalink: /docs/how-to/admin/
sidebar:
  nav: "docs"
---

本書では、KAMONOHASHI を導入する際の設定や使用中のシステム設定変更をスムーズに行う為に各機能や手順を説明します。

## 前提

読者が Linux、Git、Active Directory の基本的な知識を持つことを前提としています。

## 事前準備

KAMONOHASHI を利用するにあたり、事前に設定しておく内容は以下です。

### AD 設定

ユーザ管理に Active Directory(AD)を使用する場合、以下が AD に設定されていることを確認します。

- AD の LDAP 認証の許可

## テナント

テナントは KAMONOHASHI の管理単位です。テナント毎に、ユーザの管理、クォータ(リソースの使用量制限)の設定が行えます。
テナントには、Git リポジトリ、Docker Registry およびオブジェクトストレージが関連付けられ、それぞれ複数のテナント間で共有可能です。
Git リポジトリ、Docker Registry は 1 テナントに複数登録することが可能です。

初期状態では、下記の情報が登録されており、admin は Sandbox というテナントに所属しています。

| 項目       | 初期登録情報        | 詳細                                                          |
| ---------- | ------------------- | ------------------------------------------------------------- |
| テナント   | Sandbox             | GitHub, official-docker-hub を登録済み                        |
| Git        | GitHub              | [GitHub](https://github.com)                                  |
| Git        | GitLab.com          | [GitLab.com](https://gitlab.com)                              |
| レジストリ | official-docker-hub | [dockerhub](https://hub.docker.com/)                          |
| レジストリ | ngc                 | [NVIDIA GPU Cloud](https://ngc.nvidia.com/catalog/containers) |

### テナントの作成

テナントの作成は以下のフローで行います。

1. ストレージ情報の新規登録
1. Git 情報の新規登録
1. Docker Registry 情報の新規登録
1. テナントの作成

ストレージ情報、Git 情報、Docker Registry 情報については、過去に登録したものを再度利用する場合、改めて登録しなおす必要はありません。

#### ストレージ情報の新規登録

テナントが利用するストレージの情報を登録します。
ストレージの登録には、オブジェクトストレージへ以下の操作を行う権限を持ったユーザのアクセスキーおよびシークレットキーが必要です。

- バケット追加
- 追加したバケットに対するオブジェクトの追加・取得・削除

ストレージ情報の新規登録は、以下のフローで実行できます。

1. 管理権限を持つユーザでログイン
1. [システム管理] → [ストレージ管理]を選択し、ストレージ一覧画面を表示する
1. 画面右上の新規登録ボタンを押下する
1. 画面に下記の情報を入力し、ストレージ情報を登録する。

![ストレージ登録画面(スクショ3-3)](/assets/images/admin_figure10.png)

| 項目                     | 要否 | 説明                                                                                       |
| ------------------------ | ---- | ------------------------------------------------------------------------------------------ |
| ストレージ名             | 必須 | KAMONOHASHI に登録する名前。管理者が各登録情報を識別しやすいような、任意の名前を入力する。 |
| ホスト名:ポート          | 必須 | minio の URL<br>（例）kamonohashi.ai:9000                                                  |
| アクセスキー             | 必須 | minio のアクセスキー                                                                       |
| シークレットキー         | 必須 | minio のシークレットキー                                                                   |
| NFS サーバ               | 必須 | NFS サーバのホスト名または IP アドレス                                                     |
| NFS エクスポートポイント | 必須 | NFS のエクスポートポイント                                                                 |

#### Git 情報の新規登録

テナントが利用する Git リポジトリの情報を登録します。

Git 情報の新規登録は、以下のフローで実行できます。

1. 管理権限を持つユーザでログイン
1. [システム管理] → [Git 管理]を選択し、Git 一覧画面を表示する
1. 画面右上の新規登録ボタンを押下する
1. 画面に下記の情報を入力し、Git 情報を登録する。

![Git新規登録画面(スクショ3-3)](/assets/images/admin_figure3.png)

| 項目           | 要否 | 説明                                                                                                                                                                     |
| -------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 名前           | 必須 | 識別名。管理者が各登録情報を識別しやすいような、任意の名前を入力する。                                                                                                   |
| Git 種別       | 必須 | GitLab や GitHub,GitBucket など、Git サービスのプロバイダを選択する。                                                                                                    |
| リポジトリ URL | 必須 | リポジトリに git コマンドでアクセスする際の基底 URL を入力する。<br>（例）https://git.kamonohashi.ai:8080                                                                |
| API URL        | 必須 | リポジトリに REST API でアクセスする際の基底 URL を入力する。<br>リポジトリ URL と同様のものが自動入力されるため、変更の必要がある場合はトグルボタンを選択し、編集する。 |

なお[GitHub](https://github.com/)については、初期構築時に編集不可として自動登録されます。

#### Registry 情報の新規登録

テナントが利用する Docker Registry の情報を登録します。

KAMONOHASHI で利用可能な Docker Registry の条件は以下の通りです。

- DockerHub API、あるいは GitLab API と互換性がある REST API インターフェースがある
- KAMONOHASHI から信頼された HTTPS 通信が可能
  - Proxy ないし NoProxy が適切に設定されている
  - 信頼された証明書が設定されている

Registry 情報の新規登録は、以下のフローで実行できます。

1. 管理権限を持つユーザでログイン
1. [システム管理] → [レジストリ管理]を選択し、レジストリ一覧画面を表示する
1. 画面右上の新規登録ボタンを押下する
1. 画面に下記の情報を入力し、Docker Registry 情報を登録する
   ※DockerHub API 互換の場合と、GitLab API 互換の場合で、入力項目が異なります。

![Docker Registry新規登録画面(スクショ3-4)](/assets/images/admin_figure4.png)

##### DockerHub API の場合

| 項目         | 要否 | 説明                                                                                                                                          |
| ------------ | ---- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| レジストリ名 | 必須 | 識別名。管理者が各登録情報を識別しやすいような名前を入力する。英数字およびハイフンのみが使用可能。                                            |
| 種別         | 必須 | DockerHub を選択する。                                                                                                                        |
| ホスト名     | 必須 | イメージ名に使用されるレジストリの FQDN または IP アドレスを入力する。<br>（例）registry-server                                               |
| ポート番号   | 必須 | イメージ名に使用されるレジストリのポート番号を入力する。<br>（例）5000                                                                        |
| API URL      | 必須 | レジストリに REST API でアクセスする際の基底 URL を入力する。<br>https://ホスト名のように自動入力されるため、変更の必要がある場合は編集する。 |

##### GitLab API の場合

| 項目           | 要否 | 説明                                                                                                                                                                                 |
| -------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| レジストリ名   | 必須 | 識別名。管理者が各登録情報を識別しやすいような名前を入力する。英数字およびハイフンのみが使用可能。                                                                                   |
| 種別           | 必須 | GitLab を選択する。                                                                                                                                                                  |
| ホスト名       | 必須 | イメージ名に使用されるレジストリの FQDN または IP アドレスを入力する。<br>（例）registry-server                                                                                      |
| ポート番号     | 必須 | イメージ名に使用されるレジストリのポート番号を入力する。<br>（例）5000                                                                                                               |
| API URL        | 必須 | レジストリに REST API でアクセスする際の基底 URL を入力する。基本的には GitLab の Web URL と等しい。<br>https://ホスト名のように自動入力されるため、変更の必要がある場合は編集する。 |
| プロジェクト名 | 任意 | プロジェクトパスを、"オーナー名(ユーザ名またはグループ名)/プロジェクト名"の形式で入力する。<br>（例）kamonohashi/sample                                                              |

#### テナントの新規作成

テナントを作成します。

テナントの新規作成は、以下のフローで実行できます。

1. 管理権限を持つユーザでログイン
1. [システム管理] → [テナント管理]を選択し、テナント一覧画面を表示する
1. 画面右上の新規作成ボタンを押下する
1. 画面に下記の情報を入力し、新規テナントを作成する。

![テナント作成画面(スクショ3-2)](/assets/images/admin_figure2.png)

| 項目                   | 要否 | 説明                                                                                                                                                                                    |
| ---------------------- | ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| テナント名             | 必須 | テナントを表す識別子を入力する。英数字およびハイフンのみが使用可能。また更新時は変更不可。<br>（例）kamonohashi                                                                         |
| 表示名                 | 必須 | 画面等への表示名称を入力する。<br>（例）カモノハシ                                                                                                                                      |
| ノートブック無期限実行 | 必須 | ノートブックの無期限実行を許可するかどうかを設定する。 <br>無期限実行を許可すると、ノートブック実行画面で起動時間設定の ON/OFF 選択が可能となる。                                       |
| ストレージ情報         | 必須 | データファイルを格納するストレージを選択する。 <br>選択肢は事前に**ストレージ情報の新規登録**で設定が必要。                                                                             |
| Git 情報               | 必須 | 学習実行時のユーザスクリプトを格納する Git を一つ以上選択する。また、その中で既定で選択される Git を一つ選択する。<br>選択肢は事前に**Git 情報の新規登録**で設定が必要。                |
| Docker Registry 情報   | 必須 | 学習実行時の Docker イメージを格納するレジストリを一つ以上選択する。また、その中で既定で選択されるレジストリを一つ選択する。<br>選択肢は事前に**Registry 情報の新規登録**で設定が必要。 |

### テナント情報の更新

テナント情報、ストレージ情報、Git 情報、Docker レジストリ情報は、各管理画面からそれぞれ更新できます。
一覧に表示されている各項目をクリックして更新画面を表示し、必要な情報を入力します。

### テナントの削除

テナント編集画面の削除アイコンを押下することで、テナントが削除されます。
テナント内データの削除は時間がかかるため、削除用コンテナを別途デプロイすることで行われます。
このコンテナは kqi-admin テナントで実行され、実行状況はリソース利用状況画面のテナント別状況を見ることで確認できます。
削除終了後はコンテナステータスが Succeeded となり、リソースが占有された状態が維持されます。
リソース占有状態を解除するため、この状態を確認した後は該当コンテナを手動削除してください。

## ユーザ

ユーザは KAMONOHASHI の利用者を表し、テナントに関連付けられます。ユーザは複数のテナントに所属できます。
また、ユーザは使用する認証方式の種類によってローカルユーザ、LDAP ユーザの 2 種類に分けられます。

### ユーザの追加とテナントへの関連付け

KAMONOHASHI ではローカルユーザか、LDAP ユーザかによって、ユーザの追加フローが異なります。
ユーザの種類によらず、ユーザ名はシステム内で一意である必要があります。

#### ローカルユーザの追加

ローカルユーザの場合、KAMONOHASHI 内部で認証情報を管理されます。

ユーザの新規作成は、以下のフローで実行できます。

1. 管理権限を持つユーザでログイン
1. [システム管理] → [ユーザ管理]を選択し、ユーザ一覧画面を表示する
1. 画面右上の新規作成ボタンを押下する
1. 画面に下記の情報を入力し、ローカルユーザを新規に追加する。
   ※ローカルユーザの場合、初期テナントの関連付けを作成時にまとめて行う。

![ユーザ新規作成画面(スクショ3-5)](/assets/images/admin_figure5.png)

| 項目               | 要否 | 説明                                                                                                       |
| ------------------ | ---- | ---------------------------------------------------------------------------------------------------------- |
| ユーザアカウント   | 必須 | ユーザ名。システム内で一意である必要がある。英数字およびハイフンのみが使用可能。                           |
| パスワード         | 必須 | ユーザ認証に用いるパスワード。                                                                             |
| パスワード(再入力) | 必須 | パスワードの確認用。上記パスワードと一致している必要がある。                                               |
| システムロール     | 任意 | 追加ユーザに付与するシステムロールを選択する。                                                             |
| テナント           | 必須 | ユーザを関連付けるテナントを選択する。複数可。                                                             |
| テナントロール     | 任意 | 上記テナント項目で選択した分のテナントについて、ユーザが所有するテナントロールをそれぞれ選択する。複数可。 |

#### LDAP ユーザの追加

LDAP ユーザの場合、認証情報の管理に KAMONOHASHI 外の LDAP 認証サービスを利用します。
パスワード変更など、LDAP ユーザの管理は KAMONOHASHI 上では行えません。

LDAP ユーザは、管理者ではなく、LDAP ユーザ自身が KAMONOHASHI にログインすることで追加できます。
ユーザが LDAP サービスに登録されているユーザ名とパスワードで KAMONOHASHI にログインすると、KAMONOHASHI 内にユーザ情報が登録され、Sandbox テナントに自動で関連付けられます。

LDAP ユーザに対して、システムロールの付与や、Sandbox 以外のテナントへの関連付けを行う場合、下記の処理を実行します。

1. ユーザが LDAP サービスに登録されているユーザ名とパスワードで KAMONOHASHI にログインする
   - KAMONOHASHI 内にユーザ情報が登録される
   - 既定では Sandbox テナントに自動で関連付けられる
1. 管理権限を持つユーザが、[システム管理] → [ユーザ管理]を選択し、ユーザ一覧画面を表示する
1. 追加されている LDAP ユーザ名を一覧から探し、列を選択する
1. 表示されたユーザ編集画面で、ローカルユーザの場合と同様に、システムロールと関連付けるテナントおよびそのテナントロールをそれぞれ選択する

### ロール

ユーザにロールを付与することで、操作可能な機能を制御できます。
一人のユーザには複数のロールを関連付けられます。

ロールは大きく以下の 2 種類に大別されます。

- システムロール
  - テナントを横断する、システム全体に関連した情報を取り扱うためのロール
  - 既定では全てのシステム管理権限がある「Admin ロール」が使用できる
- テナントロール
  - 特定のテナント内に限定された情報を扱うためのロール
  - 所属するテナントごとに、ユーザに付与するテナントロールを指定する
    - 例えば、同一のユーザに対し、A テナントには Manager ロールを付与し、B テナントには User ロールと Researcher ロールを付与する、など
  - 既定では以下の 3 種類のテナントロールが使用できる

| ロール     | 説明                                                                 |
| ---------- | -------------------------------------------------------------------- |
| User       | データの追加・削除、およびデータセットの作成・変更を行う権限を持つ。 |
| Researcher | 学習の実行、結果の評価を行う権限を持つ。                             |
| Manager    | そのテナント内で使用する計算リソースや権限の管理を行う権限を持つ。   |

### カスタムロールの追加

既定されているロールに加え、独自のロールを追加することができる。追加されたロールは、全テナントで利用可能になる。

ロールの新規作成画面は、以下のフローで表示できます。

1. 管理権限を持つユーザでログイン
1. [システム管理] → [ロール管理]を選択し、ロール管理画面を表示する
1. 画面右上の新規登録ボタンを押下する
1. 画面に下記の情報を入力し、カスタムロールを新規に追加する。

![ロール新規登録画面(スクショ3-9)](/assets/images/admin_figure9.PNG)

| 項目     | 要否 | 説明                                                                                                               |
| -------- | ---- | ------------------------------------------------------------------------------------------------------------------ |
| ロール名 | 必須 | ロールの識別名。システム内で一意である必要がある。英数字およびハイフンのみが使用可能。                             |
| 表示名   | 必須 | ユーザに表示される名前。任意の文字を使用可能。                                                                     |
| 種別     | 必須 | テナントロールの場合は"テナント"、システムロールの場合は"システム"を選択する。                                     |
| ソート順 | 必須 | 並び順を入力する。小さいほど前に表示される。一意である必要はないが、同じ値の場合のソート順は変動する可能性がある。 |

追加したカスタムロールの権限設定は、メニューアクセス管理で行います。

## ノード

ノードとは、KAMONOHASHI に管理される物理的あるいは仮想的な計算用コンピュータを指します。
KAMONOHASHI 自体を構成する管理サーバ群は含みません。

### ノードの追加

KAMONOHASHI の管理下に新しくノードを追加します。 Kubernetes master ノード上での作業および、KAMONOHASHI の Web 画面上での作業が必要となります。

#### Kubernetes master ノード上での作業

1. GPU サーバの準備と KAMONOHASHI と同一ネットワークへの接続
1. Kubernetes master ノードに ssh
1. `/var/lib/kamonohashi/deploy-tools/deepops/config/inventory` を編集し、マシンを追記(下記例を参照)
1. `/var/lib/kamonohashi/deploy-tools/deploy-kamonohashi.sh update node-conf` を実行
1. 処理が完了するまで待機
1. `kubectl get csr -o name | xargs -I {} kubectl certificate approve {}`を実行

#### inventory ファイルへのノード追記例

既に利用している GPU のホスト名が gpu1(10.0.0.1)、追加する GPU のホスト名が gpu2(10.0.0.2)の場合、以下のように[all]と[kube-node]に gpu2 を追記します。

```
[all]
gpu1 ansible_host=10.0.0.1
gpu2 ansible_host=10.0.0.2
...
[kube-node]
gpu1
gpu2
```

再構築または CPU ノード追加後に KAMONOHASHI の Web 画面で次を実行します

#### KAMONOHASHI の Web 画面上での作業

1. ブラウザから kamonohashi にアクセス
1. 管理権限を持つユーザでログイン
1. [システム管理] → [ノード管理]を選択し、ノード一覧画面を表示する
1. 画面右上の新規作成ボタンを押下する
1. 画面に下記の情報を入力し、ノードを新規に追加する

![ノード新規作成画面(スクショ3-6)](/assets/images/admin_figure6.png)

| 項目                 | 要否 | 説明                                                                                                                                                                                                 |
| -------------------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 名前                 | 必須 | ノード名。システム内で一意である必要がある。英数字およびハイフンのみが使用可能。                                                                                                                     |
| メモ                 | 任意 | 管理用のメモを自由に入力できる。                                                                                                                                                                     |
| パーティション       | 任意 | ノードにパーティションを設定できる。パーティションについての詳細は後述。                                                                                                                             |
| アクセスレベル       | 必須 | ノードへのアクセスレベルを設定できる。アクセスレベルについての詳細は後述。Private を選択した場合、アクセスを許可するテナントを選択する UI が表示される。指定しなかった場合は Disabled が設定される。 |
| TensorBoard 実行可否 | 必須 | ノード上で TensorBoard コンテナの実行を許可するか指定できる。                                                                                                                                        |
| Notebook 実行可否    | 必須 | ノード上で Notebook コンテナの実行を許可するか指定できる。                                                                                                                                           |

#### パーティション

パーティションを設定することで、学習を実行するノードを指定することができます。
学習の実行時にパーティションを指定すると、そのパーティションが付与されたノードでのみ、学習が実行されます。パーティションとして指定できる文字は、英数字及びハイフンのみです。

![パーティション管理(図8-1)](/assets/images/admin_figure7.png)

#### アクセスレベル

アクセスレベルは、テナント単位で利用可能なノードを設定するための仕組みです。
アクセスレベルには以下の 3 種類があります。

- Disabled
  - どのテナントも、そのノードを利用することができない
  - ノードのメンテナンスを行う際などに指定する
- Private
  - 指定されたテナント（複数可）のみが、そのノードを利用することができる
- Public
  - すべてのテナントがそのノードを利用することができる。

## クォータ

クォータはテナントがクラスタで利用できる最大の CPU コア数、メモリ容量、GPU 数の上限を表します。
クォータを利用することで、特定のテナントがクラスタのリソースを大量に利用することを抑制できます。

### クォータ管理

クォータを設定するには、[システム管理] → [クォータ管理]を選択し、クォータ設定の一覧画面を表示します。

各テナントに対して、次の情報を入力します。
クォータの各項目に 0 を設定した場合、無制限に利用できます。

| 項目   | 要否 | 説明                                                               |
| ------ | ---- | ------------------------------------------------------------------ |
| CPU    | 任意 | テナントが利用できる最大 CPU の量。CPU のコア数で入力する。        |
| Memory | 任意 | テナントが利用できる最大メモリの量。ギガバイト(Gi)単位で入力する。 |
| GPU    | 任意 | テナントが利用できる最大 GPU の数。GPU の枚数で入力する。          |

## リソース

リソースとは、クラスタを構成する各ノードの計算リソース（CPU、メモリ、GPU）を表します。

### リソース管理

クラスタの各ノード上に現在配備されているコンテナと、それらに割り当てられたリソースを一覧で確認する事ができます。これによって、ノード単位の負荷状況や、リソースを大量消費しているテナントやユーザを把握することができるので、必要に応じて、不要なコンテナを削除し、リソースの解放処理を行えます。

現在のリソース割当状況を確認するには、管理権限を持つユーザでログインした後、[システム管理] → [リソース管理] を選択し、リソース管理の一覧画面を表示し行います。

この画面には、以下三つの表示形式があります。

- ノード別
  - ノードごとに、現在使用しているリソースの合計と、稼働しているコンテナの一覧が確認できる
  - [ノード管理](#new_node)でまだ追加されていないノードがクラスタ上で確認された場合、ノード名の前に"Unknown:"が接頭語として表示される
  - まだ配備先のノードが決定しておらず、待機状態のコンテナがある場合、"Unassined"というグループにまとめられる
- テナント別
  - テナントごとに、現在使用しているリソースの合計と、稼働しているコンテナの一覧が確認できる
  - クォータが設定されておらず、無制限にリソースが使用できる場合、"Infinity"が表示される
- コンテナ一覧
  - 全コンテナの一覧と、その利用リソースが表示される

シングルノード構築の場合、管理系コンテナ(k8s-master, kamonohashi,storage)の情報もリソース画面で可視化されます。
クラスタ構築で管理系コンテナ(k8s-master, kamonohashi,storage)の情報をリソース画面で見る場合、ブラウザからノード管理画面を開き、ノード情報を追加することでリソース画面での管理系ノードのリソース使用量の可視化を行うことができます。
管理系コンテナはブラウザ上からは編集できないため、ご注意ください。

### コンテナリソースとジョブ実行履歴
リソース管理の「データダウンロード」タブを押下するとコンテナ利用またはジョブ実行履歴を確認することができます。

- コンテナリソース
  - 定期的(デフォルト：1時間毎)にコンテナのリソース利用状況（コンテナの要求リソース数やテナント情報、取得時のステータス、配備先ノード情報（未配備先情報含む））を時系列データをDBに蓄積します。
  - 期間を指定してリソース利用履歴のCSVファイルをダウンロードすることができます。
    - ※CSVファイルの可視化についてはKAMONOHASHIでは用意していないため、Excel等で開き確認するようにしてください。
  - 指定した終了日までの履歴を削除することができます。
  - 期間を指定しない場合は保存されている全履歴が対象になります。

- ジョブ実行履歴
  - KAMONOHASHI上で実行されたジョブの履歴情報（ジョブの待機時間や開始・終了時間、要求されたリソース数、最終的なステータス、テナント情報、実行されたノード情報）を時系列データとしてDBに蓄積されます。
  - 期間を指定してジョブ実行履歴のCSVファイルをダウンロードすることができます。
    - ※CSVファイルの可視化についてはKAMONOHASHIでは用意していないため、Excel等で開き確認するようにしてください。
  - 指定した終了日までの履歴を削除することができます。
  - 期間を指定しない場合は保存されている全履歴が対象になります。

## メニュー

メニューとは、"データ管理"や"テナント管理"など、KAMONOHASHI が提供する各機能セットを表します。

メニューには、以下の 4 つの種別があります。

- Public
  - 未ログインユーザにも公開されるメニュー（e.g. ログイン）
- Internal
  - ログインユーザに公開されるメニュー（e.g. ダッシュボード）
  - ロールを一つも持っていないユーザにも利用可能
- Tenant
  - 特定のテナントロールを持つユーザのみに公開されるメニュー（e.g. データ管理）
  - 接続中のテナントに限定された情報を取り扱う
- System
  - 特定のシステムロールを持つユーザのみに公開されるメニュー（e.g. テナント管理）
  - テナントを横断する、システム全体に関連した情報を取り扱う

### メニューアクセス管理

メニューアクセス管理画面では、全メニューを一覧で確認できます。
加えて、種別が Tenant あるいは System であるメニューに対しては、公開先のロールを編集することができます。

メニューアクセス管理画面を表示するには、管理権限を持つユーザでログインした後、[システム管理] → [メニューアクセス管理]を選択します。

![メニューアクセス管理(スクショ308)](/assets/images/admin_figure8.PNG)

メニューの種別が Tenant あるいは System である場合、以下の手順でそのメニューを利用可能なロールを改廃できます。

1. ロールを改廃したいメニューの右側に表示されているボタン群を確認する。青く塗られているロールがアクセス権があるロール、白く塗られているロールがアクセス権がないロールである。
2. アクセス権を付与したいロールをクリックし、背景を白から青に変える。
3. アクセス権を削除したいロールをクリックし、背景を青から白に変える。
4. 全ての変更が完了したら、右下の「更新」ボタンを選択する

## ストレージ

学習に使用するデータや学習結果を保存するためにストレージが必要です。

### ストレージの編集

KAMONOHASHI に登録しているストレージの情報を変更します。
KAMONOHASHI の Web 画面上での作業および再デプロイに備えて Kubernetes master ノード上での作業が必要となります。

#### KAMONOHASHI の Web 画面上での作業

1. ブラウザから kamonohashi にアクセス
1. 管理権限を持つユーザでログイン
1. [システム管理] → [ストレージ管理]を選択し、ストレージ一覧画面を表示する
1. 編集したいストレージ情報を選択する
1. ストレージ編集画面にて編集し、右下の「保存」ボタンを押下する

#### Kubernetes master ノード上での作業

1. Kubernetes master ノードに ssh
1. `/var/lib/kamonohashi/deploy-tools/{バージョン番号}/kamonohashi/conf/settings.yml` を開き、appsettings の DeployOptions を編集し保存する
