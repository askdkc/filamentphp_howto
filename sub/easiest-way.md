# 凄く楽する方法
Filamentのリソース作成時には自動生成オプションが用意されています<br>
この方法を使うとモデルの持つ項目を基に一覧画面の表示部分やCRUD(作成、表示、更新、削除)機能画面を自動生成してくれます👍

## やり方

ここでは[トップページ](/README.md)のPostモデルのファイルを作成し、マイグレーションを実施した直後の前提で話を進めます

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


## Userモデルの場合

ユーザモデルでも自動生成が使えますが、ユーザ情報更新の際のパスワードのような部分はdehydrate処理を使わないと上手くいかないので、[詳しくはこちらの説明](/sub/user-resource-sample.md)を参照してください

### Userリソース作成

```sh
php artisan make:filament-resource User --generate
```

### Userリソース管理画面サンプル画像


Editをクリック


Save ChangeをクリックするとPasswordが必須項目と警告が出る（正しくない）
