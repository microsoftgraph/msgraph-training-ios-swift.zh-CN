<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1d570-101">在此练习中，你将使用 Azure Active Directory 管理中心创建新的 Azure AD 本机应用程序。</span><span class="sxs-lookup"><span data-stu-id="1d570-101">In this exercise you will create a new Azure AD native application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="1d570-102">打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。然后，使用 **个人帐户**（亦称为“Microsoft 帐户”）或 **工作或学校帐户** 登录。</span><span class="sxs-lookup"><span data-stu-id="1d570-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="1d570-103">选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。</span><span class="sxs-lookup"><span data-stu-id="1d570-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="1d570-104">应用注册的屏幕截图</span><span class="sxs-lookup"><span data-stu-id="1d570-104">A screenshot of the App registrations</span></span> ](images/aad-portal-app-registrations.png)

1. <span data-ttu-id="1d570-105">选择“新注册”。</span><span class="sxs-lookup"><span data-stu-id="1d570-105">Select **New registration**.</span></span> <span data-ttu-id="1d570-106">在“注册应用”页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="1d570-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="1d570-107">将“名称”设置为“`iOS Swift Graph Tutorial`”。</span><span class="sxs-lookup"><span data-stu-id="1d570-107">Set **Name** to `iOS Swift Graph Tutorial`.</span></span>
    - <span data-ttu-id="1d570-108">将“受支持的帐户类型”设置为“任何组织目录中的帐户和个人 Microsoft 帐户”。</span><span class="sxs-lookup"><span data-stu-id="1d570-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="1d570-109">保留“重定向 URI”为空。</span><span class="sxs-lookup"><span data-stu-id="1d570-109">Leave **Redirect URI** empty.</span></span>

    !["注册应用程序"页的屏幕截图](images/aad-register-an-app.png)

1. <span data-ttu-id="1d570-111">选择“**注册**”。</span><span class="sxs-lookup"><span data-stu-id="1d570-111">Select **Register**.</span></span> <span data-ttu-id="1d570-112">在 **iOS Swift Graph** 教程页面上，复制应用程序 (客户端) **ID** 并保存它，下一步中将需要它。</span><span class="sxs-lookup"><span data-stu-id="1d570-112">On the **iOS Swift Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 屏幕截图](images/aad-application-id.png)

1. <span data-ttu-id="1d570-114">选择“管理”下的“身份验证”。</span><span class="sxs-lookup"><span data-stu-id="1d570-114">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="1d570-115">选择 **"添加平台"，** 然后选择 **"iOS/macOS"。**</span><span class="sxs-lookup"><span data-stu-id="1d570-115">Select **Add a platform**, then **iOS / macOS**.</span></span>

1. <span data-ttu-id="1d570-116">输入应用的捆绑包 ID，**然后选择"** 配置"，然后选择"**完成"。**</span><span class="sxs-lookup"><span data-stu-id="1d570-116">Enter your app's Bundle ID and select **Configure**, then select **Done**.</span></span>
