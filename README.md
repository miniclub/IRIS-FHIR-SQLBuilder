# IRIS-FHIR-SQLBuilder

## 開始手順

1) git clone ＋ ディレクトリ移動

	```
	git clone https://github.com/iijimam/IRIS-FHIR-SQLBuilder.git
	cd IRIS-FHIR-SQLBuilder
	```

2)  IRISのイメージをダウンロードしてロードする

	http://kits-web/kits/unreleased/Docker/2022.1.0DEVIH/

	ここから最新をDLします。

	DLした後、以下実行してイメージをロードします（ダウンロードしたファイル名に合わせて実行例を変更してください）

	```
	gzip -d irishealth-2022.1.0DEVIH.39.0-docker.tar.gz
	docker load -i irishealth-2022.1.0DEVIH.39.0-docker.tar
	```

3) docker-compose.ymlの変更

 	2.でダウンロードしたイメージ:タグ名に合わせないといけないので、docker-compose.ymlの5行目を修正します。

	例）
	```
	image:  intersystems/irishealth:2022.1.0DEVIH.31.0
	```

4) setup.shを実行します

	コンテナはDurable %SYSを使うため、使用するディレクトリのchmodを実行します。


	```
	./setup.sh
	```

5) iris.keyの準備

	コンテナ用iris.keyを以下ディレクトリに置いてください。

	IRIS-FHIR-SQLBuilder/data/iris.key


6) コンテナ開始

	```
	docker-compose up -d
	```

7) リポジトリの準備＋サンプルデータロード

	IRIS-FHIR-SQLBuilder/data/fhirdata　以下にサンプルデータがあります。

	リポジトリを作りながらサンプルデータのロードを行うユーティリティをコンテナにログインし、IRISにログインした後実行します。

	作成されるリポジトリはDEMOです

	```
	docker exec -it sqlbuilder bash
	iris session iris
	ZN "HSLIB"
	Do ##class(HS.HC.FHIRSQL.Utils.Setup).Setup("/data/fhirdata/")
	```


8)　リポジトリの確認：GET要求実行

	ユーザ名 _system  パスワード SYS でアクセスできます。
	(acceptタグにapplication/fhir+jsonを追加する必要があります)

	http://localhost:57772/csp/healthshare/demo/fhir/r4/Patient

	※ポートは57772 です。ご注意を！

9)　SQLビルダ画面へのアクセス

	http://localhost:57772/csp/fhirsql/index.csp#/ にアクセスします。
	(以降はhttps://docs.intersystems.com/components/csp/docbook/DocBook.UI.Page.cls?KEY=AFIHR_SQLBUILDER を参照します。)

10)　SQLAnalysisの設定
	Analysisの右にある「New」ボタンをクリックします。
	「New FHIR Analysis」というダイアログが表示されます。ここで分析するFHIRリポジトリを指定します。
	ない場合は「New」ボタンをクリックします。
	
	「New FHIR Repository Configuration」にFHIRリポジトリのサイト情報を入力します。

	*  Name
		FHIR リポジトリ名（なんでも構いません）。今回はDEMOとしています。
	* Host
		FHIRリポジトリのホスト名またはIPアドレス。今回はlocalhost
	* Port
		ポート番号。今回は57772
	* SSL Configuration
		HTTPS通信を行う場合はそのSSL定義名を入力します。
	* Credential
		FHIRリポジトリにアクセスするときのユーザ名、パスワード。「New」ボタンで新規作成できます。今回はUsernameは_SYSTEM、パスワードはSYS
	* FHIR Repository URL
		FHIRリポジトリのURLを選択します。/csp/healthshare/demo/fhir/r4を選択します。

	「Save」ボタンで「New FHIR Analysis」画面に戻ります。
	
	Selectivity Percentageに分析を行うレコードの全体に対するパーセンテージを入力します。
	すべてを分析するときは100を入力します。

	「Launch Analysis Task」をクリックすると、分析が開始されます。

11)　Translation Specificationの設定
同じく右にある「New」ボタンをクリックします。
「New Translation Specification」ダイアログが表示されます。
NameにTranslation名を入力します。Analysis欄に先ほど設定したFHIRリポジトリの設定を選択します。
「Create Transformation Specification」ボタンをクリックします。

「Edit Transformation Specification xxx」という画面が表示されますので、一覧として必要なFHIRリソース
	

	
	
