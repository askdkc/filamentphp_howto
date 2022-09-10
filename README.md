# Filamentの使い方
## 新規のLaravelプロジェクト作成
```sh
% laravel new filavel
% cd filavel
% touch database/database.sqlite
% vi .env
```

.envの編集
```vim
---before---
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=filavel
DB_USERNAME=root
DB_PASSWORD=
-------------
↓
---after---
DB_CONNECTION=sqlite
-----------
```

## Filamentのインストール
```sh
% composer require filament/filament:"^2.0"
```

## サンプルモデル作成
```sh
% php artisan make:model -m Post
% php artisan make:model -m Tag
```

### モデルのマイグレーション作成
```vim
database/migrations/yyyy_mm_dd_hhmmss_create_posts_table.php
--------
public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }
--------
↓
--------
public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title');	//追加
            $table->text('body');	//追加
            $table->timestamps();
        });
    }
--------
```

```vim
database/migrations/yyyy_mm_dd_hhmmss_create_tags_table.php
--------
public function up()
    {
        Schema::create('tags', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }
--------
↓
--------
public function up()
    {
        Schema::create('tags', function (Blueprint $table) {
            $table->id();
            $table->string('name')->unique();	//追加
            $table->timestamps();
        });
    }
--------

```

## モデルリレーションシップテーブル作成
```sh
php artisan make:migration create_post_tag_table
```

## リレーションシップのマイグレーション作成
```vim
database/migrations/yyyy_mm_dd_hhmmss_create_post_tag_table.php
--------
public function up()
    {
        Schema::create('post_tag', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }
--------
↓
--------
public function up()
    {
        Schema::create('post_tag', function (Blueprint $table) {
            $table->id();
            $table->unsignedInteger('post_id');
            $table->unsignedInteger('tag_id');
            $table->foreign('post_id')->references('id')->on('posts')->cascadeOnDelete();
            $table->foreign('tag_id')->references('id')->on('tags')->cascadeOnDelete();
            $table->unique(['post_id','tag_id']);
            $table->timestamps();
        });
    }
--------
```


## マイグレーション実行
```sh
% php artisan migrate
```

## Postモデルの設定
```vim
app/Models/Post.php
------
class Post extends Model
{
    use HasFactory;
    protected $fillable = ['title', 'body'];  //追加
	  //下記も追加
	  public function tags() 
	  {
	      return $this->belongsToMany(Tag::class);
	  }
}
------
```

## Tagモデルの設定
```vim
app/Models/Tag.php
------
class Tag extends Model
{
    use HasFactory;
    protected $fillable = ['name'];  //追加
}
------
```


## Filament用管理ユーザ作成
```sh
% php artisan make:filament-user

 Name:
 > admin (管理者ユーザの名前入力)

 Email address:
 > admin@localhost (管理者ユーザのメアド入力)

 Password:
 >  (管理者ユーザのパスワード入力)

uccess! admin@localhost may now log in at http://filavel.test/admin/login.

 Would you like to show some love by starring the repo? (yes/no) [yes]:
→ Filamentを気に入った時は yes と入力すると GitHub のリポに飛ぶので、スター押してね
```

## Filamentにログイン
```sh
% php artisan serve
```

ブラウザで http://localhost:8000/admin/login にアクセスする

ログイン認証画面が現れるので、管理ユーザ作成で入力したメールアドレスとパスワードでログインする
![8AF46A54-3446-4781-9153-649518B5964F](https://user-images.githubusercontent.com/7894265/189489414-bc295030-d4d8-4c0a-9831-83370be4340f.png)


## Filament用管理リソースを作成する
Filamentの管理画面でモデルのデータが扱えるようにリソースを作成します。
```sh
% php artisan make:filament-resource Post
% php artisan make:filament-resource Tag
```

PostResource.phpファイルを編集します
```vim
app/Filament/Resources/PostResource.php
---before---
public static function form(Form $form): Form
    {
        return $form
            ->schema([
                //
            ]);
    }

    public static function table(Table $table): Table
    {
        return $table
            ->columns([
                //
            ])
            ->filters([
                //
            ])
            ->actions([
                Tables\Actions\EditAction::make(),
            ])
            ->bulkActions([
                Tables\Actions\DeleteBulkAction::make(),
            ]);
    }
-----------
↓
---after---
public static function form(Form $form): Form
    {
        return $form
            ->schema([
                // ここに編集したい項目を追加する
                Forms\Components\TextInput::make('title')->required()->label('タイトル')
                    ->hint("ブログのタイトル入力"),
                Forms\Components\Textarea::make('body')->required()->label('本文')
                    ->helperText('本文を入力します')
                    ->columnSpan('full'),
            ]);
    }

    public static function table(Table $table): Table
    {
        return $table
            ->columns([
                // ここに表示したい項目を追加する
                Tables\Columns\TextColumn::make('title')->label('タイトル'),
                Tables\Columns\TextColumn::make('body')->label('本文'),
            ])
            ->filters([
                //
            ])
            ->actions([
                Tables\Actions\EditAction::make(),
            ])
            ->bulkActions([
                Tables\Actions\DeleteBulkAction::make(),
            ]);
    }
-----------
```

TagResource.phpファイルを編集します
```vim
app/Filament/Resources/TagResource.php
---before---
public static function form(Form $form): Form
    {
        return $form
            ->schema([
                //
            ]);
    }

    public static function table(Table $table): Table
    {
        return $table
            ->columns([
                //
            ])
            ->filters([
                //
            ])
            ->actions([
                Tables\Actions\EditAction::make(),
            ])
            ->bulkActions([
                Tables\Actions\DeleteBulkAction::make(),
            ]);
    }
-----------
↓
---after---
public static function form(Form $form): Form
    {
        return $form
            ->schema([
                // ここに編集したい項目を追加する
                Forms\Components\TextInput::make('name')->required()->label('タグ')
                    ->hint("タグ名を入力"),
            ]);
    }

    public static function table(Table $table): Table
    {
        return $table
            ->columns([
                // ここに表示したい項目を追加する
                Tables\Columns\TextColumn::make('name')->label('タグ'),
            ])
            ->filters([
                //
            ])
            ->actions([
                Tables\Actions\EditAction::make(),
            ])
            ->bulkActions([
                Tables\Actions\DeleteBulkAction::make(),
            ]);
    }
-----------
```

### 管理画面の変更確認
管理画面にアクセスすると作成した各モデルリソースが表示されている

![149305F4-7BA3-4255-89AD-A46B215669D5](https://user-images.githubusercontent.com/7894265/189489440-5203ec0a-e782-4e85-8d86-7e058c75b4da.png)


![A5428CCF-E8B2-4261-B7FE-3AE63BEAA388](https://user-images.githubusercontent.com/7894265/189489448-c1e4937b-592c-42d1-a97b-21d7f5890736.png)


Postsリソースの右側の”New Post”をクリックして新規データを作成してみよう

![59CCB38C-1F02-412F-808A-E0826D998B47](https://user-images.githubusercontent.com/7894265/189489463-84ce7afb-6372-46c7-86cb-d757ac6c8be7.png)

タイトルと本文を入力して”Create”をクリック
![9CFE0CF3-5736-45EF-9168-8BCBE4BA4EB5](https://user-images.githubusercontent.com/7894265/189489470-06531dde-4942-4a33-9baa-56711229a71e.png)

右上に”Created”と表示されたら作成成功。左側のメニューのPostsをクリックして作成されたデータを見てみよう。
![435AB861-0F33-4E78-AA68-13579A31524F](https://user-images.githubusercontent.com/7894265/189489480-55146922-1930-4e14-b528-9ea60db95f3b.png)

作成したデータが一覧に表示される
![800B4CDA-56A0-4190-9E08-DAE863783CA7](https://user-images.githubusercontent.com/7894265/189489502-f62acb32-6d0c-46d7-b8de-e835bcd2ec95.png)


## Filamentメニューの日本語化
そのままだとFilamentの各種メニューは英語表記なので、configを修正しここを日本語化しましょう。
```vim
config/app.php
---before---
    'locale' => 'en',
------------
↓
---after---
    'locale' => 'ja',
-----------
```

再度管理画面にアクセスするとメニューが日本語に変わっています
![6F7D2D95-F8BC-497E-A346-B53B76E303C2](https://user-images.githubusercontent.com/7894265/189489522-f4346846-d3aa-43f2-88a6-06a52b793003.png)

作成ボタンをクリック後も各種ボタンやラベルが日本語表記になります
![470FAB7A-C36C-4158-AE78-A603B63ED084](https://user-images.githubusercontent.com/7894265/189489531-5ab48d33-a783-40fd-bd1d-33e1a9d67a77.png)

## Filamentリレーションリソースの追加
```sh
% php artisan make:filament-relation-manager PostResource tags name
```

### PostモデルからTagの新規作成＆割当を可能にするボタン追加
```vim
app/Filament/Resources/PostResource/RelationManagers/TagsRelationManager.php
---before---
public static function table(Table $table): Table
    {
        return $table
            ->columns([
                Tables\Columns\TextColumn::make('name'),
            ])
            ->filters([
                //
            ])
            ->headerActions([
                Tables\Actions\CreateAction::make(),
            ])
            (省略)
    }    
------------
↓
---after---
public static function table(Table $table): Table
    {
        return $table
            ->columns([
                Tables\Columns\TextColumn::make('name'),
            ])
            ->filters([
                //
            ])
            ->headerActions([
                Tables\Actions\CreateAction::make(),
                Tables\Actions\AttachAction::make()->preloadRecordSelect()->label('タグ追加'), //追加
            ])
            (省略)
    }    
-----------
```


### PostResourceにTagとのリレーションシップを追加
```vim
app/Filament/Resources/PostResource.ph
---before---
public static function getRelations(): array
    {
        return [
            //
        ];
    }
------------
↓
---after---
public static function getRelations(): array
    {
        return [
            PostResource\RelationManagers\TagsRelationManager::class, //追加
        ];
    }
------------
```

### 管理画面にアクセスし動作確認
Tagの右側にある作成をクリック
![9E7534FC-6B64-4FE4-B178-C6A852C9571C](https://user-images.githubusercontent.com/7894265/189489554-cf72439f-8402-4f56-b0cf-5bc090032778.png)

作成ウィンドウが開くのでタグ名を入力し作成をクリック
![11307C25-7A35-426F-8BA4-8475080720EB](https://user-images.githubusercontent.com/7894265/189489563-5e976a91-28a7-4975-9b58-33fe5bca831f.png)

Postデータに新規に作成されたTagデータが紐づきます
![14A98190-BA8F-4288-BAA0-C6C2C9A45485](https://user-images.githubusercontent.com/7894265/189489569-3b0f6cea-71ea-41e2-88d7-456a7291d334.png)

### 既存データの追加
他のタグが存在する場合「タグ追加」ボタンから、既存のタグを付与することもできます。
![46084205-C9AE-400E-BBED-AEC4526898CE](https://user-images.githubusercontent.com/7894265/189489582-10076f3a-dd49-4df6-bd92-c5d84eb3455d.png)

Postデータから編集をクリックし、表示された編集画面のTagの右端にある「タグ追加」ボタンをクリック
![D32E255A-CB34-4B5F-9766-FFE2A1FF3D4B](https://user-images.githubusercontent.com/7894265/189489594-9a7c9f63-3fc4-45ca-be31-dc2d7d263631.png)

表示されるタグ追加ウィンドウから既存のデータがドロップダウンや検索機能による絞り込みで選べます
![4D1AD9C1-A8BA-4661-B518-D6B5D14636DD](https://user-images.githubusercontent.com/7894265/189489602-e9a95317-70b0-4fc6-a877-4402113d225d.png)

より詳しい情報はオフィシャルドキュメントを読もう
[Installation - Admin Panel - Filament](https://filamentphp.com/docs)

