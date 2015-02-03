# Chapter 3

## What are Sprites

Spriteは, アニメーションさせたり  
rotation, position, scale, colorなどを含むpropertyの変更による変形されたりする2D画像  

## Creating Sprites

あなたが達成する必要のあるものに基づいてSpriteの作成方法は様々である  
フォーマットはPNG, JPEG, TIFFなどが使用可能  

### Creating a Sprite

次のコードのSpriteはmysprite.pngと同じサイズのSpriteを作成
```C++
auto mySprite = Sprite::create("mysprite.png");
```

### Creating a Sprite with a Rect

画像ファイルの一部のみをSpriteとして使用したい場合, Rectを使用することで対応できる  
Rectは4つの引数を取る( origin x, origin y, width, height )
Rectは左上で始まる
```C++
auto mySprite = Sprite::create("mysprite.png", Rect(0, 0, 40, 40));
```

## Creating a Sprite from a Sprit Sheet
