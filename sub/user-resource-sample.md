# ユーザモデル管理画面作成例
ユーザモデルはメールアドレスのユニーク制限やパスワードのハッシュ化、ユーザ情報更新時のパスワード情報等の取り扱いに癖があるので、ここではFilamentでのそれらの情報の扱い方を解説

## まずはUser用リソースを作成
下記コマンドを実行してUserモデル用のFilamentリソースファイルを作成してください
```sh
php artisan make:filament-resource User
```

## 次にリソース編集部分を作成
作成されたFilamentのリソースファイルを下記のように変更します

```vim
app/Filament/Resources/UserResource.php
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
------------
↓
---after---
public static function form(Form $form): Form
{
    return $form
        ->schema([
            //名前
            Forms\Components\TextInput::make('name')->required()->label('名前'),

            //メアド：編集保存時にメアドのユニーク制限でエラーにならないようにignoreRecordをtrueにする
            Forms\Components\TextInput::make('email')->required()->label('メールアドレス')
                ->unique(ignoreRecord: true),

            //パスワード：パスワードのハッシュ化や、編集時に都度再入力を求められないようにdehydrateを使う
            Forms\Components\TextInput::make('password')
                ->password()
                ->dehydrateStateUsing(fn ($state) => Hash::make($state))
                ->dehydrated(fn ($state) => filled($state))
                ->required(fn (string $context): bool => $context === 'create')
                ->same('password_confirmation')
                ->label('パスワード'),

            //パスワード確認フィールド
            Forms\Components\TextInput::make('password_confirmation')
                ->password()
                ->required(fn (string $context): bool => $context === 'create')
                ->dehydrated(false)
                ->label('パスワード確認')
        ]);
}

public static function table(Table $table): Table
{
    return $table
        ->columns([
            Tables\Columns\TextColumn::make('name')->label('名前'),
            Tables\Columns\TextColumn::make('email')->label('メールアドレス'),
        ])
        ->filters([
            //
        ])
------------
```

## 作成された画面例
- 一覧画面
<img width="1328" alt="user1" src="https://user-images.githubusercontent.com/7894265/190982447-aa9e44a0-92c1-4e3e-ba68-dae02eb2d769.png">

- ユーザ新規作成
<img width="1328" alt="user2" src="https://user-images.githubusercontent.com/7894265/190982487-fbb53265-f70e-4a9d-8230-b822abc2005f.png">

- 作成すると右上に成功メッセージ
<img width="1328" alt="user3" src="https://user-images.githubusercontent.com/7894265/190982519-ea7b4088-052a-4816-8242-8106e9d0148c.png">

- 作成後の一覧画面
<img width="1328" alt="user4" src="https://user-images.githubusercontent.com/7894265/190982561-f95e200e-7d4c-412b-9b71-59b9ae47c793.png">

- 編集画面（名前：sample -> sample123に変更）
<img width="1328" alt="user5" src="https://user-images.githubusercontent.com/7894265/190982589-0de294d3-2016-48a8-8587-cafe7b8c6393.png">

- 保存すると右上に成功メッセージ。パスワードは何も入力しない時は既存のパスワードが温存される動き
<img width="1328" alt="user6" src="https://user-images.githubusercontent.com/7894265/190982765-9880af8d-af57-4f8b-9002-06d8f3498890.png">

- 編集後の一覧画面
<img width="1328" alt="user7" src="https://user-images.githubusercontent.com/7894265/190982832-8abb924b-d29b-437d-b218-613923e40038.png">


## オフィシャルドキュメント
詳しくはオフィシャルドキュメントを見よう<br>
👇
- [uniqueについて](https://filamentphp.com/docs/2.x/forms/validation#unique)
- [パスワードのハッシュかや編集時のTIPS](https://filamentphp.com/tricks/password-form-fields)
- [Dehydrationについて](https://filamentphp.com/docs/2.x/forms/advanced#dehydration)
