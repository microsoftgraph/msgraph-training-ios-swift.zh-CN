# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="29f49-101">如何运行已完成的项目</span><span class="sxs-lookup"><span data-stu-id="29f49-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29f49-102">先决条件</span><span class="sxs-lookup"><span data-stu-id="29f49-102">Prerequisites</span></span>

<span data-ttu-id="29f49-103">若要在此文件夹中运行已完成的项目，您需要以下各项：</span><span class="sxs-lookup"><span data-stu-id="29f49-103">To run the completed project in this folder, you need the following:</span></span>

- [<span data-ttu-id="29f49-104">Xcode</span><span class="sxs-lookup"><span data-stu-id="29f49-104">Xcode</span></span>](https://developer.apple.com/xcode/)
- [<span data-ttu-id="29f49-105">CocoaPods</span><span class="sxs-lookup"><span data-stu-id="29f49-105">CocoaPods</span></span>](https://cocoapods.org)
- <span data-ttu-id="29f49-106">使用 Outlook.com 上的邮箱的个人 Microsoft 帐户，或者是 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="29f49-106">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

> <span data-ttu-id="29f49-107">**注意：** 本教程是使用 Xcode 版本11.4 和 CocoaPods 版本1.9.1 编写的。</span><span class="sxs-lookup"><span data-stu-id="29f49-107">**Note:** This tutorial was written using Xcode version 11.4 and CocoaPods version 1.9.1.</span></span> <span data-ttu-id="29f49-108">本指南中的步骤可能适用于其他版本，但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="29f49-108">The steps in this guide may work with other versions, but that has not been tested.</span></span>

<span data-ttu-id="29f49-109">如果你没有 Microsoft 帐户，可以使用以下几种方法获取免费帐户：</span><span class="sxs-lookup"><span data-stu-id="29f49-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="29f49-110">你可以[注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="29f49-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="29f49-111">你可以[注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program)以获取免费的 office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="29f49-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="29f49-112">向 Azure Active Directory 管理中心注册 web 应用程序</span><span class="sxs-lookup"><span data-stu-id="29f49-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="29f49-113">打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="29f49-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="29f49-114">使用**个人帐户**（亦称为“Microsoft 帐户”）或**工作或学校帐户**登录。</span><span class="sxs-lookup"><span data-stu-id="29f49-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="29f49-115">选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。</span><span class="sxs-lookup"><span data-stu-id="29f49-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="29f49-116">应用注册的屏幕截图</span><span class="sxs-lookup"><span data-stu-id="29f49-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="29f49-117">选择“新注册”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="29f49-117">Select **New registration**.</span></span> <span data-ttu-id="29f49-118">在“注册应用”\*\*\*\* 页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="29f49-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="29f49-119">将“名称”\*\*\*\* 设置为“`iOS Swift Graph Tutorial`”。</span><span class="sxs-lookup"><span data-stu-id="29f49-119">Set **Name** to `iOS Swift Graph Tutorial`.</span></span>
    - <span data-ttu-id="29f49-120">将“受支持的帐户类型”\*\*\*\* 设置为“任何组织目录中的帐户和个人 Microsoft 帐户”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="29f49-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="29f49-121">保留“重定向 URI”\*\*\*\* 为空。</span><span class="sxs-lookup"><span data-stu-id="29f49-121">Leave **Redirect URI** empty.</span></span>

    !["注册应用程序" 页的屏幕截图](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="29f49-123">选择“注册”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="29f49-123">Choose **Register**.</span></span> <span data-ttu-id="29f49-124">在 " **IOS Swift Graph 教程**" 页上，复制**应用程序（客户端） ID**的值并保存它，下一步将需要它。</span><span class="sxs-lookup"><span data-stu-id="29f49-124">On the **iOS Swift Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 的屏幕截图](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="29f49-126">选择“管理”\*\*\*\* 下的“身份验证”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="29f49-126">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="29f49-127">选择 "**添加平台**"，然后选择 " **iOS/macOS**"。</span><span class="sxs-lookup"><span data-stu-id="29f49-127">Select **Add a platform**, then **iOS / macOS**.</span></span>

1. <span data-ttu-id="29f49-128">输入您的应用程序的捆绑包 ID 并选择 "**配置**"，然后选择 "**完成**"。</span><span class="sxs-lookup"><span data-stu-id="29f49-128">Enter your app's Bundle ID and select **Configure**, then select **Done**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="29f49-129">配置示例</span><span class="sxs-lookup"><span data-stu-id="29f49-129">Configure the sample</span></span>

1. <span data-ttu-id="29f49-130">将`AuthSettings.plist.example`文件重命名`AuthSettings.plist`为。</span><span class="sxs-lookup"><span data-stu-id="29f49-130">Rename the `AuthSettings.plist.example` file to `AuthSettings.plist`.</span></span>
1. <span data-ttu-id="29f49-131">编辑`AuthSettings.plist`文件并进行以下更改。</span><span class="sxs-lookup"><span data-stu-id="29f49-131">Edit the `AuthSettings.plist` file and make the following changes.</span></span>
    1. <span data-ttu-id="29f49-132">将`YOUR_APP_ID_HERE`替换为你从应用注册门户获取的**应用程序 Id** 。</span><span class="sxs-lookup"><span data-stu-id="29f49-132">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="29f49-133">在**GraphTutorial**目录中打开 "终端"，并运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="29f49-133">Open Terminal in the **GraphTutorial** directory and run the following command.</span></span>

    ```Shell
    pod install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="29f49-134">运行示例</span><span class="sxs-lookup"><span data-stu-id="29f49-134">Run the sample</span></span>

<span data-ttu-id="29f49-135">在 Xcode 中，选择 "**产品**" 菜单，然后选择 "**运行**"。</span><span class="sxs-lookup"><span data-stu-id="29f49-135">In Xcode, select the **Product** menu, then select **Run**.</span></span>
