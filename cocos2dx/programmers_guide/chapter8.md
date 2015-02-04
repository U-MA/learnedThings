# Chapter 8

## What is the EventDispatcher mechanism?

EventDispatchはユーザーイベントに反応するための仕組み  
  
基本  
* Event listenerはコードを処理するイベントをカプセル化する  
* Event dispatcherはユーザーイベントのlistenerに通知する  
* Event objectはイベントについての情報を保持する  

## 5 types of event listeners

* EventListenerTouch - タッチイベントに反応する
* EventListenerKeyboard - キーボードイベントに反応する
* EventListenerAcceleration - 加速度イベントに反応する
* EventListenMouse - マウスイベントに反応する
* EventListenerCustom - カスタムイベントに反応する

## FixedPriority vs SceneGraphPriority

EventDipatcherはどのlistenerが最初にイベントを受け取るかを決めるのにpriorityを使用する  

FixedPriorityは整数値  
低いpriority値を持つイベントlistenerは高いpriority値を持つイベントlistenerより前にイベントを処理する  

SceneGraphPriorityはNodeへのポインタ  
低いz-orderを持つNodeのイベントlistenerより前に高いz-order値を持つNodeのイベントを処理する  

## Touch Events

```C++
// Create a "one by one" touch event listener
// (processes one touch at a time)
auto listener1 = EventListenerRouchOneByOne::create();

// trigger when you push down
listener1->onTouchBegan = [](Touch* touch, Event* event) {
  // your code
  return true; // if you are consuming it
};

// trigger when moving touch
listener1->onTouchMoved = [](Touch* touch, Event* event) {
  // your code
};

// trigger when you let up
listener1->onTouchEnded = [=](Touch* touch, Event* event) {
  // your code
};

// Add listener
_eventDispacher->addEventListenerWithSceneGraphPriority(listener1, this);
```

onTouchBeganはタッチしたときに作動する  
onTouchMovedはタッチしたままオブジェクトを動かすときに作動する  
onTouchEndedはタッチを離したときに作動する  

## Swallowing Events

```C++
// When "swallow touched" is true, then returning 'true' from the
// onTouchBegan method will "swallow" the touch event, preventing
// other listener from using it
listener1->setSwallowTouches(true);

// you should also return true in onTouchBegan()

listener1->onTouchBegan = [](Touch* touch, Event* event) {
  // your code
};
```

## Creating a keyboard event

```C++
// creating a keyboard event listener
auto listener = EventListenerKeyboard::create();
listener->onKeyPressed = CC_CALLBACK_2(KeyboardTest::onKeyPressed, this);
listener->onKeyReleased = CC_CALLBACK_2(KeyboardTest::onKeyReleased, this);

_eventDispatcher->addEventListenerWithSceneGraphPriority(listener, this);

// Implementation of the keyboard event callback function prototype
void KeyboardTest::onKeyPressed(EventKeyboard::KeyCode keyCode, Event* event)
{
  log("Key with keycode %d pressed", keyCode);
}

void KeyboardTest::onKeyReleased(EventKeyboard::KeyCode keyCode, Event* event)
{
  log("Key with keycode %d released", keyCode);
}
```

## Creating an accelerometer event

accelerometerを使う前には, デバイスのaccelerometerをenableにする必要がある  

```C++
Device::setAccelerometerEnabled(true);
```

```C++
// creating an accelerometer event
auto listener = EventListenerAcceleration::create(CC_CALLBACK2(
  AccelerometerTest::onAcceleration, this));

_eventDispacher->addEventListenerWithSceneGraphPriority(listener, this);

// Implementation of the accelerometer callback function prototype
void AccelerometerTest::onAcceleration(Acceleration* acc, Event* event)
{
  // Processing logic here
}
```

## Creating a mouse event

```C++
_mouseListener = EventListenerMouse::create();
_mouseListener->onMouseMove = CC_CALLBACK_1(MouseTest::onMouseMove, this);
_mouseListener->onMouseUp   = CC_CALLBACK_1(MouseTest::onMouseUp,   this);
_mouseListener->onMouseDown = CC_CALLBACK_1(MouseTest::onMouseDown, this);
_mouseListener->onMouseScroll = CC_CALLBACK_1(MouseTest::onMouseScroll, this);

_eventDispatcher->addEventListenerWithSceneGraphPriority(_mouseListener, this);

void MouseTest::onMouseDown(Event* event)
{
  EventMouse* e = (EventMouse*)event;
  string str = "Mouse Down detected, Key: ";
  str += tostr(e->getMouseButton());
  // ...
}

void MouseTest::onMouseUp(Event* event)
{
  EventMouse* e = (EventMouse*)event;
  string str = "Mouse Up detected, Key: ";
  str += tostr(e->getMouseButton());
}

void MouseTest::onMouseMove(Event* event)
{
  EventMouse* e = (EventMouse*)event;
  string str = "MousePosition X:";
  str = str + tostr(e->getCursorX()) + " Y:" + tostr(e->getCursorY());
  // ...
}

void MouseTest::onMouseScroll(Event* event)
{
  EventMouse* e = (EventMouse*)event;
  string str = "Mouse Scroll detected, X:";
  str = str + tostr(e->getScrollX()) + " Y:" + tostr(e->getScrollY());
  // ...
}
```

## Registering event with the dispacher

```C++
// Add listener
_eventDispatcher->addEventListenerWithSceneGraphPriority(listener1, sprite1);
```

タッチイベントはオブジェクトごとに登録できる  
複数のオブジェクトに同一のlistenerを使う必要があれば, clone()を使うべきである  
```C++
// Add listener
_eventDispatcher->addEventListenerWithSceneGraphPriority(listener1, sprite1);

// Add the same listener to multiple objects
_eventDispatcher->addEventListenerWithSceneGraphPriority(listener1->clone(), sprite2);

_eventDispatcher->addEventListenerWithSceneGraphPriority(listener1->clone(), sprite3);
```

## Removing events from the dispacther

```C++
_eventDispatcher->removeEventListener(listener);
```
