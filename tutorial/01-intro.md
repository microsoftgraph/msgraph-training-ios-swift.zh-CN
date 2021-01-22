<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3542a-101">本教程指导你如何使用 Swift 生成 iOS 应用，该应用使用 Microsoft Graph API 检索用户的日历信息。</span><span class="sxs-lookup"><span data-stu-id="3542a-101">This tutorial teaches you how to build an iOS app with Swift that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="3542a-102">如果你更喜欢仅下载已完成的教程，可以下载或克隆 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-ios-swift)。</span><span class="sxs-lookup"><span data-stu-id="3542a-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-ios-swift).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3542a-103">先决条件</span><span class="sxs-lookup"><span data-stu-id="3542a-103">Prerequisites</span></span>

<span data-ttu-id="3542a-104">在开始本教程之前，应在开发计算机上安装以下内容。</span><span class="sxs-lookup"><span data-stu-id="3542a-104">Before you start this tutorial, you should have the following installed on your development machine.</span></span>

- [<span data-ttu-id="3542a-105">Xcode</span><span class="sxs-lookup"><span data-stu-id="3542a-105">Xcode</span></span>](https://developer.apple.com/xcode/)
- [<span data-ttu-id="3542a-106">CocoaPods</span><span class="sxs-lookup"><span data-stu-id="3542a-106">CocoaPods</span></span>](https://cocoapods.org)

<span data-ttu-id="3542a-107">你还应该拥有具有邮箱的个人 Microsoft 帐户Outlook.com或 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="3542a-107">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="3542a-108">如果你没有 Microsoft 帐户，则有几个选项可以获取免费帐户：</span><span class="sxs-lookup"><span data-stu-id="3542a-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="3542a-109">你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="3542a-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="3542a-110">你可以 [注册 Office 365 开发人员计划](https://developer.microsoft.com/office/dev-program) ，获取免费的 Office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="3542a-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="3542a-111">本教程是使用 Xcode 版本 12.3 和 CocoaPods 版本 1.10.1 编写的。本指南中的步骤可能与其他版本一样，但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="3542a-111">This tutorial was written using Xcode version 12.3 and CocoaPods version 1.10.1 The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="3542a-112">反馈</span><span class="sxs-lookup"><span data-stu-id="3542a-112">Feedback</span></span>

<span data-ttu-id="3542a-113">请在 GitHub 存储库中提供有关本教程 [的任何反馈](https://github.com/microsoftgraph/msgraph-training-ios-swift)。</span><span class="sxs-lookup"><span data-stu-id="3542a-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-ios-swift).</span></span>
