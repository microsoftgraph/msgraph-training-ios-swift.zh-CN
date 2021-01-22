<!-- markdownlint-disable MD002 MD041 -->

首先创建新的 Swift 项目。

1. 打开 Xcode。 在"**文件"** 菜单上，**选择"新建"，** 然后选择"**项目"。**
1. 选择应用 **模板**，然后选择"下 **一步"。**

    ![Xcode 模板选择对话框的屏幕截图](images/xcode-select-template.png)

1. 将 **产品名称设置为** `GraphTutorial` ，将 **语言设置为** **Swift。**
1. 填写其余字段，然后选择"下 **一步"。**
1. 选择项目的位置，然后选择"创建 **"。**

## <a name="install-dependencies"></a>安装依赖项

在继续之前，请安装一些稍后将使用的其他依赖项。

- [Microsoft 身份验证库 (MSAL) for iOS，](https://github.com/AzureAD/microsoft-authentication-library-for-objc) 用于使用 Azure AD 进行身份验证。
- [用于调用 Microsoft](https://github.com/microsoftgraph/msgraph-sdk-objc) Graph 的目标 C 的 Microsoft Graph SDK。
- [用于表示 Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-objc-models) 资源（如用户或事件）的强类型对象的目标 C 的 Microsoft Graph 模型 SDK。

1. 退出 Xcode。
1. 打开终端，将目录更改为 **GraphTu一l** 项目的位置。
1. 运行以下命令以创建 Podfile。

    ```Shell
    pod init
    ```

1. 打开 Podfile，并添加行后的以下 `use_frameworks!` 行。

    ```Ruby
    pod 'MSAL', '~> 1.1.13'
    pod 'MSGraphClientSDK', ' ~> 1.0.0'
    pod 'MSGraphClientModels', '~> 1.3.0'
    ```

1. 保存 Podfile，然后运行以下命令以安装依赖项。

    ```Shell
    pod install
    ```

1. 命令完成后，在 Xcode 中打开新建 **的 GraphTu一l.xcworkspace。**

## <a name="design-the-app"></a>设计应用

在此部分中，你将为应用创建视图：登录页、选项卡栏导航器、欢迎页面和日历页。 你还将创建活动指示器覆盖。

### <a name="create-sign-in-page"></a>创建登录页

1. 在 **Xcode 中展开 GraphTu一l** 文件夹，然后选择 **ViewController.swift。**
1. 在 **文件检查器** 中 **，将文件** 的名称更改为 `SignInViewController.swift` 。

    ![文件检查器屏幕截图](images/rename-file.png)

1. 打开 **SignInViewController.swift，** 并将其内容替换为以下代码。

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

1. 打开 **Main.storyboard**。 展开 **"视图控制器场景**"，然后选择 **"视图控制器"。**

    ![选中视图控制器的 Xcode 屏幕截图](images/storyboard-select-view-controller.png)

1. 选择 **标识检查器**，然后将 **类下拉列表更改为** **SignInViewController。**

    ![标识检查器屏幕截图](images/change-class.png)

1. 选择 **库**，然后将 **按钮** 拖动到 **登录视图控制器上**。

    ![Xcode 中的库屏幕截图](images/add-button-to-view.png)

1. 选中按钮后，选择 **属性检查** 器， **将按钮的标题** 更改为 `Sign In` 。

    ![Xcode 中属性检查器中标题字段的屏幕截图](images/set-button-title.png)

1. 选中按钮后 **，选择情节** 提要底部的"对齐"按钮。 在容器中 **选择"水平"** 和"垂直"容器约束，将其值保留为 0，然后选择"添加 **2 个约束"。**

    ![Xcode 中的对齐约束设置的屏幕截图](images/add-alignment-constraints.png)

1. 选择 **登录视图控制器**，然后选择连接 **检查器**。
1. 在 **"已** 接收操作"下，将未填充的圆圈拖到 **按钮的"登录** "旁边。 在 **弹出菜单上** 选择"内部触摸"。

    ![将 signIn 操作拖动到 Xcode 中的按钮的屏幕截图](images/connect-sign-in-button.png)

### <a name="create-tab-bar"></a>创建选项卡栏

1. 选择 **库**，然后将 **选项卡栏控制器** 拖动到情节提要上。
1. 选择 **登录视图控制器**，然后选择连接 **检查器**。
1. 在 **"触发的 Segues"** 下，将手动旁的未填充圆圈拖到情节提要上的 **选项卡** 栏控制器上。 在 **弹出菜单中** 选择"以模式方式显示"。

    ![在 Xcode 中将手动标签拖动到新选项卡栏控制器的屏幕截图](images/add-segue.png)

1. 选择刚添加的 segue，然后选择 **属性检查器**。 将 **"标识符"** 字段设置为 `userSignedIn` ，将 **"演示文稿"** 设置为 **"全屏"。**

    ![Xcode 中属性检查器中"标识符"字段的屏幕截图](images/set-segue-identifier.png)

1. 选择 **项目 1 场景**，然后选择连接 **检查器**。
1. 在 **"触发的 Segues"** 下，将手动旁的未填充圆圈拖到情节提要上的登录 **视图** 控制器上。 在 **弹出菜单中** 选择"以模式方式显示"。
1. 选择刚添加的 segue，然后选择 **属性检查器**。 将 **"标识符"** 字段设置为 `userSignedOut` ，将 **"演示文稿"** 设置为 **"全屏"。**

### <a name="create-welcome-page"></a>创建欢迎页面

1. 选择 **Assets.xcassets** 文件。
1. 在编辑器 **菜单** 上，**选择"添加新资产"，** 然后选择 **"图像集"。**
1. 选择新的 **Image** 资源并使用 **属性检查** 器将其 **名称设置为** `DefaultUserPhoto` 。
1. 添加要用作默认用户配置文件照片的任何图像。

    ![Xcode 中图像集资产视图的屏幕截图](images/add-default-user-photo.png)

1. 在 **GraphTu一** l 文件夹中 **新建一** 个名为 Cocoa Touch 类的文件 `WelcomeViewController` 。 在 **字段的子类中选择 UIViewController。** 
1. 打开 **WelcomeViewController.swift，** 并将其内容替换为以下代码。

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

1. 打开 **Main.storyboard**。 选择 **项目 1 场景**，然后选择 **标识检查器**。 将 **类** 值更改为 **WelcomeViewController**。
1. 使用 **库，** 将以下项添加到项目 **1 场景**。

    - 一 **个图像视图**
    - 两 **个标签**
    - 一 **个按钮**
1. 使用连接 **检查器** 进行以下连接。

    - 将 **userDisplayName** 出口链接到第一个标签。
    - 将 **userEmail 出口** 链接到第二个标签。
    - 将 **userProfilePhoto** 出口链接到图像视图。
    - 将 **signOut** 接收的操作链接到按钮的 **"内部触摸"。**

1. 选择图像视图，然后选择大小 **检查器**。
1. 将 **Width** 和 **Height** 设置为 196。
1. 使用 **"对齐**"按钮可添加值为 0 的"水平"容器约束。
1. 使用 **"对齐" (** 旁边的"添加新约束"按钮) 添加以下约束：

    - 顶部对齐：安全区域，值：0
    - 底部空间：用户显示名称、值：Standard
    - 高度，值：196
    - 宽度，值：196

    ![Xcode 中新约束设置的屏幕截图](./images/add-new-constraints.png)

1. 选择第一个标签，然后使用 **"对齐** "按钮添加值为 0 的 **"水平"** 容器约束。
1. 使用 **"添加新约束"** 按钮可添加以下约束：

    - 顶部空间：用户配置文件照片，值：标准
    - 底部空间：用户电子邮件，值：标准

1. 选择第二个标签，然后选择 **属性检查器**。
1. 将颜色 **更改为****深灰色**，将 **字体更改为****系统 12.0。**
1. 使用 **"对齐** "按钮添加值为 0 的 **"** 水平"容器约束。
1. 使用 **"添加新约束"** 按钮可添加以下约束：

    - 顶部空间：用户显示名称、值：Standard
    - 底部空间：注销，值：14

1. 选择该按钮，然后选择 **属性检查器**。
1. 将 **标题更改为** `Sign Out` 。
1. 使用 **"对齐**"按钮可添加值为 0 的"水平"容器约束。
1. 使用 **"添加新约束"** 按钮可添加以下约束：

    - 顶部空间：用户电子邮件，值：14

1. 选择场景底部的选项卡栏项，然后选择 **属性检查器**。 将 **标题更改为** `Me` 。

完成后，欢迎场景应类似于此场景。

![欢迎场景布局的屏幕截图](images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a>创建日历页

1. 在 **GraphTu一** l 文件夹中 **新建一** 个名为 Cocoa Touch 类的文件 `CalendarViewController` 。 在 **字段的子类中选择 UIViewController。** 
1. 打开 **CalendarViewController.swift，** 并将其内容替换为以下代码。

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

1. 打开 **Main.storyboard**。 选择 **项目 2 场景**，然后选择 **标识检查器**。 将 **类** 值更改为 **CalendarViewController**。
1. 使用库 **，** 将 **文本视图添加到****项目 2 场景**。
1. 选择刚刚添加的文本视图。 在编辑器 **菜单** 上，选择 **"嵌入"，** 然后选择 **"滚动视图"。**
1. 调整滚动视图和文本视图的大小以使用整个屏幕。
1. 使用连接 **检查器**，将 **calendarJSON** 出口连接到文本视图。
1. 选择场景底部的选项卡栏项，然后选择 **属性检查器**。 将 **标题更改为** `Calendar` 。
1. 在 **编辑器菜单** 上，选择 **"解决自动布局** 问题"，然后选择"在日历视图控制器的所有视图下方添加缺少 **的约束"。**

完成后，日历场景看起来应该与此类似。

![日历场景布局的屏幕截图](images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a>创建活动指示器

1. 在 **GraphTu一** l 文件夹中 **新建一** 个名为 Cocoa Touch 类的文件 `SpinnerViewController` 。 在 **字段的子类中选择 UIViewController。** 
1. 打开 **SpinnerViewController.swift，** 并将其内容替换为以下代码。

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/SpinnerViewController.swift" id="SpinnerSnippet":::

## <a name="test-the-app"></a>测试应用程序

保存更改并启动应用。 你应该能够使用"登录"和"注销 **"按钮和** 选项卡栏在屏幕之间移动。

![应用程序的屏幕截图](images/app-screens.png)
