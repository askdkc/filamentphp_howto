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

## before


## after


## オフィシャルドキュメント
詳しくはオフィシャルドキュメントを見よう<br>
👇
- [uniqueについて](https://filamentphp.com/docs/2.x/forms/validation#unique)
- [パスワードのハッシュかや編集時のTIPS](https://filamentphp.com/tricks/password-form-fields)
- [Dehydrationについて](https://filamentphp.com/docs/2.x/forms/advanced#dehydration)
