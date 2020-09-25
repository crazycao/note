# iOS 架构漫谈

## 为何要在意架构的选择呢？

因为如果你不在意的话，难保一天，你就需要去调试一个巨大无比又有着各种问题的类，然后你会发现在这个类里面，你完全就找不到也修复不了任何 bug。一般来说，把这么大的一个类作为整体放在脑子里记着是一件非常困难的事情，你总是难免会忘掉一些比较重要的细节。如果你发现在你的应用里面已经开始出现这种状况了，那你很可能遇到过下面这类问题：

- 这个类是一个 UIViewController 的子类。
- 你的数据直接保存在了 UIViewController 里面。
- 你的 UIViews 好像什么都没做。
- 你的 Model 只是一个纯粹的数据结构
- 你的单元测试什么都没有覆盖到

其实即便你遵循了 Apple 的设计规范，实现了 [Apple 的 MVC 框架](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html)，也还是一样会遇到上面这些问题；所以也没什么好失落的。Apple 的 MVC 框架有它自身的缺陷。

## 怎样才算好框架？

好的框架应该具有的特征：

- 用严格定义的角色，平衡的将职责 **划分** 给不同的实体。
- **可测性** 通常取决于上面说的第一点（不用太担心，如果架构合适的话，做到这点并不难）。
- **易用** 并且维护成本低。

### 划分

当我们试图去理解事物的工作原理的时候，划分可以减轻我们的脑部压力。如果你觉得开发的越多，大脑就越能适应去处理复杂的工作，确实是这样。但是大脑的这种能力不是线性提高的，而且很快就会达到一个瓶颈。所以要处理复杂的事情，最好的办法还是在遵循 单一责任原则 的条件下，将它的职责划分到多个实体中去。

### 可测性

对于那些对单元测试心存感激的人来说，应该不会有这方面的疑问：单元测试帮助他们测试出了新功能里面的错误，或者是帮他们找出了重构的一个复杂类里面的 bug。这意味着这些单元测试帮助这些开发者们在程序运行之前就发现了问题，这些问题如果被忽视的话很可能会提交到用户的设备上去；而修复这些问题，又至少需要一周左右的时间（AppStore 审核）。

### 易用

这块没什么好说的，直说一点：最好的代码是那些从未被写出来的代码。代码写的越少，问题就越少；所以开发者想少写点代码并不一定就是因为他懒。还有，当你想用一个比较 “聪明” 的方法的时候，全完不要忽略了它的维护成本。

## MV(X)系列

MV(X)包括 MVC、MVP、MVVM，都是把所有的实体归类到了下面三种分类中的一种：

- **Models（模型）**：数据层，或者负责处理数据的 数据接口层。比如 Person 和 PersonDataProvider 类
- **Views（视图）**：展示层(GUI)。对于 iOS 来说所有以 UI 开头的类基本都属于这层。
- **Controller/Presenter/ViewModel（控制器/展示器/视图模型）**：它是 Model 和 View 之间的胶水或者说是中间人。一般来说，当用户对 View 有操作时它负责去修改相应 Model；当 Model 的值发生变化时它负责去更新对应 View。

将实体进行分类之后我们可以：

- 更好的理解
- 重用（主要是 View 和 Model）
- 对它们独立的进行测试

### 传统的 MVC

![MVC](https://dn-coding-net-production-pp.codehub.cn/93c3aeab-92c0-4cc0-8344-969b668fe76b.png)

View 是无状态的，在 Model 变化的时候它只是简单的被 Controller 重绘；就像网页一样，点击了一个新的链接，整个网页就重新加载。

由于 MVC 的三种实体被紧密耦合着，每一种实体都和其他两种有着联系，这种紧耦合戏剧性的减少了它们被重用的可能，这恐怕不是你想要在自己的应用里面看到的。

显然，传统的 MVC 已经不适合当下的 iOS 开发了。

### Apple 的 MVC

![Apple MVC](https://dn-coding-net-production-pp.codehub.cn/8d779f6a-265b-43c3-90be-dc9997b9963d.png)

View 和 Model 之间是相互独立的，它们只通过 Controller 来相互联系。有点恼人的是 Controller 是重用性最差的，因为我们一般不会把冗杂的业务逻辑放在 Model 里面，那就只能放在 Controller 里了。

理想状况会如上图一般清晰，而现实状况却是这样：

![Real Apple MVC](https://dn-coding-net-production-pp.codehub.cn/ad9bafec-2e16-4725-abe5-4e7fde2e0877.png)

Cocoa MVC 鼓励你去写重控制器是因为 View 的整个生命周期都需要它去管理，Controller 和 View 很难做到相互独立。

虽然你可以把控制器里的一些业务逻辑和数据转换的工作交给 Model，但是你再想把负担往 View 里面分摊的时候就没办法了；因为 View 的主要职责就只是将用户的操作行为交给 Controller 去处理而已。

于是 ViewController 最终就变成了所有东西的代理和数据源，甚至还负责网络请求的发起和取消，还有……于是有人把 MVC 叫做重控制器模式，于是iOS 开发者们开始热议关于“ViewController 瘦身”的话题了。

Cocoa MVC 的问题在单元测试中更加凸显。Controller 测试起来很困难，因为它和 View 耦合的太厉害，要测试它的话就需要频繁的去 mock View 和 View 的生命周期；而且按照这种架构去写控制器代码的话，业务逻辑的代码也会因为视图布局代码的原因而变得很散乱。

> 按照文章开头的标准对 Cocoa MVC 进行评价：
> 
- 划分：View 和 Model 确实是实现了分离，但是 View 和 Controller 耦合的太厉害
- 可测性：因为划分的不够清楚，所以能测的基本就只有 Model 而已
- 易用：相较于其他模式，它的代码量最少。而且基本上每个人都很熟悉它，即便是没太多经验的开发者也能维护。

综上所述，如果你最看重的是开发速度，那么 Cocoa MVC 就是你最好的选择。

在这种情况下你可以选择 Cocoa MVC：你并不想在架构上花费太多的时间，而且你觉得对于你的小项目来说，花费更高的维护成本只是浪费而已。

### MVP - 保证了职责划分的（promises delivered） Cocoa MVC

![MVP](https://dn-coding-net-production-pp.codehub.cn/d8ad72b3-f150-4988-af6f-0db785c40793.png)

在 MVC 里面 View 和 Controller 是耦合紧密的，但是对于 MVP 里面的 Presenter 来讲，它完全不关注 ViewController 的生命周期，而且 View 也能被简单 mock 出来，所以在 Presenter 里面基本没什么布局相关的代码，它的职责只是通过数据和状态更新 View。

在 MVP 架构里面，UIViewController 的那些子类其实是属于 View 的，而不是 Presenter。这种区别提供了极好的可测性，但是这是用开发速度的代价换来的，因为你必须要手动的去创建数据和绑定事件。MVP 的代码量差不多是 MVC 架构的两倍。

> 按照文章开头的标准对 MVP 进行评价：
> 
- 划分：我们把大部分的职责都分配到了 Presenter 和 Model 里面，而 View 基本上不需要做什么。
- 可测性：简直棒，我们可以通过 View 来测试大部分的业务逻辑。
- 易用：虽然代码量很多，但是 MVP 的思路还是蛮清晰的。

总上所述，MVP 架构在 iOS 中意味着极好的可测性和巨大的代码量。

### MVP + Data Binding - 添加了数据绑定的 MVP

![MVP+](https://dn-coding-net-production-pp.codehub.cn/41e82632-02fc-44f9-a0eb-ef32aae5818a.png)

这个版本的 MVP 包括了 View 和 Model 的直接绑定，与此同时 Presenter（Supervising Controller）仍然继续处理 View 上的用户操作，控制 View 的显示变化。

但是我们之前讲过，模糊的职责划分是不好的事情，比如 View 和 Model 的紧耦合。这个道理在 Cocoa 桌面应用开发上面也是一样的。

> 作者似乎特别排斥这种架构，评论没写两句，图也画得很随意。


### MVVM - MV(X) 系列架构里面最新兴的，也是最出色的

![MVVM](https://dn-coding-net-production-pp.codehub.cn/1b8ff549-4fa4-489a-adf3-e8ba52e6bb96.png)

MVVM 架构把 ViewController 看做 View。View 和 Model 之间没有紧耦合。另外，它还像 Supervising 版的 MVP 那样做了数据绑定，不过这次不是绑定 View 和 Model，而是绑定 View 和 ViewModel。

ViewModel 能主动调用对 Model 做更改，也能在 Model 更新的时候对自身进行调整，然后通过 View 和 ViewModel 之间的绑定，对 View 也进行对应的更新。

现在提到「MVVM」你应该就会想到 ReactiveCocoa，反过来也是一样。虽然我们可以通过简单的绑定（KVO 和 通知）来实现 MVVM 模式，但是 ReactiveCocoa（或者同类型的框架，RxSwift、PromiseKit等）会让你更大限度的去理解 MVVM。

响应式编程框架也有一点不好的地方。用响应式编程用得不好的话，很容易会把事情搞得一团糟。或者这么说，如果有什么地方出错了，你需要花费更多的时间去调试。看着下面这张调用堆栈图感受一下：

![RAC](https://dn-coding-net-production-pp.codehub.cn/c8b080f0-d245-4628-a909-62d2deccf16c.png)

> 按照文章开头的标准对 MVVM 进行评价：
> 
- 划分：MVVM 框架里面的 View 比 MVP 里面负责的事情要更多一些。因为前者是通过 ViewModel 的数据绑定来更新自身状态的，而后者只是把所有的事件统统交给 Presenter 去处理就完了，自己本身并不负责更新。
- 可测性：因为 ViewModel 对 View 是一无所知的，这样我们对它的测试就变得很简单。View 应该也是能够被测试的，但是可能因为它对 UIKit 的依赖，你会直接略过它。
- 易用：在实际的应用当中 MVVM 的代码量 MVP 会更简洁一些。因为在 MVP 下你必须要把 View 的所有事件都交给 Presenter 去处理，而且需要手动的去更新 View 的状态；而在 MVVM 下，你只需要用绑定就可以解决。

## VIPER 更细粒度的新框架

![VIPER](https://dn-coding-net-production-pp.codehub.cn/aaa8d85f-b66a-4bb2-a8f5-13fae704d3b1.png)

到目前为止，我们都只是把职责划分成三层。现在 VIPER 从另一个角度对职责进行了划分，这次划分了 五层。

- **Interactor（交互器）**：包括数据（Entities）或者网络相关的业务逻辑。比如创建新的 entities 或者从服务器上获取数据；要实现这些功能，你可能会用到一些服务和管理（Services and Managers）——这些可能会被误以为成是外部依赖东西，但是它们就是 VIPER 的 Interactor 模块。
- **Presenter（展示器）**：包括 UI（but UIKit independent）相关的业务逻辑，可以调用 Interactor 中的方法。
- **Entities（实体）**：纯粹的数据对象。不包括数据访问层，因为这是 Interactor 的职责。
- **Router（路由）**：负责 VIPER 模块之间的转场。

实际上 VIPER 模块可以只是一个页面（screen），也可以是你应用里整个的用户使用流程（the whole user story）- 比如说「验证」这个功能，它可以只是一个页面，也可以是连续相关的一组页面。你的每个「乐高积木」想要有多大，都是你自己来决定的。

VIPER 是第一个把导航的职责单独划分出来的架构模式，负责导航的就是 Router 层。

> 按照文章开头的标准对 MVVM 进行评价：
> 
- 划分 - 毫无疑问的，VIPER 在职责划分方面是做的最好的。
- 可测性 - 理所当然的，职责划分的越好，测试起来就越容易
- 易用 - 最后，你可能已经猜到了，上面两点好处都是用维护性的代价换来的。一个小小的任务，可能就需要你为各种类写大量的接口。

## 结论

介绍完了以上几种架构，你应该已经意识到了，在选择架构模式这件问题上面，不存在什么 银色子弹，你需要做的就是具体情况具体分析，权衡利弊而已。

因此在同一个应用里面，即便有几种混合的架构模式也是很正常的一件事情。比如：开始的时候，你用的是 MVC 架构，后来你意识到有一个特殊的页面用 MVC 做的的话维护起来会相当的麻烦；这个时候你可以只针对这一个页面用 MVVM 模式去开发，对于之前那些用 MVC 就能正常工作的页面，你完全没有必要去重构它们，因为两种架构是完全可以和睦共存的。