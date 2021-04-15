
# データセットの公開

ABCIクラウドストレージを用いてデータセットを一般に公開することができます。[ABCI約款・規約](https://abci.ai/ja/how_to_use/) や [ABCIクラウドストレージ規約](https://abci.ai/ja/how_to_use/data/cloudstorage-agreement.pdf) に改めて目を通し、そのデータセットの公開が本当に適切であるかをご確認の上、以下の手順に従って、データセットの公開設定と登録を行って下さい。


## 1. 公開設定 {#publishing}

ABCIクラウドストレージにデータをアップロードし、[アクセス制御(1)](acl.md) を参考に公開設定（パブリックアクセスの設定）を行って下さい。

以下では、ABCIクラウドストレージに example-dataset というバケットを作り、sensor1 というディレクトリに格納された複数のファイルをアップロードし、公開設定を行う例を示します。

```
[username@es1 ~]$ module load aws-cli
[username@es1 ~]$ aws --endpoint-url https://s3.abci.ai s3 mb s3://example-dataset
[username@es1 ~]$ aws --endpoint-url https://s3.abci.ai s3api put-bucket-acl --acl public-read --bucket example-dataset
[username@es1 ~]$ aws --endpoint-url https://s3.abci.ai s3 cp --acl public-read --recursive sensor1 s3://example-dataset/sensor1
upload: sensor1/0003.dat to s3://example-dataset/sensor1/0003.dat
upload: sensor1/0001.dat to s3://example-dataset/sensor1/0001.dat
upload: sensor1/0002.dat to s3://example-dataset/sensor1/0002.dat
:
```

上記ではアップロート時に `--acl public-read` を指定しているため、アップロードが完了後、ABCIの外部から `https://example-dataset.s3.abci.ai/sensor1/0001.dat` 等の URL でアクセスができるようになります。

<!-- データセットの利用者がダウンロードできるように、これらの URL のリストを用意して下さい。-->


## 2. 公開データセットの登録 {#registration}

まず、公開するデータセットに関する基本情報を記入したYAMLファイルを準備して下さい。[テンプレートファイル (dataset_info_template.yaml)](https://datasets.abci.ai/dataset_info_template.yaml) を手元にダウンロードし、下記を参考に記入して下さい。

<!--UsageInfo には、後述の index.html または別途用意するページの URL を記入します。UsageInfo には、データファイルまたはデータファイルのリストが記載されているページの URL を記入します。-->

```yaml
# データセット名  *必須
Name: ABC Dataset

# データセットの概要 (50文字以上)  *必須
Description: This is a fictitious dataset ....

# データセットに関するキーワード (object detection, vehicle, action recognition, earth observation, etc.)  *必須
# 項目が1つであっても、下記のようなハイフンではじまるリスト形式で記入してください
Keywords:
  - image processing
  - health

# より詳細な情報を記した Web ページ　*任意
UsageInfo: https://example-dataset.s3-website.abci.ai/

# 配布方法
#   EncodingFormat: XML, CSV, GeoTIFF, etc.  *任意
#   ContentURL: 公開データの URL　*必須
Distribution:
  EncodingFormat: DICOM
  ContentURL:  # 項目が1つであっても、下記のようなハイフンではじまるリスト形式で記入してください
    - https://example-dataset.s3.abci.ai/abc.zip

# 作成者
#   Name: 組織名(または個人名)  *必須
#   URL: 組織のWEBサイト(または個人のWEBサイト)  *任意
#   ContactPoint:
#     Email: 連絡先メールアドレス  *Email or URL のいずれか必要
#     URL: 問い合わせ窓口のページ  *Email or URL のいずれか必要
Creator:
  Name: ABC Team
  URL: https://example.com/about/
  ContactPoint:
    Email: dataset@example.com
    URL: https://example.com/contact/

# ライセンス
#   Name: MIT License, CC-BY-SA 4.0, Custom License, etc.  *必須
#   URL: ライセンスが記述されているページのURL  *任意
License:
  Name: Custom License
  URL: https://example.com/dataset/LISENCE.txt

# バージョン
Version: "1.1b"

# データ作成日、修正日、公開日
# 下記のような ISO 8601 フォーマットで記載してください。記入する場合でも時間の部分は省略可能です。
DateCreated: 2020-04-18
DateModified: 2020-04-20T22:30:10+09:00
DatePublished: 2020-04-19

# DOI 等の識別子
#Identifier: https://doi.org/1234....

# 関連文献等の情報。項目が1つであっても、ContentURL と同じくハイフンを使ったリスト形式で記入してください。
Citation: 
- John, Doe. "Example Paper 1," presented at Example Conf., Los Angeles, CA, USA, Oct. 8-10, 2020.
- John, Doe. "Example Paper 3," in 23rd Example Conf., London, U.K., Aug. 2015. [Online]. Available: https://example.com/papers/23-5.pdf

# 詳細な説明や追加情報等  *任意
#AdditionalInfo:
```

データセットを公開しているバケットに、記入したYAMLファイルをアップロードして下さい。 `my_dataset_info.yaml` は違うファイル名でも問題ありません。

```
[username@es1 ~]$ aws --endpoint-url https://s3.abci.ai s3 cp --acl public-read my_dataset_info.yaml s3://example-dataset/my_dataset_info.yaml
```

上記が完了したら、以下のフォーマットで <abci-application-ml@aist.go.jp> までお知らせ下さい。複数のABCIグループに所属している場合、データセットがアップロードされているバケットを所有しているABCIグループを記載して下さい。

```text
Subject: データセットの公開(<データセット名>)

申請者の氏名: 
ABCIグループ: 
申請者のメールアドレス: 
アップロードしたYAMLファイルのURL: 
```

登録が完了したデータセットは <https://datasets.abci.ai/> に一覧表示されます。
