# 並び替え機能の追加

管理画面で登録データの並び替えしたい場合、Filamentでは手軽に機能を追加することができます

```vim
app/Filament/Resources/PostResource.php
---before---
public static function table(Table $table): Table
{
    return $table
        ->columns([
            Tables\Columns\TextColumn::make('title')->label('タイトル'),
            Tables\Columns\TextColumn::make('body')->label('本文'),
        ])
------------
↓
---after---
public static function table(Table $table): Table
{
    return $table
        ->columns([
            Tables\Columns\TextColumn::make('title')->label('タイトル')-->sortable(), //ここにsortableを追加
            Tables\Columns\TextColumn::make('body')->label('本文')->sortable(), //ここにsortableを追加
        ])
-----------
```

## before
<img width="1353" alt="before-sort" src="https://user-images.githubusercontent.com/7894265/190680657-3ade9e50-5c37-4df4-9ada-8779725a69c8.png">


## after
<img width="1353" alt="after-sort" src="https://user-images.githubusercontent.com/7894265/190680671-f18c41f1-81a3-4a0c-8eec-ae55be24d502.png">

## タイトルでソート時
<img width="1353" alt="title-sort" src="https://user-images.githubusercontent.com/7894265/190680977-17678bf5-07ef-43ef-9428-bc4809da067f.png">


あら便利💕
<br><br>
詳しくはオフィシャルドキュメントを参照👉 [Columns - Table Builder - Filament](https://filamentphp.com/docs/2.x/tables/columns#sorting)
