# PuTTYの利用

ここでは、Windowsで利用可能なSSHクライアントである、PuTTYを用いたインタラクティブノードへのログイン手順を示します。OpenSSHクライアントなどコマンドラインベースのSSHクライアントを利用したログイン手順は、[インタラクティブノードへの接続](../getting-started.md#connecting-to-interactive-node)を参照してください。

インタラクティブノードへのログインは、以下の手順で実施します。

1. PuTTYを起動し、SSHトンネル情報を設定
2. アクセスサーバ(*as.v3.abci.ai*)にログインして、SSHトンネルを作成
3. 別のPuTTY画面を起動して、SSHトンネルを用いてインタラクティブノードにログイン

!!! note
    事前に[ABCI利用者ポータル](https://portal.v3.abci.ai/)にてSSH公開鍵の登録が必要です。

## SSHトンネル情報の設定 {#ssh-tunnel-with-putty}

* PuTTY の起動

    PuTTYを起動すると以下のような画面が表示されます。

<div align="center">
<img src="3login_putty_00.png"  width="480" title="success login as" />
</div>

* SSHポート転送の設定

    左のCategoryから、 Connection -> SSH -> Tunnels とメニューを開き、Tunnlesをクリックします。表示されたSSHポート転送の設定項目に、以下表の設定値を入力します。ポート番号はお使いのシステムで許容されているポート番号の中から任意の値を設定可能ですが、他の設定は固定値です。

	| 項目 | 画面と設定値 |
	|:--|:--|
	| 設定画面 | <img src="3login_putty_02.png" width="480" title="ssh tunnel setting" /> |
	| ローカルのポート | システムで許されている任意のポート番号 (画面例: 31022) |
	| リモート側ホスト | *login:22* (画面例: *login:22*) |
	| ポート | 22 (固定値) |

* [Add]ボタンをクリックして設定を追加します。

<div align="center">
<img src="3login_putty_03.png" width="480" title="add ssh tunnel" />
</div>

## アクセスサーバへの接続 {#login-to-access-server-with-putty}

ここでは、アクセスサーバの接続手順を説明します。

* 認証設定（アクセスサーバ）<a name="ssh-auth-as"></a>

    左のCategoryから、 Connection -> SSH -> Auth -> Credentialsを選択します。設定は以下のとおりです。

	| 項目 | 画面と設定値 |
	|:--|:--|
	| 設定画面 | <img src="3login_putty_04.png" width="480" title="auth info" /> |
	| 秘密鍵ファイル | 認証に使用する秘密鍵ファイルのパス |

* アクセスサーバへのログイン
     左のCategoryからSessionを選択し、アクセスサーバへのログイン情報を入力します。

	| 項目 | 画面と設定値 |
	|:--|:--|
	| 設定画面 | <img src="3login_putty_05.png" width="480" title="login info" /> |
	| ホスト名 | *as.v3.abci.ai* |

    入力が完了したら[Open]ボタンをクリックします。

    ABCIアカウントおよびパスフレーズを入力し、アクセスサーバにログインします。ログインに成功すると以下の画面が表示されます。ABCIにログイン中は、このログインセッションを維持する必要があります。

<div align="center">
<img src="3login_putty_07.png" width="540" title="login as success" />
</div>

!!! warning
    画面上で何らかのキーを入力するとSSH接続が切断されてしますので注意してください。

## インタラクティブノードへの接続 {#login-to-interactive-node-with-putty}

ここでは、インタラクティブノードへの接続手順を説明します。

* 認証設定（インタラクティブノード）

    新規にPuTTYを起動し、SSHの認証情報を設定します。インタラクティブノードの認証は、アクセスサーバと同じです。[アクセスサーバの認証方法](#ssh-auth-as)を参照し、SSHの認証情報を入力してください。

* インタラクティブノードへのログイン

    左のCategoryからSessionを選択し、インタラクティブノードへのログイン情報を入力します。

	| 項目 | 画面と設定値 |
	|:--|:--|
	| 設定画面 | <img src="3login_putty_08.png" width="480" title="login es" /> |
	| ホスト名 | localhost |
	| TCPポート | [SSHトンネル情報の設定](#ssh-tunnel-with-putty)で設定したポート番号 (画面例: 31022) |

    入力が完了したら[Open]ボタンをクリックします。

    ログインに成功すると、以下のような画面が表示されます。

<div align="center">
<img src="3login_putty_09.png" width="540" title="login es success" />
</div>
