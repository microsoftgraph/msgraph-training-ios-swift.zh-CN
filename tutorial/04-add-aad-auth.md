<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="08ccc-101">在此练习中，你将扩展上一练习中的应用程序，以支持使用 Azure AD 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="08ccc-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="08ccc-102">这是获取调用 Microsoft Graph 所需的 OAuth 访问令牌所必需的。</span><span class="sxs-lookup"><span data-stu-id="08ccc-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="08ccc-103">为此，您需要将 Microsoft 身份验证库 [ (MSAL) iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) 集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="08ccc-103">To do this, you will integrate the [Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) into the application.</span></span>

1. <span data-ttu-id="08ccc-104">在 **GraphTu一\*\*\*\*l** 项目中新建一个名为 **AuthSettings.plist 的属性列表文件**。</span><span class="sxs-lookup"><span data-stu-id="08ccc-104">Create a new **Property List** file in the **GraphTutorial** project named **AuthSettings.plist**.</span></span>
1. <span data-ttu-id="08ccc-105">将以下项添加到根字典 **中的文件** 。</span><span class="sxs-lookup"><span data-stu-id="08ccc-105">Add the following items to the file in the **Root** dictionary.</span></span>

    | <span data-ttu-id="08ccc-106">键</span><span class="sxs-lookup"><span data-stu-id="08ccc-106">Key</span></span> | <span data-ttu-id="08ccc-107">类型</span><span class="sxs-lookup"><span data-stu-id="08ccc-107">Type</span></span> | <span data-ttu-id="08ccc-108">值</span><span class="sxs-lookup"><span data-stu-id="08ccc-108">Value</span></span> |
    |-----|------|-------|
    | `AppId` | <span data-ttu-id="08ccc-109">String</span><span class="sxs-lookup"><span data-stu-id="08ccc-109">String</span></span> | <span data-ttu-id="08ccc-110">Azure 门户中的应用程序 ID</span><span class="sxs-lookup"><span data-stu-id="08ccc-110">The application ID from the Azure portal</span></span> |
    | `GraphScopes` | <span data-ttu-id="08ccc-111">数组</span><span class="sxs-lookup"><span data-stu-id="08ccc-111">Array</span></span> | <span data-ttu-id="08ccc-112">三个字符串值：、 `User.Read` `MailboxSettings.Read` 和 `Calendars.ReadWrite`</span><span class="sxs-lookup"><span data-stu-id="08ccc-112">Three String values: `User.Read`, `MailboxSettings.Read`, and `Calendars.ReadWrite`</span></span> |

    ![Xcode 中的 AuthSettings.plist 文件的屏幕截图](images/auth-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="08ccc-114">如果你使用的是源代码管理（如 git），那么现在应该从源代码管理中排除 **AuthSettings.plist** 文件，以避免意外泄露应用 ID。</span><span class="sxs-lookup"><span data-stu-id="08ccc-114">If you're using source control such as git, now would be a good time to exclude the **AuthSettings.plist** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="08ccc-115">实现登录</span><span class="sxs-lookup"><span data-stu-id="08ccc-115">Implement sign-in</span></span>

<span data-ttu-id="08ccc-116">在此部分中，你将为 MSAL 配置项目，创建身份验证管理器类，并更新应用以登录和注销。</span><span class="sxs-lookup"><span data-stu-id="08ccc-116">In this section you will configure the project for MSAL, create an authentication manager class, and update the app to sign in and sign out.</span></span>

### <a name="configure-project-for-msal"></a><span data-ttu-id="08ccc-117">为 MSAL 配置项目</span><span class="sxs-lookup"><span data-stu-id="08ccc-117">Configure project for MSAL</span></span>

1. <span data-ttu-id="08ccc-118">向项目功能添加新的钥匙链组。</span><span class="sxs-lookup"><span data-stu-id="08ccc-118">Add a new keychain group to your project's capabilities.</span></span>
    1. <span data-ttu-id="08ccc-119">选择 **GraphTu一l** 项目，然后签署 **&功能**。</span><span class="sxs-lookup"><span data-stu-id="08ccc-119">Select the **GraphTutorial** project, then **Signing & Capabilities**.</span></span>
    1. <span data-ttu-id="08ccc-120">选择 **+ 功能**，然后双击 **钥匙链共享**。</span><span class="sxs-lookup"><span data-stu-id="08ccc-120">Select **+ Capability**, then double-click **Keychain Sharing**.</span></span>
    1. <span data-ttu-id="08ccc-121">添加带值的钥匙链组 `com.microsoft.adalcache` 。</span><span class="sxs-lookup"><span data-stu-id="08ccc-121">Add a keychain group with the value `com.microsoft.adalcache`.</span></span>

1. <span data-ttu-id="08ccc-122">控件单击 **Info.plist，** 然后选择 **"打开为**"，然后选择 **"源代码"。**</span><span class="sxs-lookup"><span data-stu-id="08ccc-122">Control click **Info.plist** and select **Open As**, then **Source Code**.</span></span>
1. <span data-ttu-id="08ccc-123">在元素中添加 `<dict>` 以下内容。</span><span class="sxs-lookup"><span data-stu-id="08ccc-123">Add the following inside the `<dict>` element.</span></span>

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        </array>
      </dict>
    </array>
    <key>LSApplicationQueriesSchemes</key>
    <array>
        <string>msauthv2</string>
        <string>msauthv3</string>
    </array>
    ```

1. <span data-ttu-id="08ccc-124">打开 **AppDelegate.swift，** 在文件顶部添加以下导入语句。</span><span class="sxs-lookup"><span data-stu-id="08ccc-124">Open **AppDelegate.swift** and add the following import statement at the top of the file.</span></span>

    ```Swift
    import MSAL
    ```

1. <span data-ttu-id="08ccc-125">将以下函数添加到 `AppDelegate` 类。</span><span class="sxs-lookup"><span data-stu-id="08ccc-125">Add the following function to the `AppDelegate` class.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/AppDelegate.swift" id="HandleMsalResponseSnippet":::

### <a name="create-authentication-manager"></a><span data-ttu-id="08ccc-126">创建身份验证管理器</span><span class="sxs-lookup"><span data-stu-id="08ccc-126">Create authentication manager</span></span>

1. <span data-ttu-id="08ccc-127">在名为 **AuthenticationManager.swift\*\*\*\*的 GraphTu一l** 项目中创建新的 **Swift** 文件。</span><span class="sxs-lookup"><span data-stu-id="08ccc-127">Create a new **Swift File** in the **GraphTutorial** project named **AuthenticationManager.swift**.</span></span> <span data-ttu-id="08ccc-128">将以下代码添加到文件中。</span><span class="sxs-lookup"><span data-stu-id="08ccc-128">Add the following code to the file.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.swift" id="AuthManagerSnippet":::

### <a name="add-sign-in-and-sign-out"></a><span data-ttu-id="08ccc-129">添加登录和注销</span><span class="sxs-lookup"><span data-stu-id="08ccc-129">Add sign-in and sign-out</span></span>

1. <span data-ttu-id="08ccc-130">打开 **SignInViewController.swift，** 并将其内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="08ccc-130">Open **SignInViewController.swift** and replace its contents with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/SignInViewController.swift" id="SignInViewSnippet":::

1. <span data-ttu-id="08ccc-131">打开 **WelcomeViewController.swift，** 将现有 `signOut` 函数替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="08ccc-131">Open **WelcomeViewController.swift** and replace the existing `signOut` function with the following.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.swift" id="SignOutSnippet":::

1. <span data-ttu-id="08ccc-132">保存更改，并重启模拟器中的应用程序。</span><span class="sxs-lookup"><span data-stu-id="08ccc-132">Save your changes and restart the application in Simulator.</span></span>

<span data-ttu-id="08ccc-133">如果登录应用，应该会看到访问令牌显示在 Xcode 的输出窗口中。</span><span class="sxs-lookup"><span data-stu-id="08ccc-133">If you sign in to the app, you should see an access token displayed in the output window in Xcode.</span></span>

![Xcode 中输出窗口的屏幕截图，显示访问令牌](images/access-token-output.png)

## <a name="get-user-details"></a><span data-ttu-id="08ccc-135">获取用户详细信息</span><span class="sxs-lookup"><span data-stu-id="08ccc-135">Get user details</span></span>

<span data-ttu-id="08ccc-136">在此部分中，你将创建一个帮助程序类来保存对 Microsoft Graph 的所有调用，并更新为使用此新类获取 `WelcomeViewController` 登录用户。</span><span class="sxs-lookup"><span data-stu-id="08ccc-136">In this section you will create a helper class to hold all of the calls to Microsoft Graph and update the `WelcomeViewController` to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="08ccc-137">在 **GraphTu一\*\*\*\*l** 项目中新建一个名为 **GraphManager.swift** 的 Swift 文件。</span><span class="sxs-lookup"><span data-stu-id="08ccc-137">Create a new **Swift File** in the **GraphTutorial** project named **GraphManager.swift**.</span></span> <span data-ttu-id="08ccc-138">将以下代码添加到文件中。</span><span class="sxs-lookup"><span data-stu-id="08ccc-138">Add the following code to the file.</span></span>

    ```Swift
    import Foundation
    import MSGraphClientSDK
    import MSGraphClientModels

    class GraphManager {

        // Implement singleton pattern
        static let instance = GraphManager()

        private let client: MSHTTPClient?

        public var userTimeZone: String

        private init() {
            client = MSClientFactory.createHTTPClient(with: AuthenticationManager.instance)
            userTimeZone = "UTC"
        }

        public func getMe(completion: @escaping(MSGraphUser?, Error?) -> Void) {
            // GET /me
            let select = "$select=displayName,mail,mailboxSettings,userPrincipalName"
            let meRequest = NSMutableURLRequest(url: URL(string: "\(MSGraphBaseURL)/me?\(select)")!)
            let meDataTask = MSURLSessionDataTask(request: meRequest, client: self.client, completion: {
                (data: Data?, response: URLResponse?, graphError: Error?) in
                guard let meData = data, graphError == nil else {
                    completion(nil, graphError)
                    return
                }

                do {
                    // Deserialize response as a user
                    let user = try MSGraphUser(data: meData)
                    completion(user, nil)
                } catch {
                    completion(nil, error)
                }
            })

            // Execute the request
            meDataTask?.execute()
        }
    }
    ```

1. <span data-ttu-id="08ccc-139">打开 **WelcomeViewController.swift，** 在文件顶部 `import` 添加以下语句。</span><span class="sxs-lookup"><span data-stu-id="08ccc-139">Open **WelcomeViewController.swift** and add the following `import` statement at the top of the file.</span></span>

    ```Swift
    import MSGraphClientModels
    ```

1. <span data-ttu-id="08ccc-140">将以下属性添加到 `WelcomeViewController` 类。</span><span class="sxs-lookup"><span data-stu-id="08ccc-140">Add the following property to the `WelcomeViewController` class.</span></span>

    ```Swift
    private let spinner = SpinnerViewController()
    ```

1. <span data-ttu-id="08ccc-141">将现有 `viewDidLoad` 代码替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="08ccc-141">Replace the existing `viewDidLoad` with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.swift" id="ViewDidLoadSnippet":::

<span data-ttu-id="08ccc-142">如果保存更改并立即重新启动应用，则登录 UI 后，会使用用户的 显示名称 和电子邮件地址进行更新。</span><span class="sxs-lookup"><span data-stu-id="08ccc-142">If you save your changes and restart the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
