# <a name="contributing-to-microsoft-graph-training-repositories"></a>为 Microsoft Graph 培训存储库做贡献

感谢你为此项目做贡献！ 在提交拉取请求之前，请务必考虑以下事项。

## <a name="overview"></a>概述

此存储库中的代码有三个用途：

- 教程文件夹中的 Markdown [文件在](/tutorial) Microsoft Graph 教程 [页面上](https://docs.microsoft.com/graph/tutorials) 作为教程发布。
- 演示 [文件夹中的示例项目](/demo) 是 Microsoft Graph 快速入门的 [源](https://developer.microsoft.com/graph/quick-start).* *\** _
- 演示文件夹中的示例项目也可直接从 GitHub 下载，并且应在一些简单配置后如期运行。

> _*\**_ 并非所有培训存储库都作为快速入门 (尚未) 。

请记住这一点很重要，因为一处更改_may* 需要更改另一处，以保持内容同步。只要有可能，Markdown 文件就会直接引用 (语法) ，以便更新源中的代码将自动更新 `:::code` Markdown 中的代码。

## <a name="updating-code"></a>更新代码

`:::code`Markdown 中使用的语法取决于源代码文件中的特定注释。 这些注释如下所示：

```csharp
// <MySnippet>
Console.WriteLine("Hello World!");
// </MySnippet>
```

如果在这些"标记"注释之间更新代码，Markdown 文件将在发布到 Microsoft Graph 文档网站时自动获取这些更改。 如果在这些注释之外更新代码，则很可能需要更新相应的 Markdown。

## <a name="adding-features"></a>添加功能

尽管这种欢迎是一种欢迎，但请不要发送拉取请求来向示例中添加新功能。 由于此存储库主要是"生成你的第一个应用"教程，因此功能集受设计限制。

## <a name="submitting-pull-requests"></a>提交拉取请求

请将所有拉取请求提交到 `master` 分支。

## <a name="when-do-changes-get-published"></a>何时发布更改？

向 [Microsoft Graph 教程网站发布](https://docs.microsoft.com/graph/tutorials) 更新不是自动的。 必须先将更改提升至分支，然后网站管理员必须 `live` 触发生成。 这通常以"根据需要"为基础完成。

## <a name="code-of-conduct"></a>行为准则

此项目采用 [Microsoft 开源行为准则](https://opensource.microsoft.com/codeofconduct/)。有关详细信息，请参阅 [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/)（行为准则常见问题解答），有任何其他问题或意见，也可联系 [opencode@microsoft.com](mailto:opencode@microsoft.com)。
