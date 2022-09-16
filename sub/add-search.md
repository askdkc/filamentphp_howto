# 検索機能の追加

管理画面で登録データを検索機能で探し出したり絞り込みたい場合に、Filamentでは手軽に機能を追加することができます

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
            Tables\Columns\TextColumn::make('title')->label('タイトル')->searchable(), //ここにsearchableを追加
            Tables\Columns\TextColumn::make('body')->label('本文')->searchable(), //ここにsearchableを追加
        ])
-----------
```

