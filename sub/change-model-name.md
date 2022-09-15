# モデル名を好きな名前に変えたい
初期の状態では左のメニューバーやモデル一覧画面のタイトルにはLaravelで作成したモデルのファイル名の表示がされてます。例：app/Models/Post.php → Post。これを「ブログ記事」のように変えたい場合の修正箇所は下記になります。

```vim
---before---
class PostResource extends Resource
{
    protected static ?string $model = Post::class;

    protected static ?string $navigationIcon = 'heroicon-o-collection';

    public static function form(Form $form): Form
------------
↓
---after---
class PostResource extends Resource
{
    protected static ?string $model = Post::class;

    protected static ?string $navigationIcon = 'heroicon-o-collection';

    protected static ?string $modelLabel = 'ブログ記事'; //ここを追加

    public static function form(Form $form): Form

-----------
```

