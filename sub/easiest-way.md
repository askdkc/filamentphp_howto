# 凄く楽する方法
Filamentのリソース作成時には自動生成オプションが用意されています[(オフィシャルドキュメントへのリンク)](https://filamentphp.com/docs/2.x/admin/resources/getting-started#automatically-generating-forms-and-tables)<br>
この方法を使うとモデルの持つ項目を基に一覧画面の表示部分やCRUD(作成、表示、更新、削除)機能画面を自動生成してくれます👍

## やり方

ここでは[トップページ](/README.md)の**Postモデルのファイルを作成し、マイグレーションを実施した直後の前提**で話を進めます

### 必要なパッケージのインストール

自動生成には `doctrine/dbal` パッケージが必要なので事前にインストールします

```sh
composer require doctrine/dbal
```

### モデルを基にしたリソースの自動生成コマンド

次のコマンドでPostモデルを対象としてFilamentのリソースを自動生成させます

```sh
php artisan make:filament-resource Post --generate
```

## 管理画面にアクセスして動作確認

管理画面にアクセスすると次のような状態になっています
<img width="1332" alt="ez1" src="https://user-images.githubusercontent.com/7894265/191151444-ecba137f-d024-4d81-9708-65a83c338e10.png">

New Post(作成)をクリックすると次のような新規データ作成画面に（テキストはダミーです）
<img width="1332" alt="ez2" src="https://user-images.githubusercontent.com/7894265/191151553-085dcf6d-c4e5-4bb4-9d1e-b499ee23f6c1.png">

保存してPostデータ一覧に戻るとデータが追加されています
<img width="1332" alt="ez3" src="https://user-images.githubusercontent.com/7894265/191151597-153c92d4-c4e0-437a-860b-1afb29c648ba.png">

何これめっちゃ楽🤯💕

## Userモデルの場合

ユーザモデルでも自動生成が使えますが、ユーザ情報更新の際のパスワードのような部分はdehydrate処理を使わないと上手くいかないので、[詳しくはこちらの説明](/sub/user-resource-sample.md)を参照してください

### Userリソース作成

```sh
php artisan make:filament-resource User --generate
```

### Userリソース管理画面サンプル画像

<img width="1332" alt="ezuser1" src="https://user-images.githubusercontent.com/7894265/191151630-ba887fac-8996-4154-9114-ea1c49d1673f.png">

Editをクリック後の画面
<img width="1332" alt="ezuser2" src="https://user-images.githubusercontent.com/7894265/191151635-a3eaecc2-d396-496f-a8a7-a570c663269b.png">

上記のまま"Save Change"をクリックするとPasswordが必須項目と警告が出る（正しくないというか、好ましくない動作）
<img width="1332" alt="ezuser3" src="https://user-images.githubusercontent.com/7894265/191151643-d1d2f949-5be8-4d70-8b8c-9113b7377715.png">

**Userモデルの場合**にも書きましたが、対処方法については[こちらの説明](/sub/user-resource-sample.md)を参照してください
