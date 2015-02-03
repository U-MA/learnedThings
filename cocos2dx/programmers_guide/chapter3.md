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

Sprite sheetは複数のSpriteを一つのファイルにまとめる方法  
これはそれぞれSpriteに対する個々のファイルを保持しておくより全体のサイズを減らす  
つまり, メモリの使用, ファイルサイズ, 読み込み時間を減らすことができる  
  
Sprite Sheetを使用する際, Sprite Sheetは最初にSpriteFrameCacheにloadされる  
SpriteFrameCacheは将来の素早いアクセスに対してSpriteFrameを保持するchachingクラス
SpriteFrameは画像ファイル名とSpriteのサイズを表すRectから成る
SpriteFrameCacheはSpriteFrameを複数回読み込む必要性を排除する
そのSpriteFrameは一度loadされ, SpriteFrameCacheに保持される

### Loading a Sprite Sheet

Sprite SheetをSpriteFrameCache, おそらくAppDelegateにloadする
```C++
// load the Sprite Sheet
auto spritecache = SpriteFrameChche::getInstance();

// the .plist file can be generated with any of the tools mentioned below
spritecache->addSpriteFramesWithFile("sprites.plist");
```

今, SpriteFrameCacheにloadされたSprite Sheetがある  
これを利用することでSpriteオブジェクトを作成できる  

### Creating a Sprite from SpriteFrameCache

```C++
// Our .plist file has names for each of the sprites in it.
// We'll grab the sprite named. "Blue_Front`" from the sprite sheet.
auto mysprite = Sprite::createWithSpriteFrameName("Blue_Front1.png");
```

### Creating a Sprite from a SpriteFrame

// this is equivalent to the previous example,
// but it is created by retrieving the spriteframe from the cache.
auto newspriteFrame = SpriteFrameCache::getInstance()->getSpriteFramebyName("Blue_Front1.png");
auto newSprite = Sprite::createWithSpriteFrame(newspriteFrame);
```

### Tools for creating Sprite Sheets

Sprite Sheetを生成するツールが存在 

* Cocos Studio
* Texture Packer
* Zwoptex

## Sprite Manipulation

Spriteを作成した後, 様々なpropertyにアクセスする  
```C++
auto mySprite = Sprite::create("mysprite.png");
```

### Anchor Point and Position

Anchor Pointは, Spriteのpositionを設定する際に使われる部分を明記する座標  
Anchor Pointは, scale, rotation, skewのようなpropertyに影響する
Anchor Pointは左下座標系を使用  
NodeオブジェクトのデフォルトのAnchor Pointは(0.5, 0.5)
```C++
// DEFAULT anchor point for all Sprites
mySprite->setAnchorPoint(0.5, 0.5);

// bottom left
mySprite->setAnchorPoint(0, 0);

// top left
mySprite->setAnchorPoint(0, 1);

// bottom right
mySprite->setAnchorPoint(1, 0);

// top right
mySprite->setAnchorPoint(1, 1);
```

### Sprite properties effected by anchor point

Anchor Pointは, scale, rotation, skewに影響を及ぼす  

#### Position

```C++
// position a sprite to a specific position of x = 100, y = 200
mySprite->setPosition(Vec2(100, 200));
```

#### Rotation

positive value => clockwise  
negative value => counter-clockwise  
デフォルト値は0  

```C++
// rotate sprite by +20 degrees
mySprite->setRotation(20.0f);

// rotate sprite by -20 degrees
mySprite->setRotation(-20.0f);

// rotate sprite by +60 degrees
mySprite->setRotation(60.0f);

// rotate sprite by -60 degrees
mySprite->setRotation(-60.0f);
```

#### Scale

```C++
// increases X and Y size by 2.0 uniformly
mySprite->setScale(2.0);

// increases just X scale by 2.0
mySprite->setScaleX(2.0);

// increases just Y scale by 2.0
mySprite->setScaleY(2.0);
```

#### Skew

```C++
// adjusts the X skew by 20.0
mySprite->setSkewX(20.0f);

// adjusts the Y skew by 20.0
mySprite->setSkewY(20.0f);
```

### Sprite properties not affected by anchor point

#### Color

色を変更するにはColor3Bオブジェクトを使用する  
Color3BオブジェクトはRGB値から成る  
Cocos2d-xでは事前定義された色が提供されている  
例えば, Color3B::White and Color3B::Red
```C++
// set the color by passing in a pre-defined Color3B object.
mySprite->setColor(Color3B::WHITE);

// set the color by passing in a Color3B object.
mySprite->setColor(Color3B(255, 255, 255)); // Same as Color3B::WHITE
```

#### Opacity

opacity値 [0, 255]
0は透明, 255は不透明
デフォルト値は255
```C++
// set the opacity to 30, which makes this sprite 11.7% opaque.
// (30 divided by 256 equals 0.1171875...)
mySprite->setOpacity(30);
```
