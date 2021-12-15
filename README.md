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

	IRIS-FHIR-SQLBuilder/data/patientdata　以下にサンプルデータがあります。

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

	http://localhost:57772/csp/healthshare/demo/fhir/r4/Patient

	※ポートは57772 です。ご注意を！


