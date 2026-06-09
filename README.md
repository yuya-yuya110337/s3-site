# AWS学習ログ（S3 静的サイトホスティング）

## ■ 概要
自作ポートフォリオサイトを AWS S3 の静的ウェブサイトホスティング機能を利用して公開しました。  
GitHub でコードを管理し、S3 を公開環境として利用する構成です。  
無料枠がなくても、S3 は保存容量と通信量に応じた従量課金のため、今回のような小規模サイトは **実質無料で運用可能** です。

---

## ■ 実施した手順

### ① ローカル環境の準備
- `Documents/s3-site` フォルダを作成
- `index.html`、`style.css`、画像フォルダなどを配置

### ② GitHub リポジトリの作成
- `s3-site` という名前でリポジトリを作成
- ローカルのファイルをアップロードし、バージョン管理を開始

### ③ S3 バケットの作成
- バケット名：`yuya-s3-site`
- リージョン：ap-northeast-1（東京）
- Block Public Access を OFF に設定
- 「このバケットを公開することを理解しています」にチェック

### ④ 静的ウェブサイトホスティングの有効化
- プロパティ → 静的ウェブサイトホスティング → 有効化
- Index Document：`index.html`

### ⑤ ファイルのアップロード
- S3 の「オブジェクト」タブから、ローカルの `s3-site` フォルダ内のファイルをアップロード

### ⑥ 公開設定（バケットポリシー）
以下のポリシーを設定し、外部からアクセス可能にした：

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::yuya-s3-site/*"
    }
  ]
}

## 学んだこと
- S3の静的ホスティングは無料枠がなくてもほぼ無料で運用可能
- 公開にはBlock Public Accessの設定変更が必須
- GitHubとS3を併用することでコード管理と公開環境を分離できる
- 今後はCloudFrontやGitHub Actionsで自動化を検討

## 公開URL
http://yuya-s3-site.s3-website-ap-northeast-1.amazonaws.com