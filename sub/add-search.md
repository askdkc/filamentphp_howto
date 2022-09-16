# 検索機能の追加

管理画面で登録データの項目データを検索したい場合、Filamentでは手軽に機能を追加することができます

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

## before
<img width="1353" alt="before-search-feature" src="https://user-images.githubusercontent.com/7894265/190614892-9c3580cc-fabc-40be-8471-9453c08b6876.png">

## after
<img width="1353" alt="after-search-feature" src="https://user-images.githubusercontent.com/7894265/190614940-06f6bc1a-f16b-4ad3-9a4d-6cf4363b24aa.png">


