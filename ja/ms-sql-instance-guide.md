## Database > MS-SQL Instance > 使用ガイド

## MS-SQL Intance作成

MS-SQLを使用するには、先にインスタンスを作成する必要があります。
MS-SQL Instance作成 **ショートカット** ボタンをクリックすると **Compute > Instance > インスタンス作成** に移動します。

![mssqlinstance_01_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_01_201812_en.png)

MS-SQLイメージ選択後、追加設定完了後にインスタンスを作成します。
インスタンス作成の詳細内容は、[Instance概要](https://docs.toast.com/ko/Compute/Instance/ko/overview/)を参照してください。

インスタンス作成完了後、RDP(Remote Desktop Protocol)を活用してインスタンスにアクセスします。
インスタンスにFloating IPが接続されている必要があり、セキュリティーグループでTCPポート3389(RDP)が許可されている必要があります。
**+ パスワード確認** ボタンをクリックし、インスタンス作成時に設定したキーペアを使用してパスワードを確認します。

![mssqlinstance_02_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_02_201812_en.png)

**接続** ボタンをクリックし、.rdpファイルをダウンロードした後に、獲得したパスワードを使用してインスタンスに接続します。

## MS-SQLイメージ作成後の初期設定

### 1. SQL認証モード設定

サーバーの基本認証モードが「Windows認証モード」になっています。
MS-SQLのデータベースアカウントを使用するためにSQL認証モードに変更する必要があります。

Microsoft SQL Server Management Studioを実行して、インスタンス名でオブジェクトに接続します。

![mssqlinstance_03_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_03_201812_en.png)

1. オブジェクトを右クリックします。
2. メニューで **プロパティ**を選択します。
3. サーバープロパティウィンドウで **セキュリティー** メニューを選択します。
4. **サーバー認証** 方式を「SQL ServerおよびWindows認証モード」に変更します。

※ SQL認証モード設定後、適用するためにMS-SQLサービスを再起動する必要があります。

### 2. MS-SQLサービスポート変更

MS-SQLのデフォルトのサービスポート1433は、広く認知されているポートなので、セキュリティー脆弱性になることがあります。
次のポートに変更することを推奨します。
※ Expressの場合、デフォルトのポートが指定されていません。

SQL Server構成管理者を実行します。

![mssqlinstance_04_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_04_201812_en.png)

1. 左のチェンネルで **SQL Serverネットワーク構成**の下位項目 **MSSQLSERVERに対するプロトコル**を選択します。
2. プロトコル名の中から **TCP/IP**を右クリックします。
3. メニューで **プロパティ**を選択します。
4. **IPアドレス** タブをクリックします。
5. ドロップダウンメニューの中から **IP ALL**を選択し、別のポート番号に変更します。

※ MS-SQLサービスポート変更後、適用のためにMS-SQLサービスを再起動する必要があります。

### 3. 外部からのMS-SQLデータベース接続許容設定

外部からMS-SQLデータベースに接続するために、 **Network > Security Group** でMS-SQLサービスポートをSecurity Groupに追加する必要があります。
Security Groupに追加する時、接続を許可するMS-SQLサービスポート(基本ポート：1433)および遠隔IPを登録します。 

## データボリューム割り当て

MS-SQLのデータ/ログファイル(MDF/LDF)、バックアップファイルは別途のBlock Storageの使用を推奨します。
Block Storageを作成するには、**Compute > Instance > Block Storage** タブで + Block Storage作成ボタンをクリックします。

![mssqlinstance_05_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_05_201812_en.png)

Block Storage作成時、Volumeタイプは性能を考慮して「汎用SSD」の使用を推奨します。

![mssqlinstance_06_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_06_201812_en.png)

Block Storage作成完了後、Storageを選択し、 **接続管理** ボタンをクリックしてインスタンスに接続します。

<br/>

RDPでインスタンスに接続し、 **コンピュータ管理**を実行して **保存場所 > ディスクの管理**に移動します。

![mssqlinstance_07_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_07_201812_en.png)

接続されたBlock Storageが探知されたことを確認できます。使用するには先にディスク初期化を実行する必要があります。
1. **ディスク1** ブロックを右クリックした後、**ディスク初期化**をクリックします。
2. パーティション形式選択後、 **確認** ボタンをクリックします。

<br/>

初期化完了後、ディスクボリュームを作成します。

![mssqlinstance_08_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_08_201812_en.png)

割り当てられていないディスクを右クリックし、 **新しいシンプルボリューム**をクリックして新しいシンプルボリュームウィザードを進行します。

<br/>

Microsoft SQL Server Management Studioサーバープロパティのデータベース設定で、データベース基本位置を作成したボリュームのディレクトリに変更します。

![mssqlinstance_09_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_09_201812_en.png)

※ MS-SQLデータベース基本位置の変更後、適用のためにMS-SQLサービスを再起動する必要があります。

## MS-SQLサービス再起動
MS-SQLの設定変更時、MS-SQLサービスの再起動が必要な場合があります。
変更設定を適用するためにMS-SQLサービスを再起動します。

SQL Server構成管理者の **SQL Server構成管理者(ローカル) > SQL Serverサービス > SQL Server(MSSQLSERVER)** を選択後、右クリックして表示されるメニューにある「再起動」からMS-SQLサービスを再起動します。

![mssqlinstance_10_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_10_201812_en.png)

## MS-SQLサービス自動実行確認/設定
MS-SQLのサービスが、OS起動時に自動で起動するように設定されているかを確認します。 

SQL Server構成管理者のSQL Server構成管理者(ローカル) > SQL Serverサービスで「起動モード」を確認できます。 

![mssqlinstance_11_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_11_201812_en.png)

**SQL SERVER (MSSSQLSERVER)** および **SQL Serverエージェント(MSSQLSERVER)** などのサービス起動モードが **自動**ではない場合：
1. サービスを右クリックした後、 **プロパティ**を選択します。
2. **サービス**タブで **General > 起動モード**を **自動**に変更します。

> [参考]
> MS-SQL Instanceのリリース状況は、[インスタンスリリースノート](/Compute/Compute/ko/release-notes/)を参照してください。
