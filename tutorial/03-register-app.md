<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将使用 Azure Active Directory 管理中心创建新的 Azure AD 本机应用程序。

1. 打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。然后，使用 **个人帐户**（亦称为“Microsoft 帐户”）或 **工作或学校帐户** 登录。

1. 选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。

    ![应用注册的屏幕截图 ](images/aad-portal-app-registrations.png)

1. 选择“新注册”。 在“注册应用”页上，按如下方式设置值。

    - 将“名称”设置为“`iOS Swift Graph Tutorial`”。
    - 将“受支持的帐户类型”设置为“任何组织目录中的帐户和个人 Microsoft 帐户”。
    - 保留“重定向 URI”为空。

    !["注册应用程序"页的屏幕截图](images/aad-register-an-app.png)

1. 选择“**注册**”。 在 **iOS Swift Graph** 教程页面上，复制应用程序 (客户端) **ID** 并保存它，下一步中将需要它。

    ![新应用注册的应用程序 ID 屏幕截图](images/aad-application-id.png)

1. 选择“管理”下的“身份验证”。 选择 **"添加平台"，** 然后选择 **"iOS/macOS"。**

1. 输入应用的捆绑包 ID，**然后选择"** 配置"，然后选择"**完成"。**
