# Chapter 2

## Main Components

* Scene  
* Transition  
* Sprite  
* Menu  
* Sprite3D  
* Audio  
* and so on.  
  
Cocos2d-xの核となるのは, Scene, Node, Sprite, Menu and Action.  

## Director

Cocos2d-xはDirectorという概念を使用している  
Directorはoperationの流れを制御し, すべきことを必要なrecipientに伝える  
Directorの仕事の一つは, Sceneの置き換えや遷移を制御すること  
Directorは, コードのどこからでも呼ぶことが出来る, 共有されるシングルトンオブジェクト  

## Scene

メインメニューとかの一つの画面
SceneはRendererによって描かれる  
Rendererはスクリーン内のスプライトや他のオブジェクトを描くことに責任がある  
このことについて理解するためには, Scene Graphについて知っておく必要がある  

## Scene Graph

Scene Graphは, グラフィカルなSceneを整理するデータ構造    
Scene Graphは木構造の中のNodeオブジェクトから成っている  
Cocos2d-xでは, 木を探索するのにin-oder walkアルゴリズムを採用している  
in-oder walkアルゴリズム = 左からの深さ優先探索  
木の右側は最後に描画されるので, Scene Graph上では最初に表示される  
木の左側の要素は負のz-orderを持ち, 木の右側の要素は正のz-orderを持つ  
  
Cocos2d-xでは, addChild() APIでScene Graphを構築することが出来る  
```C++
// Adds a child with the z-order of -2, that means
// it goes to the "left" side of the tree (because it is negative)
scene->addChild(title_node, -2);

// When you don't specify the z-order, it will use 0
scene->addChild(label_node);

// Adds a child with the z-order of 1, that means
// it goes to the "right" side of the tree (because it is positive)
scene->addChild(sprite_node, 1);
```

