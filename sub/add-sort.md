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


## after


あら便利💕
<br><br>
詳しくはオフィシャルドキュメントを参照👉 [Columns - Table Builder - Filament](https://filamentphp.com/docs/2.x/tables/columns#sorting)
