<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3f9e2-101">首先创建新的 Swift 项目。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-101">Begin by creating a new Swift project.</span></span>

1. <span data-ttu-id="3f9e2-102">打开 Xcode。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-102">Open Xcode.</span></span> <span data-ttu-id="3f9e2-103">在"**文件"** 菜单上，**选择"新建"，** 然后选择"**项目"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-103">On the **File** menu, select **New**, then **Project**.</span></span>
1. <span data-ttu-id="3f9e2-104">选择应用 **模板**，然后选择"下 **一步"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-104">Choose the **App** template and select **Next**.</span></span>

    ![Xcode 模板选择对话框的屏幕截图](images/xcode-select-template.png)

1. <span data-ttu-id="3f9e2-106">将 **产品名称设置为** `GraphTutorial` ，将 **语言设置为** **Swift。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-106">Set the **Product Name** to `GraphTutorial` and the **Language** to **Swift**.</span></span>
1. <span data-ttu-id="3f9e2-107">填写其余字段，然后选择"下 **一步"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-107">Fill in the remaining fields and select **Next**.</span></span>
1. <span data-ttu-id="3f9e2-108">选择项目的位置，然后选择"创建 **"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-108">Choose a location for the project and select **Create**.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="3f9e2-109">安装依赖项</span><span class="sxs-lookup"><span data-stu-id="3f9e2-109">Install dependencies</span></span>

<span data-ttu-id="3f9e2-110">在继续之前，请安装一些稍后将使用的其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-110">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="3f9e2-111">[Microsoft 身份验证库 (MSAL) for iOS，](https://github.com/AzureAD/microsoft-authentication-library-for-objc) 用于使用 Azure AD 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-111">[Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) for authenticating to with Azure AD.</span></span>
- <span data-ttu-id="3f9e2-112">[用于调用 Microsoft](https://github.com/microsoftgraph/msgraph-sdk-objc) Graph 的目标 C 的 Microsoft Graph SDK。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-112">[Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="3f9e2-113">[用于表示 Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-objc-models) 资源（如用户或事件）的强类型对象的目标 C 的 Microsoft Graph 模型 SDK。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-113">[Microsoft Graph Models SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) for strongly-typed objects representing Microsoft Graph resources like users or events.</span></span>

1. <span data-ttu-id="3f9e2-114">退出 Xcode。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-114">Quit Xcode.</span></span>
1. <span data-ttu-id="3f9e2-115">打开终端，将目录更改为 **GraphTu一l** 项目的位置。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-115">Open Terminal and change the directory to the location of your **GraphTutorial** project.</span></span>
1. <span data-ttu-id="3f9e2-116">运行以下命令以创建 Podfile。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-116">Run the following command to create a Podfile.</span></span>

    ```Shell
    pod init
    ```

1. <span data-ttu-id="3f9e2-117">打开 Podfile，并添加行后的以下 `use_frameworks!` 行。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-117">Open the Podfile and add the following lines just after the `use_frameworks!` line.</span></span>

    ```Ruby
    pod 'MSAL', '~> 1.1.13'
    pod 'MSGraphClientSDK', ' ~> 1.0.0'
    pod 'MSGraphClientModels', '~> 1.3.0'
    ```

1. <span data-ttu-id="3f9e2-118">保存 Podfile，然后运行以下命令以安装依赖项。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-118">Save the Podfile, then run the following command to install the dependencies.</span></span>

    ```Shell
    pod install
    ```

1. <span data-ttu-id="3f9e2-119">命令完成后，在 Xcode 中打开新建 **的 GraphTu一l.xcworkspace。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-119">Once the command completes, open the newly created **GraphTutorial.xcworkspace** in Xcode.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="3f9e2-120">设计应用</span><span class="sxs-lookup"><span data-stu-id="3f9e2-120">Design the app</span></span>

<span data-ttu-id="3f9e2-121">在此部分中，你将为应用创建视图：登录页、选项卡栏导航器、欢迎页面和日历页。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-121">In this section you will create the views for the app: a sign in page, a tab bar navigator, a welcome page, and a calendar page.</span></span> <span data-ttu-id="3f9e2-122">你还将创建活动指示器覆盖。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-122">You'll also create an activity indicator overlay.</span></span>

### <a name="create-sign-in-page"></a><span data-ttu-id="3f9e2-123">创建登录页</span><span class="sxs-lookup"><span data-stu-id="3f9e2-123">Create sign in page</span></span>

1. <span data-ttu-id="3f9e2-124">在 **Xcode 中展开 GraphTu一l** 文件夹，然后选择 **ViewController.swift。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-124">Expand the **GraphTutorial** folder in Xcode, then select **ViewController.swift**.</span></span>
1. <span data-ttu-id="3f9e2-125">在 **文件检查器** 中 **，将文件** 的名称更改为 `SignInViewController.swift` 。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-125">In the **File Inspector**, change the **Name** of the file to `SignInViewController.swift`.</span></span>

    ![文件检查器屏幕截图](images/rename-file.png)

1. <span data-ttu-id="3f9e2-127">打开 **SignInViewController.swift，** 并将其内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-127">Open **SignInViewController.swift** and replace its contents with the following code.</span></span>

    ```Swift
    import UIKit

    class SignInViewController: UIViewController {

        override func viewDidLoad() {
            super.viewDidLoad()
            // Do any additional setup after loading the view.
        }

        @IBAction func signIn() {
            self.performSegue(withIdentifier: "userSignedIn", sender: nil)
        }
    }
    ```

1. <span data-ttu-id="3f9e2-128">打开 **Main.storyboard**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-128">Open **Main.storyboard**.</span></span> <span data-ttu-id="3f9e2-129">展开 **"视图控制器场景**"，然后选择 **"视图控制器"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-129">Expand **View Controller Scene**, then select **View Controller**.</span></span>

    ![选中视图控制器的 Xcode 屏幕截图](images/storyboard-select-view-controller.png)

1. <span data-ttu-id="3f9e2-131">选择 **标识检查器**，然后将 **类下拉列表更改为** **SignInViewController。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-131">Select the **Identity Inspector**, then change the **Class** dropdown to **SignInViewController**.</span></span>

    ![标识检查器屏幕截图](images/change-class.png)

1. <span data-ttu-id="3f9e2-133">选择 **库**，然后将 **按钮** 拖动到 **登录视图控制器上**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-133">Select the **Library**, then drag a **Button** onto the **Sign In View Controller**.</span></span>

    ![Xcode 中的库屏幕截图](images/add-button-to-view.png)

1. <span data-ttu-id="3f9e2-135">选中按钮后，选择 **属性检查** 器， **将按钮的标题** 更改为 `Sign In` 。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-135">With the button selected, select the **Attributes Inspector** and change the **Title** of the button to `Sign In`.</span></span>

    ![Xcode 中属性检查器中标题字段的屏幕截图](images/set-button-title.png)

1. <span data-ttu-id="3f9e2-137">选中按钮后 **，选择情节** 提要底部的"对齐"按钮。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-137">With the button selected, select the **Align** button at the bottom of the storyboard.</span></span> <span data-ttu-id="3f9e2-138">在容器中 **选择"水平"** 和"垂直"容器约束，将其值保留为 0，然后选择"添加 **2 个约束"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-138">Select both the **Horizontally in container** and **Vertically in container** constraints, leave their values as 0, then select **Add 2 constraints**.</span></span>

    ![Xcode 中的对齐约束设置的屏幕截图](images/add-alignment-constraints.png)

1. <span data-ttu-id="3f9e2-140">选择 **登录视图控制器**，然后选择连接 **检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-140">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="3f9e2-141">在 **"已** 接收操作"下，将未填充的圆圈拖到 **按钮的"登录** "旁边。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-141">Under **Received Actions**, drag the unfilled circle next to **signIn** onto the button.</span></span> <span data-ttu-id="3f9e2-142">在 **弹出菜单上** 选择"内部触摸"。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-142">Select **Touch Up Inside** on the pop-up menu.</span></span>

    ![将 signIn 操作拖动到 Xcode 中的按钮的屏幕截图](images/connect-sign-in-button.png)

### <a name="create-tab-bar"></a><span data-ttu-id="3f9e2-144">创建选项卡栏</span><span class="sxs-lookup"><span data-stu-id="3f9e2-144">Create tab bar</span></span>

1. <span data-ttu-id="3f9e2-145">选择 **库**，然后将 **选项卡栏控制器** 拖动到情节提要上。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-145">Select the **Library**, then drag a **Tab Bar Controller** onto the storyboard.</span></span>
1. <span data-ttu-id="3f9e2-146">选择 **登录视图控制器**，然后选择连接 **检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-146">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="3f9e2-147">在 **"触发的 Segues"** 下，将手动旁的未填充圆圈拖到情节提要上的 **选项卡** 栏控制器上。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-147">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Tab Bar Controller** on the storyboard.</span></span> <span data-ttu-id="3f9e2-148">在 **弹出菜单中** 选择"以模式方式显示"。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-148">Select **Present Modally** in the pop-up menu.</span></span>

    ![在 Xcode 中将手动标签拖动到新选项卡栏控制器的屏幕截图](images/add-segue.png)

1. <span data-ttu-id="3f9e2-150">选择刚添加的 segue，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-150">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="3f9e2-151">将 **"标识符"** 字段设置为 `userSignedIn` ，将 **"演示文稿"** 设置为 **"全屏"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-151">Set the **Identifier** field to `userSignedIn`, and set **Presentation** to **Full Screen**.</span></span>

    ![Xcode 中属性检查器中"标识符"字段的屏幕截图](images/set-segue-identifier.png)

1. <span data-ttu-id="3f9e2-153">选择 **项目 1 场景**，然后选择连接 **检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-153">Select the **Item 1 Scene**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="3f9e2-154">在 **"触发的 Segues"** 下，将手动旁的未填充圆圈拖到情节提要上的登录 **视图** 控制器上。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-154">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Sign In View Controller** on the storyboard.</span></span> <span data-ttu-id="3f9e2-155">在 **弹出菜单中** 选择"以模式方式显示"。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-155">Select **Present Modally** in the pop-up menu.</span></span>
1. <span data-ttu-id="3f9e2-156">选择刚添加的 segue，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-156">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="3f9e2-157">将 **"标识符"** 字段设置为 `userSignedOut` ，将 **"演示文稿"** 设置为 **"全屏"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-157">Set the **Identifier** field to `userSignedOut`, and set **Presentation** to **Full Screen**.</span></span>

### <a name="create-welcome-page"></a><span data-ttu-id="3f9e2-158">创建欢迎页面</span><span class="sxs-lookup"><span data-stu-id="3f9e2-158">Create welcome page</span></span>

1. <span data-ttu-id="3f9e2-159">选择 **Assets.xcassets** 文件。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-159">Select the **Assets.xcassets** file.</span></span>
1. <span data-ttu-id="3f9e2-160">在编辑器 **菜单** 上，**选择"添加新资产"，** 然后选择 **"图像集"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-160">On the **Editor** menu, select **Add New Asset**, then **Image Set**.</span></span>
1. <span data-ttu-id="3f9e2-161">选择新的 **Image** 资源并使用 **属性检查** 器将其 **名称设置为** `DefaultUserPhoto` 。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-161">Select the new **Image** asset and use the **Attribute Inspector** to set its **Name** to `DefaultUserPhoto`.</span></span>
1. <span data-ttu-id="3f9e2-162">添加要用作默认用户配置文件照片的任何图像。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-162">Add any image you like to serve as a default user profile photo.</span></span>

    ![Xcode 中图像集资产视图的屏幕截图](images/add-default-user-photo.png)

1. <span data-ttu-id="3f9e2-164">在 **GraphTu一** l 文件夹中 **新建一** 个名为 Cocoa Touch 类的文件 `WelcomeViewController` 。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-164">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `WelcomeViewController`.</span></span> <span data-ttu-id="3f9e2-165">在 **字段的子类中选择 UIViewController。** </span><span class="sxs-lookup"><span data-stu-id="3f9e2-165">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="3f9e2-166">打开 **WelcomeViewController.swift，** 并将其内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-166">Open **WelcomeViewController.swift** and replace its contents with the following code.</span></span>

    ```Swift
    import UIKit

    class WelcomeViewController: UIViewController {

        @IBOutlet var userProfilePhoto: UIImageView!
        @IBOutlet var userDisplayName: UILabel!
        @IBOutlet var userEmail: UILabel!

        override func viewDidLoad() {
            super.viewDidLoad()

            // Do any additional setup after loading the view.

            // TEMPORARY
            self.userProfilePhoto.image = UIImage(imageLiteralResourceName: "DefaultUserPhoto")
            self.userDisplayName.text = "Default User"
            self.userEmail.text = "default@contoso.com"
        }

        @IBAction func signOut() {
            self.performSegue(withIdentifier: "userSignedOut", sender: nil)
        }
    }
    ```

1. <span data-ttu-id="3f9e2-167">打开 **Main.storyboard**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-167">Open **Main.storyboard**.</span></span> <span data-ttu-id="3f9e2-168">选择 **项目 1 场景**，然后选择 **标识检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-168">Select the **Item 1 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="3f9e2-169">将 **类** 值更改为 **WelcomeViewController**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-169">Change the **Class** value to **WelcomeViewController**.</span></span>
1. <span data-ttu-id="3f9e2-170">使用 **库，** 将以下项添加到项目 **1 场景**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-170">Using the **Library**, add the following items to the **Item 1 Scene**.</span></span>

    - <span data-ttu-id="3f9e2-171">一 **个图像视图**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-171">One **Image View**</span></span>
    - <span data-ttu-id="3f9e2-172">两 **个标签**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-172">Two **Labels**</span></span>
    - <span data-ttu-id="3f9e2-173">一 **个按钮**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-173">One **Button**</span></span>
1. <span data-ttu-id="3f9e2-174">使用连接 **检查器** 进行以下连接。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-174">Using the **Connections Inspector**, make the following connections.</span></span>

    - <span data-ttu-id="3f9e2-175">将 **userDisplayName** 出口链接到第一个标签。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-175">Link the **userDisplayName** outlet to the first label.</span></span>
    - <span data-ttu-id="3f9e2-176">将 **userEmail 出口** 链接到第二个标签。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-176">Link the **userEmail** outlet to the second label.</span></span>
    - <span data-ttu-id="3f9e2-177">将 **userProfilePhoto** 出口链接到图像视图。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-177">Link the **userProfilePhoto** outlet to the image view.</span></span>
    - <span data-ttu-id="3f9e2-178">将 **signOut** 接收的操作链接到按钮的 **"内部触摸"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-178">Link the **signOut** received action to the button's **Touch Up Inside**.</span></span>

1. <span data-ttu-id="3f9e2-179">选择图像视图，然后选择大小 **检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-179">Select the image view, then select the **Size Inspector**.</span></span>
1. <span data-ttu-id="3f9e2-180">将 **Width** 和 **Height** 设置为 196。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-180">Set the **Width** and **Height** to 196.</span></span>
1. <span data-ttu-id="3f9e2-181">使用 **"对齐**"按钮可添加值为 0 的"水平"容器约束。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-181">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="3f9e2-182">使用 **"对齐" (** 旁边的"添加新约束"按钮) 添加以下约束：</span><span class="sxs-lookup"><span data-stu-id="3f9e2-182">Use the **Add New Constraints** button (next to the **Align** button) to add the following constraints:</span></span>

    - <span data-ttu-id="3f9e2-183">顶部对齐：安全区域，值：0</span><span class="sxs-lookup"><span data-stu-id="3f9e2-183">Align Top to: Safe Area, value: 0</span></span>
    - <span data-ttu-id="3f9e2-184">底部空间：用户显示名称、值：Standard</span><span class="sxs-lookup"><span data-stu-id="3f9e2-184">Bottom Space to: User Display Name, value: Standard</span></span>
    - <span data-ttu-id="3f9e2-185">高度，值：196</span><span class="sxs-lookup"><span data-stu-id="3f9e2-185">Height, value: 196</span></span>
    - <span data-ttu-id="3f9e2-186">宽度，值：196</span><span class="sxs-lookup"><span data-stu-id="3f9e2-186">Width, value: 196</span></span>

    ![Xcode 中新约束设置的屏幕截图](./images/add-new-constraints.png)

1. <span data-ttu-id="3f9e2-188">选择第一个标签，然后使用 **"对齐** "按钮添加值为 0 的 **"水平"** 容器约束。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-188">Select the first label, then use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="3f9e2-189">使用 **"添加新约束"** 按钮可添加以下约束：</span><span class="sxs-lookup"><span data-stu-id="3f9e2-189">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="3f9e2-190">顶部空间：用户配置文件照片，值：标准</span><span class="sxs-lookup"><span data-stu-id="3f9e2-190">Top Space to: User Profile Photo, value: Standard</span></span>
    - <span data-ttu-id="3f9e2-191">底部空间：用户电子邮件，值：标准</span><span class="sxs-lookup"><span data-stu-id="3f9e2-191">Bottom Space to: User Email, value: Standard</span></span>

1. <span data-ttu-id="3f9e2-192">选择第二个标签，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-192">Select the second label, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="3f9e2-193">将颜色 **更改为\*\*\*\*深灰色**，将 **字体更改为\*\*\*\*系统 12.0。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-193">Change the **Color** to **Dark Gray Color**, and change the **Font** to **System 12.0**.</span></span>
1. <span data-ttu-id="3f9e2-194">使用 **"对齐** "按钮添加值为 0 的 **"** 水平"容器约束。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-194">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="3f9e2-195">使用 **"添加新约束"** 按钮可添加以下约束：</span><span class="sxs-lookup"><span data-stu-id="3f9e2-195">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="3f9e2-196">顶部空间：用户显示名称、值：Standard</span><span class="sxs-lookup"><span data-stu-id="3f9e2-196">Top Space to: User Display Name, value: Standard</span></span>
    - <span data-ttu-id="3f9e2-197">底部空间：注销，值：14</span><span class="sxs-lookup"><span data-stu-id="3f9e2-197">Bottom Space to: Sign Out, value: 14</span></span>

1. <span data-ttu-id="3f9e2-198">选择该按钮，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-198">Select the button, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="3f9e2-199">将 **标题更改为** `Sign Out` 。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-199">Change the **Title** to `Sign Out`.</span></span>
1. <span data-ttu-id="3f9e2-200">使用 **"对齐**"按钮可添加值为 0 的"水平"容器约束。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-200">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="3f9e2-201">使用 **"添加新约束"** 按钮可添加以下约束：</span><span class="sxs-lookup"><span data-stu-id="3f9e2-201">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="3f9e2-202">顶部空间：用户电子邮件，值：14</span><span class="sxs-lookup"><span data-stu-id="3f9e2-202">Top Space to: User Email, value: 14</span></span>

1. <span data-ttu-id="3f9e2-203">选择场景底部的选项卡栏项，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-203">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="3f9e2-204">将 **标题更改为** `Me` 。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-204">Change the **Title** to `Me`.</span></span>

<span data-ttu-id="3f9e2-205">完成后，欢迎场景应类似于此场景。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-205">The welcome scene should look similar to this once you're done.</span></span>

![欢迎场景布局的屏幕截图](images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a><span data-ttu-id="3f9e2-207">创建日历页</span><span class="sxs-lookup"><span data-stu-id="3f9e2-207">Create calendar page</span></span>

1. <span data-ttu-id="3f9e2-208">在 **GraphTu一** l 文件夹中 **新建一** 个名为 Cocoa Touch 类的文件 `CalendarViewController` 。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-208">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `CalendarViewController`.</span></span> <span data-ttu-id="3f9e2-209">在 **字段的子类中选择 UIViewController。** </span><span class="sxs-lookup"><span data-stu-id="3f9e2-209">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="3f9e2-210">打开 **CalendarViewController.swift，** 并将其内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-210">Open **CalendarViewController.swift** and replace its contents with the following code.</span></span>

    ```Swift
    import UIKit

    class CalendarViewController: UIViewController {

        @IBOutlet var calendarJSON: UITextView!

        override func viewDidLoad() {
            super.viewDidLoad()

            // Do any additional setup after loading the view.

            // TEMPORARY
            calendarJSON.text = "Calendar"
            calendarJSON.sizeToFit()
        }
    }
    ```

1. <span data-ttu-id="3f9e2-211">打开 **Main.storyboard**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-211">Open **Main.storyboard**.</span></span> <span data-ttu-id="3f9e2-212">选择 **项目 2 场景**，然后选择 **标识检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-212">Select the **Item 2 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="3f9e2-213">将 **类** 值更改为 **CalendarViewController**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-213">Change the **Class** value to **CalendarViewController**.</span></span>
1. <span data-ttu-id="3f9e2-214">使用库 **，** 将 **文本视图添加到\*\*\*\*项目 2 场景**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-214">Using the **Library**, add a **Text View** to the **Item 2 Scene**.</span></span>
1. <span data-ttu-id="3f9e2-215">选择刚刚添加的文本视图。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-215">Select the text view you just added.</span></span> <span data-ttu-id="3f9e2-216">在编辑器 **菜单** 上，选择 **"嵌入"，** 然后选择 **"滚动视图"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-216">On the **Editor** menu, choose **Embed In**, then **Scroll View**.</span></span>
1. <span data-ttu-id="3f9e2-217">调整滚动视图和文本视图的大小以使用整个屏幕。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-217">Resize the scroll view and text view to take up the entire screen.</span></span>
1. <span data-ttu-id="3f9e2-218">使用连接 **检查器**，将 **calendarJSON** 出口连接到文本视图。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-218">Using the **Connections Inspector**, connect the **calendarJSON** outlet to the text view.</span></span>
1. <span data-ttu-id="3f9e2-219">选择场景底部的选项卡栏项，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-219">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="3f9e2-220">将 **标题更改为** `Calendar` 。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-220">Change the **Title** to `Calendar`.</span></span>
1. <span data-ttu-id="3f9e2-221">在 **编辑器菜单** 上，选择 **"解决自动布局** 问题"，然后选择"在日历视图控制器的所有视图下方添加缺少 **的约束"。**</span><span class="sxs-lookup"><span data-stu-id="3f9e2-221">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Calendar View Controller**.</span></span>

<span data-ttu-id="3f9e2-222">完成后，日历场景看起来应该与此类似。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-222">The calendar scene should look similar to this once you're done.</span></span>

![日历场景布局的屏幕截图](images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a><span data-ttu-id="3f9e2-224">创建活动指示器</span><span class="sxs-lookup"><span data-stu-id="3f9e2-224">Create activity indicator</span></span>

1. <span data-ttu-id="3f9e2-225">在 **GraphTu一** l 文件夹中 **新建一** 个名为 Cocoa Touch 类的文件 `SpinnerViewController` 。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-225">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `SpinnerViewController`.</span></span> <span data-ttu-id="3f9e2-226">在 **字段的子类中选择 UIViewController。** </span><span class="sxs-lookup"><span data-stu-id="3f9e2-226">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="3f9e2-227">打开 **SpinnerViewController.swift，** 并将其内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-227">Open **SpinnerViewController.swift** and replace its contents with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/SpinnerViewController.swift" id="SpinnerSnippet":::

## <a name="test-the-app"></a><span data-ttu-id="3f9e2-228">测试应用程序</span><span class="sxs-lookup"><span data-stu-id="3f9e2-228">Test the app</span></span>

<span data-ttu-id="3f9e2-229">保存更改并启动应用。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-229">Save your changes and launch the app.</span></span> <span data-ttu-id="3f9e2-230">你应该能够使用"登录"和"注销 **"按钮和** 选项卡栏在屏幕之间移动。</span><span class="sxs-lookup"><span data-stu-id="3f9e2-230">You should be able to move between the screens using the **Sign In** and **Sign Out** buttons and the tab bar.</span></span>

![应用程序的屏幕截图](images/app-screens.png)
