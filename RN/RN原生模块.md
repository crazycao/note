# RN 原生模块

## 定义

在 React Native 中，一个“**原生模块**”就是一个实现了“**RCTBridgeModule**”协议的 Objective-C 类，其中 RCT 是 ReaCT 的缩写。

## 使用场景

- 有时候 App 需要访问平台 API，但 React Native 可能还没有相应的模块封装
- 有时候你需要复用 Objective-C、Swift 或 C++代码，而不是用 JavaScript 重新实现一遍
- 有时候你需要实现某些高性能、多线程的代码，譬如图片处理、数据库、或者各种高级扩展等等

## 基础实现

- 在原生模块（Object-C 类）的头文件中声明遵从 `RCTBridgeModule` 协议

```
// CalendarManager.h
#import <React/RCTBridgeModule.h>
#import <React/RCTLog.h>

@interface CalendarManager : NSObject <RCTBridgeModule>
@end
```

- 在类的实现中包含 `RCT_EXPORT_MODULE()` 宏

```
// CalendarManager.m
@implementation CalendarManager

RCT_EXPORT_MODULE();

@end
```

- 在类的实现中明确的声明要给 Javascript 导出的方法。声明通过 `RCT_EXPORT_METHOD()` 宏来实现：

```
RCT_EXPORT_METHOD(addEvent:(NSString *)name location:(NSString *)location)
{
  // 导出方法 addEvent:location:
  RCTLogInfo(@"Pretending to create an event %@ at %@", name, location);
}
```

- 从 Javascript 里可以这样调用这个方法

```
import { NativeModules } from 'react-native';
// 获取原生模块
var CalendarManager = NativeModules.CalendarManager;
// 原生模块的方法调用
CalendarManager.addEvent(
  'Birthday Party',
  '4 Privet Drive, Surrey'
);
```

> **注意**: 
> 
> 1. Javascript 方法名
> 
> 	导出到 Javascript 的方法名是 Objective-C 的方法名的第一部分。
> 
> 	React Native 还定义了一个 `RCT_REMAP_METHOD()` 宏，它可以指定 Javascript 方法名。当许多方法的第一部分相同的时候用它来避免在 Javascript 端的名字冲突。
> 
> 2. 桥接到 Javascript 的方法返回值类型必须是void。
> 3. React Native 的桥接操作是异步的，所以要返回结果给 Javascript，你必须通过回调或者触发事件来进行。

## 回调函数

原生模块通过回调函数来把返回值传回给 JavaScript。

```
RCT_EXPORT_METHOD(findEvents:(RCTResponseSenderBlock)callback)
{
  NSArray *events = @[@"a", @"b", @"v"];
  callback(@[[NSNull null], events]);
}
```

`RCTResponseSenderBlock` 只接受一个参数 —— 传递给 JavaScript 回调函数的参数数组。

参数数组中，第一个参数是一个错误对象（没有发生错误的时候为 null），而剩下的部分是函数的返回值。

在 Javascript 里可以这样使用带回调函数的方法：

```
CalendarManager.findEvents((error, events) => {
  if (error) {
    console.error(error);
  } else {
    this.setState({ events: events });
  }
});
```

## Promises

原生模块还可以使用 promise 来简化代码。

```
RCT_REMAP_METHOD(findEvents,
                 resolver:(RCTPromiseResolveBlock)resolve
                 rejecter:(RCTPromiseRejectBlock)reject)
{
  NSArray *events = ...
  if (events) {
    resolve(events);
  } else {
    reject(error);
  }
}
```

如果桥接原生方法的最后两个参数是 `RCTPromiseResolveBlock` 和 `RCTPromiseRejectBlock`，则对应的 JS 方法就会返回一个 Promise 对象。

此时，可以在一个声明了 `async` 的异步函数内使用 `await` 关键字来调用，并等待其结果返回。

```
async function updateEvents() {
  try {
    var events = await CalendarManager.findEvents();

    this.setState({ events });
  } catch (e) {
    console.error(e);
  }
}

updateEvents();
```

## 导出常量

原生模块可以导出一些常量，这些常量在 JavaScript 端随时都可以访问。用这种方法来传递一些静态数据，可以避免通过 bridge 进行一次来回交互。

```
- (NSDictionary *)constantsToExport
{
  return @{ @"firstDayOfTheWeek": @"Monday" };
}
```

Javascript 端可以随时同步地访问这个数据：

```
console.log(CalendarManager.firstDayOfTheWeek);
```

> **注意：**这个常量仅仅在初始化的时候导出了一次，所以即使你在运行期间改变constantToExport返回的值，也不会影响到 JavaScript 环境下所得到的结果。

## 给 Javascript 发送事件

- 原生模块（Objective-C 类）需要继承 `RCTEventEmitter`。

```
// CalendarManager.h
#import <React/RCTBridgeModule.h>
#import <React/RCTEventEmitter.h>

@interface CalendarManager : RCTEventEmitter <RCTBridgeModule>

@end
```

- 实现 `suppportEvents` 方法

```
- (NSArray<NSString *> *)supportedEvents
{
  return @[@"EventReminder"];
}
```

- 在需要发送事件时调用 `[self sendEventWithName:]`

```
- (void)calendarEventReminderReceived:(NSNotification *)notification
{
  NSString *eventName = notification.userInfo[@"name"];
  [self sendEventWithName:@"EventReminder" body:@{@"name": eventName}];
}
```

- 在 JS 中订阅这些事件：

```
import { NativeEventEmitter, NativeModules } from 'react-native';
const { CalendarManager } = NativeModules;

const calendarManagerEmitter = new NativeEventEmitter(CalendarManager);

const subscription = calendarManagerEmitter.addListener(
  'EventReminder',
  (reminder) => console.log(reminder.name)
);
...
// 别忘了取消订阅，通常在componentWillUnmount生命周期方法中实现。
subscription.remove();
```

