# Chapter 5

## What is a Scene?

Sceneはゲームに必要になるSprite, Label, Nodeやその他オブジェクトを保持するコンテナ  
Sceneはゲームロジックの実行と要素を描画を担当している  

## Creating a Scene

```C++
auto myScene = Scene::create();
```

# Remember the Scene Graph?

Chapter 2でやった  

# A Simple Scene

Cocos2d-xでは、右手座標系を使用している  
すなわち, (0, 0)はスクリーンの左下を表す  
```C++
auto dirs = Director::getInstance();
Size visibleSize = dirs->getVisitbleSize();

auto scene1 = Scene::create();

auto label1 = Label::createWithTTF("MyGame", "Marker Felt.ttf", 36);
label1->setPosition(Vec2(visibleSize.width / 2, visibleSize.height / 2));

scene->addChild(label1);

auto sprite1 = Sprite::create("mysprite.png");
sprite1->setPosition(Vec2(100, 100));

scene->addChild(sprite1);
```

## Transitioning between Scenes

Cocos2d-xでは多くのsceneの遷移方法を提供している  

### Ways to transition between Scenes

```C++
auto myScene = Scene::create();
```

runWithSceneは最初のsceneにのみ使用する
```C++
Director::getInstance()->runWithScene(myScene);
```

replaceSceneは直接あるシーンに置き換える
```C++
Director::getInstance()->replaceScene(myScene);
```

pushSceneは現在実行されているsceneを中断し, 今のsceneを中断されたsceneのスタックに積む
```C++
Director::getInstance()->pushScene(myScene);
```

popSceneは実行しているsceneを置き換える  
実行しているsceneは消去される  
```C++
Director::getInstance()->popScene(myScene);
```

### Transition Scenes with effecs

Scene遷移に対してvisual effectを追加できる  
```C++
auto myScene = Scene::create();

// Transition Fade
Director::getInstance()->replaceScene(TransitionFade::create(0.5, myScene, Color3B(0, 255, 255)));

// FlipX
Director::getInstance()->replaceScene(TransitionFlipX::create(2, myScene));

// Transition Slide In
Director::getInstance()->replaceScene(TransitionSlideInT::create(1, myScene));
```
