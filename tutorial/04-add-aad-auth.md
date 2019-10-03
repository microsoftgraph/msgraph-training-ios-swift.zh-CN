<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1cce7-101">在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="1cce7-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="1cce7-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph，这是必需的。</span><span class="sxs-lookup"><span data-stu-id="1cce7-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="1cce7-103">为此，需要将[iOS 的 Microsoft 身份验证库（MSAL）](https://github.com/AzureAD/microsoft-authentication-library-for-objc)集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="1cce7-103">To do this, you will integrate the [Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) into the application.</span></span>

1. <span data-ttu-id="1cce7-104">在名为**AuthSettings**的**GraphTutorial**项目中创建一个新的**属性列表**文件。</span><span class="sxs-lookup"><span data-stu-id="1cce7-104">Create a new **Property List** file in the **GraphTutorial** project named **AuthSettings.plist**.</span></span>
1. <span data-ttu-id="1cce7-105">将以下项添加到**根**字典中的文件中。</span><span class="sxs-lookup"><span data-stu-id="1cce7-105">Add the following items to the file in the **Root** dictionary.</span></span>

    | <span data-ttu-id="1cce7-106">Key</span><span class="sxs-lookup"><span data-stu-id="1cce7-106">Key</span></span> | <span data-ttu-id="1cce7-107">类型</span><span class="sxs-lookup"><span data-stu-id="1cce7-107">Type</span></span> | <span data-ttu-id="1cce7-108">值</span><span class="sxs-lookup"><span data-stu-id="1cce7-108">Value</span></span> |
    |-----|------|-------|
    | `AppId` | <span data-ttu-id="1cce7-109">String</span><span class="sxs-lookup"><span data-stu-id="1cce7-109">String</span></span> | <span data-ttu-id="1cce7-110">Azure 门户中的应用程序 ID</span><span class="sxs-lookup"><span data-stu-id="1cce7-110">The application ID from the Azure portal</span></span> |
    | `GraphScopes` | <span data-ttu-id="1cce7-111">数组</span><span class="sxs-lookup"><span data-stu-id="1cce7-111">Array</span></span> | <span data-ttu-id="1cce7-112">两个字符串值`User.Read` ：和`Calendars.Read`</span><span class="sxs-lookup"><span data-stu-id="1cce7-112">Two String values: `User.Read` and `Calendars.Read`</span></span> |

    ![Xcode 中的 plist 文件的屏幕截图 AuthSettings](./images/auth-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="1cce7-114">如果您使用的是源代码管理（如 git），现在是从源代码控制中排除**AuthSettings**文件以避免无意中泄漏您的应用程序 ID 的最佳时机。</span><span class="sxs-lookup"><span data-stu-id="1cce7-114">If you're using source control such as git, now would be a good time to exclude the **AuthSettings.plist** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="1cce7-115">实施登录</span><span class="sxs-lookup"><span data-stu-id="1cce7-115">Implement sign-in</span></span>

<span data-ttu-id="1cce7-116">在本节中，您将为 MSAL 配置项目，创建身份验证管理器类，并更新应用程序以进行登录和注销。</span><span class="sxs-lookup"><span data-stu-id="1cce7-116">In this section you will configure the project for MSAL, create an authentication manager class, and update the app to sign in and sign out.</span></span>

### <a name="configure-project-for-msal"></a><span data-ttu-id="1cce7-117">为 MSAL 配置项目</span><span class="sxs-lookup"><span data-stu-id="1cce7-117">Configure project for MSAL</span></span>

1. <span data-ttu-id="1cce7-118">控件单击 " **plist** "，然后选择 "**打开方式**"，然后选择 "**源代码**"。</span><span class="sxs-lookup"><span data-stu-id="1cce7-118">Control click **Info.plist** and select **Open As**, then **Source Code**.</span></span>
1. <span data-ttu-id="1cce7-119">在`<dict>`元素内添加以下项。</span><span class="sxs-lookup"><span data-stu-id="1cce7-119">Add the following inside the `<dict>` element.</span></span>

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
    <array>
        <string>msauthv2</string>
        <string>msauthv3</string>
    </array>
    ```

1. <span data-ttu-id="1cce7-120">打开**AppDelegate** ，并在文件的顶部添加以下导入语句。</span><span class="sxs-lookup"><span data-stu-id="1cce7-120">Open **AppDelegate.swift** and add the following import statement at the top of the file.</span></span>

    ```Swift
    import MSAL
    ```

1. <span data-ttu-id="1cce7-121">将以下函数添加到 `AppDelegate` 类。</span><span class="sxs-lookup"><span data-stu-id="1cce7-121">Add the following function to the `AppDelegate` class.</span></span>

    ```Swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {

        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }

        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
    ```

### <a name="create-authentication-manager"></a><span data-ttu-id="1cce7-122">创建身份验证管理器</span><span class="sxs-lookup"><span data-stu-id="1cce7-122">Create authentication manager</span></span>

1. <span data-ttu-id="1cce7-123">在名为**authenticationmanager.java**的**GraphTutorial**项目中创建一个新的**Swift 文件**。</span><span class="sxs-lookup"><span data-stu-id="1cce7-123">Create a new **Swift File** in the **GraphTutorial** project named **AuthenticationManager.swift**.</span></span> <span data-ttu-id="1cce7-124">将以下代码添加到文件中。</span><span class="sxs-lookup"><span data-stu-id="1cce7-124">Add the following code to the file.</span></span>

    ```Swift
    import Foundation
    import MSAL

    class AuthenticationManager {

        // Implement singleton pattern
        static let instance = AuthenticationManager()

        private let publicClient: MSALPublicClientApplication?
        private let appId: String
        private let graphScopes: Array<String>

        private init() {
            // Get app ID and scopes from AuthSettings.plist
            let bundle = Bundle.main
            let authConfigPath = bundle.path(forResource: "AuthSettings", ofType: "plist")!
            let authConfig = NSDictionary(contentsOfFile: authConfigPath)!

            self.appId = authConfig["AppId"] as! String
            self.graphScopes = authConfig["GraphScopes"] as! Array<String>

            do {
                // Create the MSAL client
                try self.publicClient = MSALPublicClientApplication(clientId: self.appId,
                                                                    keychainGroup: bundle.bundleIdentifier)
            } catch {
                print("Error creating MSAL public client: \(error)")
                self.publicClient = nil
            }
        }

        public func getPublicClient() -> MSALPublicClientApplication? {
            return self.publicClient
        }

        public func getTokenInteractively(completion: @escaping(_ accessToken: String?, Error?) -> Void) {
            // Call acquireToken to open a browser so the user can sign in
            publicClient?.acquireToken(forScopes: self.graphScopes, completionBlock: {
                (result: MSALResult?, error: Error?) in
                guard let tokenResult = result, error == nil else {
                    print("Error getting token interactively: \(String(describing: error))")
                    completion(nil, error)
                    return
                }

                print("Got token interactively: \(tokenResult.accessToken)")
                completion(tokenResult.accessToken, nil)
            })
        }

        public func getTokenSilently(completion: @escaping(_ accessToken: String?, Error?) -> Void) {
            // Check if there is an account in the cache
            var userAccount: MSALAccount?

            do {
                userAccount = try publicClient?.allAccounts().first
            } catch {
                print("Error getting account: \(error)")
            }

            if (userAccount != nil) {
                // Attempt to get token silently
                publicClient?.acquireTokenSilent(forScopes: self.graphScopes, account: userAccount!, completionBlock: {
                    (result: MSALResult?, error: Error?) in
                    guard let tokenResult = result, error == nil else {
                        print("Error getting token silently: \(String(describing: error))")
                        completion(nil, error)
                        return
                    }

                    print("Got token silently: \(tokenResult.accessToken)")
                    completion(tokenResult.accessToken, nil)
                })
            } else {
                print("No account in cache")
                completion(nil, NSError(domain: "AuthenticationManager",
                                        code: MSALErrorCode.interactionRequired.rawValue, userInfo: nil))
            }
        }

        public func signOut() -> Void {
            do {
                // Remove all accounts from the cache
                let accounts = try publicClient?.allAccounts()

                try accounts!.forEach({
                    (account: MSALAccount) in
                    try publicClient?.remove(account)
                })
            } catch {
                print("Sign out error: \(String(describing: error))")
            }
        }
    }
    ```

### <a name="add-sign-in-and-sign-out"></a><span data-ttu-id="1cce7-125">添加登录和注销</span><span class="sxs-lookup"><span data-stu-id="1cce7-125">Add sign-in and sign-out</span></span>

1. <span data-ttu-id="1cce7-126">打开**SignInViewController** ，并将其内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="1cce7-126">Open **SignInViewController.swift** and replace its contents with the following code.</span></span>

    ```Swift
    import UIKit

    class SignInViewController: UIViewController {

        private let spinner = SpinnerViewController()

        override func viewDidLoad() {
            super.viewDidLoad()
            // Do any additional setup after loading the view.

            // See if a user is already signed in
            spinner.start(container: self)

            AuthenticationManager.instance.getTokenSilently {
                (token: String?, error: Error?) in

                DispatchQueue.main.async {
                    self.spinner.stop()

                    guard let _ = token, error == nil else {
                        // If there is no token or if there's an error,
                        // no user is signed in, so stay here
                        return
                    }

                    // Since we got a token, a user is signed in
                    // Go to welcome page
                    self.performSegue(withIdentifier: "userSignedIn", sender: nil)
                }
            }
        }

        @IBAction func signIn() {
            spinner.start(container: self)

            // Do an interactive sign in
            AuthenticationManager.instance.getTokenInteractively {
                (token: String?, error: Error?) in

                DispatchQueue.main.async {
                    self.spinner.stop()

                    guard let _ = token, error == nil else {
                        // Show the error and stay on the sign-in page
                        let alert = UIAlertController(title: "Error signing in",
                                                      message: error.debugDescription,
                                                      preferredStyle: .alert)

                        alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
                        self.present(alert, animated: true)
                        return
                    }

                    // Signed in successfully
                    // Go to welcome page
                    self.performSegue(withIdentifier: "userSignedIn", sender: nil)
                }
            }
        }
    }
    ```

1. <span data-ttu-id="1cce7-127">打开**WelcomeViewController** ，并将现有`signOut`函数替换为以下项。</span><span class="sxs-lookup"><span data-stu-id="1cce7-127">Open **WelcomeViewController.swift** and replace the existing `signOut` function with the following.</span></span>

    ```Swift
    @IBAction func signOut() {
        AuthenticationManager.instance.signOut()
        self.performSegue(withIdentifier: "userSignedOut", sender: nil)
    }
    ```

1. <span data-ttu-id="1cce7-128">保存所做的更改，并在模拟器中重新启动应用程序。</span><span class="sxs-lookup"><span data-stu-id="1cce7-128">Save your changes and restart the application in Simulator.</span></span>

<span data-ttu-id="1cce7-129">如果登录到应用，您应该会看到在 Xcode 的输出窗口中显示的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="1cce7-129">If you sign in to the app, you should see an access token displayed in the output window in Xcode.</span></span>

![显示访问令牌的 Xcode 中的输出窗口的屏幕截图](./images/access-token-output.png)

## <a name="get-user-details"></a><span data-ttu-id="1cce7-131">获取用户详细信息</span><span class="sxs-lookup"><span data-stu-id="1cce7-131">Get user details</span></span>

<span data-ttu-id="1cce7-132">在本节中，您将创建一个帮助程序类，以保存对 Microsoft Graph 的所有调用并`WelcomeViewController`更新，以使用此新类获取已登录的用户。</span><span class="sxs-lookup"><span data-stu-id="1cce7-132">In this section you will create a helper class to hold all of the calls to Microsoft Graph and update the `WelcomeViewController` to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="1cce7-133">打开**authenticationmanager.java** ，并将以下函数添加到`AuthenticationManager`类中。</span><span class="sxs-lookup"><span data-stu-id="1cce7-133">Open **AuthenticationManager.swift** and add the following function to the `AuthenticationManager` class.</span></span>

    ```Swift
    public func getGraphAuthProvider() -> MSALAuthenticationProvider? {
        // Create an MSAL auth provider for use with the Graph client
        return MSALAuthenticationProvider(publicClientApplication: self.publicClient,
                                          andScopes: self.graphScopes)
    }
    ```

1. <span data-ttu-id="1cce7-134">在名为**GraphManager**的**GraphTutorial**项目中创建一个新的**Swift 文件**。</span><span class="sxs-lookup"><span data-stu-id="1cce7-134">Create a new **Swift File** in the **GraphTutorial** project named **GraphManager.swift**.</span></span> <span data-ttu-id="1cce7-135">将以下代码添加到文件中。</span><span class="sxs-lookup"><span data-stu-id="1cce7-135">Add the following code to the file.</span></span>

    ```Swift
    import Foundation
    import MSGraphClientSDK
    import MSGraphClientModels

    class GraphManager {

        // Implement singleton pattern
        static let instance = GraphManager()

        private let client: MSHTTPClient?

        private init() {
            client = MSClientFactory.createHTTPClient(with: AuthenticationManager.instance.getGraphAuthProvider())
        }

        public func getMe(completion: @escaping(MSGraphUser?, Error?) -> Void) {
            // GET /me
            let meRequest = NSMutableURLRequest(url: URL(string: "\(MSGraphBaseURL)/me")!)
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

1. <span data-ttu-id="1cce7-136">打开**WelcomeViewController** ，并在文件顶部添加`import`以下语句。</span><span class="sxs-lookup"><span data-stu-id="1cce7-136">Open **WelcomeViewController.swift** and add the following `import` statement at the top of the file.</span></span>

    ```Swift
    import MSGraphClientModels
    ```

1. <span data-ttu-id="1cce7-137">将以下属性添加到`WelcomeViewController`类中。</span><span class="sxs-lookup"><span data-stu-id="1cce7-137">Add the following property to the `WelcomeViewController` class.</span></span>

    ```Swift
    private let spinner = SpinnerViewController()
    ```

1. <span data-ttu-id="1cce7-138">将现有`viewDidLoad`替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="1cce7-138">Replace the existing `viewDidLoad` with the following code.</span></span>

    ```Swift
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.

        self.spinner.start(container: self)

        // Get the signed-in user
        self.userProfilePhoto.image = UIImage(imageLiteralResourceName: "DefaultUserPhoto")

        GraphManager.instance.getMe {
            (user: MSGraphUser?, error: Error?) in

            DispatchQueue.main.async {
                self.spinner.stop()

                guard let currentUser = user, error == nil else {
                    print("Error getting user: \(String(describing: error))")
                    return
                }

                // Set display name
                self.userDisplayName.text = currentUser.displayName ?? "Mysterious Stranger"
                self.userDisplayName.sizeToFit()

                // AAD users have email in the mail attribute
                // Personal accounts have email in the userPrincipalName attribute
                self.userEmail.text = currentUser.mail ?? currentUser.userPrincipalName ?? ""
                self.userEmail.sizeToFit()
            }
        }
    }
    ```

<span data-ttu-id="1cce7-139">如果现在保存更改并重新启动应用程序，则在使用用户的显示名称和电子邮件地址更新 UI 后，登录 UI。</span><span class="sxs-lookup"><span data-stu-id="1cce7-139">If you save your changes and restart the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
