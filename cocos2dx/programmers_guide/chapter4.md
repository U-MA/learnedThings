# Chapter 4

ActionオブジェクトはNodeオブジェクトにpropertyの変更を行わせる  
Nodeを基底クラスに持つ任意のオブジェクトはActionオブジェクトを使用可能  

```C++
// Move sprite to position 50, 10 in 2 seconds.
auto moveTo = MoveTo::create(2, Vec2(50, 10));
mySprite1->runAction(moveTo);

// Move sprite 20 points to right in 2 seconds
auto moveBy = MoveBy::create(2, Vec2(20, 0));
mySprite2->runAction(moveBy);
```

## By and To, what is the difference?

ByはNodeの現在の状態に対して相対的である 
ToはNodeの現在の状態を考慮しない  

```C++
auto mySprite = Sprite::create("mysprite.png");
mysprite->setPosition(Vec2(200, 256));

// MoveBy - lets move the sprite by 500 on the x axis over 2 seconds
// MoveBy is relarive - since x = 200 + 200 move = x is now 400 after the move
auto moveBy = MoveBy::create(2, Vec2(200, mysprite->getPosisionY()));

// MoveTo - lets move the new sprite to 300 x 256 over 2 seconds
// MoveTo is absolute - The sprite gets moved to 300 x 256 regardles of
// where it is located now.
auto moveTo = MoveTo::create(2, Vec2(300, mysprite->getPosisionY()));

auto seq = Sequence::create(moveBy, delay, moveTo, nullptr);

mysprite->runAction(seq);
```

## Basic Actions and how to run them

### Move

```C++
auto mySprite = Sprite::create("mysprite.png");

// Move a sprite to a specific location over 2 seconds.
auto moveTo = MoveTo::create(2, Vec2(50, 0));

mysprite->runAction(moveTo);

// Move a sprite 50 pixels to the right, and 0 pixels to the top over 2 seconds.
auto moveBy = MoveBy::create(2, Vec2(50, 0));

mysprite->runAction(moveBy);
```

### Rotate

```C++
auto mySprite = Sprite::create("mysprite.png");

// Rotates a Node to the specific angle over 2 seconds
auto rotateTo = RotatedTo::create(2.0f, 40.0f);
mySprite->runAction(rotateTo);

// Rotates a Node clockwise by 40 degrees over 2 seconds
auto rotateBy = RotateBy::create(2.0f, 40.0f);
mySprite->runAction(rotateBy);
```

### Scale

```C++
auto mySprite = Sprite::create("mysprite.png");

// Scale uniformly by 3x over 2 seconds
auto scaleBy = ScaleBy::create(2.0f, 3.0f);
mySprite->runAction(scaleBy);

// Scale X by 5 and Y by 3x over 2 seconds
auto scaleBy = ScaleBy::create(2.0f, 3.0f, 3.0f);
mySprite->runAction(scaleBy);

// Scale to uniformly to 3x over 2 seconds
auto scaleTo = ScaleTo::create(2.0f, 3.0f);
mySprite->runAction(scaleTo);

// Scale X to 5 and Y to 3x over 2 seconds
auto scaleTo = ScaleTo::create(2.0f, 3.0f, 3.0f);
mySprite->runAction(scaleTo);
```

### Fade In/Out

```C++
auto mySprite = Sprite::create("mysprite.png");

// fades in the sprite in 1 second
auto fadeIn = FadeIn::create(1.0f);
mysprite->runAction(fadeIn);

// fades out the sprite in 2 seconds
auto fadeOut = FadeOut::create(2.0f);
mySprite->runAction(fadeOut);
```

### Tint

```C++
auto mySprite = Sprite::create("mysprite.png");

// Tints a node to the specified RGB values
auto tintTo = TintTo::create(2.0f, 120.0f, 232.0f, 254.0f);

// Tints a node BY the delta of the specified RGB values
auto tintBy = TintBy::create(2.0f, 120.0f, 232.0f, 254.0f);
mySprite->runAction(tintBy);
```

### Animate

```C++
auto mySprite = Sprite::create("mysprite.png");

// now lets animate the sprite we moved

Vector<SpriteFrame*> animFrames;
animFrames.reserve(12);
animFrames.pushBack(SpriteFrame::create("Blue_Front1.png", Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Front2.png", Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Front3.png", Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Left1.png",  Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Left2.png",  Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Left3.png",  Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Back1.png",  Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Back2.png",  Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Back3.png",  Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Right1.png", Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Right2.png", Rect(0, 0, 65, 81)));
animFrames.pushBack(SpriteFrame::create("Blue_Right3.png", Rect(0, 0, 65, 81)));

// create the animation out of the frames
Animation* animation = Animation::createWithSpriteFrames(animFrames, 0.1f);
Animate* animate = Animate::create(animation);

// run it and repeat it forever
mySprite->runAction(RepeatForever::create(animate));
```

### Easing

```C++
// create a sprite
auto mySprite = Sprite::create("mysprite.png");

// create a MoveBy Action to where we want the sprite to drop from.
auto move = MoveBy::create(2, Vec2(200, dirs->getVisibleSize().height - newSprite2->getContentSize().height));
auto move_back = move->reverse();

// create a BounceIn Ease Action
auto move_ease_in = EaseBounceIn::create(move->clone());

// create a delay that is run in between sequence events
auto delay = DelayTime::create(0.25f);

// create the sequence of actions, in the order we want to run them
auto seq1 = Sequence::create(move_ease_in, delay, move_ease_in_back, delay->clone(), nullptr);

// ru the sequence and repeat forever
mySprite->runAction(RepeatForever::create(seq1));
```

## Sequences and how to run them

Sequenceは順番に実行されるActionオブジェクトの流れである  
任意の数のActionオブジェクト, 関数, 他のSequenceが利用可能である  
Sequenceに関数を渡すにはCallFuncオブジェクトを使用する  

### An example sequence

```C++
auto mySprite = Sprite::create("mysprite.png");

// create a few actions.
auto jump = JumpBy::create(0.5, Vec2(0, 0), 100, 1);

auto rotate = RotateTo::create(2.0f, 10);

// create a few callbacks
auto callbackJump = CallFunc::create([]() {
  log("Jumped!");
});

auto callbackRotate = CallFunc::create([]() {
  log("Rotated!");
});

// create a sequence with actions and callbacks
auto seq = Sequence::create(jump, callbackJump, rotate, callbackRotate, nullptr);

// run it
mySprite->runAction(seq);
```

### Spawn

SpawnはすべてのActionが同時に実行されるのを除き, Sequenceに似ている  
任意の数のActionオブジェクトと他のSpawnオブジェクトが使用可能  

```C++
// create 2 actions and run a Spawn on a Sprite
auto mySprite = Sprite::create("mysprite.png");

auto moveBy = MoveBy::create(10, Vec2(400, 100));
auto fadeTo = FadeTo::create(2.0f, 120.0f):

// running the above Actions with Spawn
auto mySpawn = Spawn::createWithTwoActions(moveBy, fadeTo);
mySprite->runActions(mySpawn);
```

次のコードは上の動作と同様になる
```C++
// running the above Actions with consecutive runAction() statements.
mySprite->runAction(moveBy);
mySprite->runAction(fadeTo);
```

```C++
// create a Sprite
auto mySprite = Sprite::create("mysprite.png");

// create a few Actions
auto moveBy = MoveBy::create(10, Vec2(400, 100));
auto fadeTo = FadeTo::create(2.0f, 120.0f);
auto scaleBy = ScaleBy::create(2.0f, 3.0f);

// create a Spawn to use
auto mySpawn = Spawn::createWithTwoActions(scaleBy, fadeTo);

// tie everything together in a sequence
auto seq = Seequence::create(moveBy, mySpawn, moveBy, nullptr);

// run it
mySprite->runAction(seq);
```

### Reverse

```C++
//reverse a sequence, spawn or action
mySprite->runAction(mySpawn->reverse());
```

```C++
// create a Sprite
auto mySprite = Sprite::create("mysprite.png");
mySprite->setPosition(50, 56);

// create a few Actions
auto moveBy = MoveBy::create(2.0f, Vec2(500, 0));
auto scaleBy = ScaleBy::create(2.0f, 2.0f);
auto delay = DelayTime::create(2.0f);

// create a sequence
auto delaySequence = Sequence::create(delay, delay->clone(), delay->clone(),
                                      delay->clone(), nullptr);

auto sequence = Sequence::create(moveBy, delay, scaleBy, delaySequence, nullptr);

// run it
newSprite2->runAction(sequence);

// reverse it
newSprite2->runAction(sequence->reverse());
```
