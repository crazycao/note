# Requesting App Store Reviews 请求 App Store 评论

原文地址：[https://developer.apple.com/documentation/storekit/skstorereviewcontroller/requesting_app_store_reviews?language=objc](https://developer.apple.com/documentation/storekit/skstorereviewcontroller/requesting_app_store_reviews?language=objc)

Use best practices for prompting users to leave a review for your app in the App Store.

采用最佳的实践提示用户在 App Store 中为你的 app 留下评论。

[【示例代码下载链接】](https://docs-assets.developer.apple.com/published/02b4b6fb44/RequestingAppStoreReviews.zip?language=objc)

## Overview 概览

Presenting the user with a request for an App Store review using [SKStoreReviewController](https://developer.apple.com/documentation/storekit/skstorereviewcontroller?language=objc) is a good way to get feedback on your app. However, you should be aware that the prompt will only be displayed to a user a maximum of three times within a 365-day period. Choosing when and where to display this prompt is critical to your success using this API.

使用 [](https://developer.apple.com/documentation/storekit/skstorereviewcontroller?language=objc) 向用户展示 App Store 评论请求是一种获得对你的 app 的反馈的良好方式。但是，您应该知道，提示在365天内最多只能向用户显示三次。选择何时何地使用这个API展示这个提示对你的成功至关重要。

This sample project consists of a simulated three-step process. The user first taps the “Start Process” button, then taps “Continue Process” twice to finally be presented with a “Process Completed” view controller scene. The request for review is only shown to the user from this scene.

这个示例项目由三个模拟步骤组成。用户首先点击 “Start Process” 按钮，然后点击 “Continue Process” 两次，最终会在屏幕上展示一个 “Process Completed” 视图控制器。评论的请求只会在这个场景下展示给用户。

In addition, the following conditions need to be met before the prompt is displayed:

另外，下面的条件也必须满足才能展示这个提示：

- The review prompt must not have been shown for a version of the app bundle that matches the current bundle version. This ensures that the user is not asked to review the same version of your app multiple times.

- 对于与当前 bundle 版本相同的 app bundle 版本，必须没有展示过评论提示。这确保了用户不会被多次请求评论同一个版本。

- The multistep process mentioned above must be successfully completed at least four times. This number is arbitrary and you should choose something that fits well with how many times the user is likely to complete a process in your app.

- 上述多个步骤必须被成功的完成至少四次。这个数字是任意的，你应该选择与用户可能在你的应用程序中完成一个过程的次数相匹配的数字。

- Finally, the user must pause on the “Process Completed” scene for a few seconds. This requirement limits the possibility of the prompt interrupting the user if they are about to move to a different task in your application straight away.

- 最后，用户必须停在 “Proceess Completed” 界面几秒钟。该要求限制了评论提示在用户要立即转移到 app 中的其他任务时中断用户的可能性。

The conditions above exist purely to delay the call to requestReview and so days, weeks, or potentially even months could elapse without a user being prompted to review your app. Techniques to delay the call are valuable as they will cause it to be shown to more experienced users that are getting real value from using your app.

上述条件的存在纯粹是为了延迟对 `requestReview` 的调用，可能几天、几周、甚至几个月过去了都没有提示用户评论你的 app。延迟调用的技巧是很有价值的，因为这会将它展示给更有经验的用户，这些用户正从使用你的 app 的过程中获得真正的价值。

## Present the Review Request 展示评论请求

Spend some time thinking about the best places within your own app to show a request for review, and what conditions are appropriate to delay it. Here are some best practices:

花一些时间来思考在你自己的 app 中展示评论请求的最好的地方，以及什么样的条件适合延迟它。这里是一些最佳实践：

- Try to make the request at a time that will not interrupt what the user is trying to achieve in your app. For example, at the end of a sequence of events that the user has successfully completed.

- 不要在用户正在你的 app 中试图达成时尝试发出请求，这会打断用户。例如，应该在一连串用户成功完成的事件之后。

- Avoid showing a request for a review immediately when a user launches your app, even if it is not the first time that it launches.

- 避免在用户启动你的 app 时立即展示评论请求，即使不是第一次启动。

Also remember that the user can disable requests for reviews from ever appearing on their device, so you should avoid referring to your app showing this prompt and never request a review using requestReview as the result of a user action.

也要记住用户可以从曾经展示过评论请求的设备上将其关闭，因此你应该避免提到你的 app 将展示这个提示，并且永远不要使用 `requestReview` 作为某个用户操作的结果。

```
// If the count has not yet been stored, this will return 0
// 如果 count 没有被存储过，它将返回 0
var count = UserDefaults.standard.integer(forKey: UserDefaultsKeys.processCompletedCountKey)
count += 1
UserDefaults.standard.set(count, forKey: UserDefaultsKeys.processCompletedCountKey)

print("Process completed \(count) time(s)")

// Get the current bundle version for the app
// 获取 app 的当前 bundle 版本
let infoDictionaryKey = kCFBundleVersionKey as String
guard let currentVersion = Bundle.main.object(forInfoDictionaryKey: infoDictionaryKey) as? String
    else { fatalError("Expected to find a bundle version in the info dictionary") }

let lastVersionPromptedForReview = UserDefaults.standard.string(forKey: UserDefaultsKeys.lastVersionPromptedForReviewKey)

// Has the process been completed several times and the user has not already been prompted for this version?
// 该过程已经被完成了若干次，并且用户在这个版本没有展示过提示？
if count >= 4 && currentVersion != lastVersionPromptedForReview {
    let twoSecondsFromNow = DispatchTime.now() + 2.0
    DispatchQueue.main.asyncAfter(deadline: twoSecondsFromNow) { [navigationController] in
        if navigationController?.topViewController is ProcessCompletedViewController {
            SKStoreReviewController.requestReview()
            UserDefaults.standard.set(currentVersion, forKey: UserDefaultsKeys.lastVersionPromptedForReviewKey)
        }
    }
}
```

In this code sample the usage data used to delay the review request is stored in NSUserDefaults. In your app there may be more appropriate on-device storage options.

在该代码示例中，用于延迟评审请求的使用数据存储在NSUserDefaults中。在你的 app 中也可能有更合适的基于设备的存储选项。

Further information on best practices for requesting reviews can be found in the ratings and reviews section of the Human Interface Guidelines.

有关请求评论的最佳实践的更多信息，请参阅《人机交互指南》的“评分和评审”部分。

## Manually Request a Review 手动请求评论

To enable a user to initiate a review as a result of an action in the UI use a deep link to the App Store page for your app with the query parameter `action=write-review` appended to the URL.

要使用户可以基于 UI 上的操作初始化评论，可以使用到你的 app 的 App Store 页的深度链接，并在 URL 后面加上请求参数 `action=write-review`。

```
@IBAction func requestReviewManually() {
    // Note: Replace the XXXXXXXXXX below with the App Store ID for your app
    //       You can find the App Store ID in your app's product URL
    // 注意：使用你的 app 的 App Store ID 替换下面的 XXXXXXXXXX
    //      你可以在你的 app 的产品链接中找到 App Store ID
    guard let writeReviewURL = URL(string: "https://apps.apple.com/app/idXXXXXXXXXX?action=write-review")
        else { fatalError("Expected a valid URL") }
    UIApplication.shared.open(writeReviewURL, options: [:], completionHandler: nil)
}
```

