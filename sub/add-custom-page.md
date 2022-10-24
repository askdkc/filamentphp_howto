# カスタムページの作り方
Filamentでは個別ページを作成する機能も用意されています

## カスタムページの作成
下記のコマンドを実行してカスタムページを作ってみましょう
```bash
php artisan make:filament-page Imports

 (Optional) Resource (e.g. `UserResource`):
 >   （ここは何も入れずにEnterキーをおします）
```
実行すると下記の2つのファイルが作成されます
- `app/Filament/Pages/Imports.php`
- `resources/views/filament/pages/imports.blade.php`

ダッシュボードにアクセスすると何も無いブランクページが作成されています

<img width="1148" alt="image" src="https://user-images.githubusercontent.com/7894265/197438835-70849a2e-7f92-442e-be71-ebbb66c2d9a1.png">

## カスタムコンポーネントを読み込ませてみよう
ブログ記事用のPostモデルにCSVファイルからデータを読み込ませるカスタムコンポーネントをこのページに設定してみましょう

### コンポーネントパッケージのインストール
手前味噌ですが、Livewireを使用してお手軽にCSVデータをインポートするパッケージを作ってありますので、こちらを本Laravelプロジェクトにインストールします
```bash
composer require askdkc/livewire-csv
```
インストール後、下記のコマンドを実行し必要なセットアップを実行します
```bash
php artisan livecsv-setup

Preparing Livewire-CSV necessary migrations / 必要なマイグレーションを準備します
Publishing migration... / マイグレーションファイル準備中
Migration Jobs created successfully / Jobsテーブル作成
Migration Job Batches created successfully / Job Batches テーブル作成
Migration Csv Imports created successfully / Csv Importsテーブル作成
Adding Japanese Validation file... / 日本語のバリデーションファイルを作成します
Done! / 完了!

 Would you like to run the migrations now? / マイグレーションを実行しますか? (yes/no) [no]:
 > yes  ←ここはyesと入力してください

Running migrations... / 実行中...
   INFO  Running migrations.  

  2022_10_24_023104_create_jobs_table .............................. 10ms DONE
  2022_10_24_023105_create_csv_imports_table ........................ 2ms DONE
  2022_10_24_023106_create_job_batches_table ........................ 1ms DONE

 Would you like to star our repo on GitHub? 
 GitHubリポジトリにスターの御協力をお願いします🙏 (yes/no) [no]:
 > yes ← よかったらGitHubリポにスターをお願いします

```

### コンポーネントをカスタムページに設定する
それではインストールされたLivewireコンポーネントをカスタムページに読み込ませてみましょう

```vim
resources/views/filament/pages/imports.blade.php を開いて編集します

---before---
<x-filament::page>

</x-filament::page>
------------
↓
---after---
<x-filament::page>
<x-csv-button>データインポート</x-csv-button>

<livewire:csv-importer :model="App\Models\Post::class"
                        :columns-to-map="['id','title', 'body']"
                        :required-columns="['title', 'body']"
                        :column-labels="[
                            'id' => 'ID',
                            'title' => 'タイトル',
                            'body' => '本文',
                        ]" />
</x-filament::page>
-----------

```

ダッシュボードに追加されたImportsにアクセスし直すと下記のような表示になります

<img width="1148" alt="image" src="https://user-images.githubusercontent.com/7894265/197439825-0d63e370-15cf-40aa-8ed5-749bbf6d21ef.png">

`データインポート`をクリックするとインポート用ウィンドウが画面右側からにょっきり生えてきます
<img width="1449" alt="image" src="https://user-images.githubusercontent.com/7894265/197439973-82fcacad-aed1-4d2c-a568-56f4ec81c35b.png">

ただし、Filamentのデフォルトのスタイルと相性が悪くてインポート用のボタンが見えません
![image](https://user-images.githubusercontent.com/7894265/197440187-650a99cd-c221-42c5-80e9-eeda59912b79.png)

### コンポーネントのカスタマイズ
インポート用のLivewireコンポーネントのViewファイルを`vendor:publish`してカスタマイズします

```bash
php artisan vendor:publish --tag=livewire-csv-views


   INFO  Publishing [livewire-csv-views] assets.  

  Copying directory [vendor/askdkc/livewire-csv/resources/views] to [resources/views/vendor/livewire-csv]  DONE

```

`resources/views/vendor/livewire-csv/livewire/tailwindcss/csv-importer.blade.php` を開いて編集します

```vim
Filamentのカラーテーマはカスタムカラーを使っているので、そちらに合わせます(indigo→primary)

---before---
<button type="submit" class="ml-4 ...(中略)... bg-indigo-600 （後略） }}>{{ __('Import') }}</button>
------------
↓
---after---
<button type="submit" class="ml-4 ...(中略)... bg-primary-600 （後略） }}>{{ __('Import') }}</button>
-----------
```
