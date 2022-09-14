# データ作成/更新時にリダイレクト先を変えたい
該当のファイルに作成時や更新時のリダイレクトURLを指定することで動作を変更できます<br>
ここではPostモデルの例を記載します<br><br>
他のモデルの時は<br>
`app/Filament/Resources/(モデル名)Resource/`<br>
にファイルパスを置き換えたファイルを変更してください

## データ作成時のリダイレクト先変更方法
```vim
app/Filament/Resources/PostResource/Pages/CreatePost.php
---before---
class CreatePost extends CreateRecord
{
    protected static string $resource = PostResource::class;
}
------------
↓
---after---
class CreatePost extends CreateRecord
{
    protected static string $resource = PostResource::class;

    // ここを追加します
    protected function getRedirectUrl(): string
    {
        return $this->getResource()::getUrl('index');
    }
}
-----------
```

## データ更新時のリダイレクト先変更方法
```vim
app/Filament/Resources/PostResource/Pages/EditPost.php
---before---
class EditPost extends EditRecord
{
    protected static string $resource = PostResource::class;

    protected function getActions(): array
    {
        return [
            Actions\DeleteAction::make(),
        ];
    }
}
------------
↓
---after---
class EditPost extends EditRecord
{
    protected static string $resource = PostResource::class;

    protected function getActions(): array
    {
        return [
            Actions\DeleteAction::make(),
        ];
    }

    protected function getRedirectUrl(): string
    {
        return $this->getResource()::getUrl('index');
    }
}
-----------
```

より詳しい情報はオフィシャルドキュメントを読もう👉 
[Customizing form redirects - Filament](https://filamentphp.com/docs/2.x/admin/resources/creating-records#customizing-form-redirects)