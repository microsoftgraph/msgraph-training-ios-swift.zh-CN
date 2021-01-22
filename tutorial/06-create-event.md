<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="edcc5-101">在此部分中，您将添加在用户日历上创建事件的能力。</span><span class="sxs-lookup"><span data-stu-id="edcc5-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="edcc5-102">打开 **GraphManager.swift** 并添加以下函数以在用户日历上创建新事件。</span><span class="sxs-lookup"><span data-stu-id="edcc5-102">Open **GraphManager.swift** and add the following function to create a new event on the user's calendar.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/GraphManager.swift" id="CreateEventSnippet":::

1. <span data-ttu-id="edcc5-103">在 **GraphTu一** l 文件夹中 **新建一** 个名为 Cocoa Touch 类的文件 `NewEventViewController` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-103">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `NewEventViewController`.</span></span> <span data-ttu-id="edcc5-104">在 **字段的子类中选择 UIViewController。** </span><span class="sxs-lookup"><span data-stu-id="edcc5-104">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="edcc5-105">打开 **NewEventViewController.swift，** 并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="edcc5-105">Open **NewEventViewController.swift** and replace its contents with the following.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/NewEventViewController.swift" id="NewEventViewControllerSnippet":::

1. <span data-ttu-id="edcc5-106">打开 **Main.storyboard**。</span><span class="sxs-lookup"><span data-stu-id="edcc5-106">Open **Main.storyboard**.</span></span> <span data-ttu-id="edcc5-107">使用 **库** 将视图 **控制器拖动** 到情节提要上。</span><span class="sxs-lookup"><span data-stu-id="edcc5-107">Use the **Library** to drag a **View Controller** onto the storyboard.</span></span>
1. <span data-ttu-id="edcc5-108">使用 **库**，将 **导航栏** 添加到视图控制器。</span><span class="sxs-lookup"><span data-stu-id="edcc5-108">Using the **Library**, add a **Navigation Bar** to the view controller.</span></span>
1. <span data-ttu-id="edcc5-109">双击导航 **栏中** 的标题，并更新为 `New Event` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-109">Double-click the **Title** in the navigation bar and update it to `New Event`.</span></span>
1. <span data-ttu-id="edcc5-110">使用 **库**，将 **栏按钮项** 添加到导航栏的左侧。</span><span class="sxs-lookup"><span data-stu-id="edcc5-110">Using the **Library**, add a **Bar Button Item** to the left-hand side of the navigation bar.</span></span>
1. <span data-ttu-id="edcc5-111">选择新的栏按钮，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="edcc5-111">Select the new bar button, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="edcc5-112">将 **标题更改为** `Cancel` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-112">Change **Title** to `Cancel`.</span></span>
1. <span data-ttu-id="edcc5-113">使用 **库**，将 **栏按钮项** 添加到导航栏的右侧。</span><span class="sxs-lookup"><span data-stu-id="edcc5-113">Using the **Library**, add a **Bar Button Item** to the right-hand side of the navigation bar.</span></span>
1. <span data-ttu-id="edcc5-114">选择新的栏按钮，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="edcc5-114">Select the new bar button, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="edcc5-115">将 **标题** 更改为 `Create` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-115">Change **Title** to `Create`.</span></span>
1. <span data-ttu-id="edcc5-116">选择视图控制器，然后选择 **标识检查器**。</span><span class="sxs-lookup"><span data-stu-id="edcc5-116">Select the view controller, then select the **Identity Inspector**.</span></span> <span data-ttu-id="edcc5-117">将 **类** 更改为 **NewEventViewController**。</span><span class="sxs-lookup"><span data-stu-id="edcc5-117">Change **Class** to **NewEventViewController**.</span></span>
1. <span data-ttu-id="edcc5-118">将以下控件从 **库** 添加到视图中。</span><span class="sxs-lookup"><span data-stu-id="edcc5-118">Add the following controls from the **Library** to the view.</span></span>

    - <span data-ttu-id="edcc5-119">在导航 **栏** 下添加标签。</span><span class="sxs-lookup"><span data-stu-id="edcc5-119">Add a **Label** under the navigation bar.</span></span> <span data-ttu-id="edcc5-120">将文本设置为 `Subject` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-120">Set its text to `Subject`.</span></span>
    - <span data-ttu-id="edcc5-121">在标签 **下添加** 文本字段。</span><span class="sxs-lookup"><span data-stu-id="edcc5-121">Add a **Text Field** under the label.</span></span> <span data-ttu-id="edcc5-122">将占位符 **属性** 设置为 `Subject` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-122">Set its **Placeholder** attribute to `Subject`.</span></span>
    - <span data-ttu-id="edcc5-123">在文本 **字段** 下添加标签。</span><span class="sxs-lookup"><span data-stu-id="edcc5-123">Add a **Label** under the text field.</span></span> <span data-ttu-id="edcc5-124">将文本设置为 `Attendees` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-124">Set its text to `Attendees`.</span></span>
    - <span data-ttu-id="edcc5-125">在标签 **下添加** 文本字段。</span><span class="sxs-lookup"><span data-stu-id="edcc5-125">Add a **Text Field** under the label.</span></span> <span data-ttu-id="edcc5-126">将占位符 **属性** 设置为 `Separate multiple entries with ;` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-126">Set its **Placeholder** attribute to `Separate multiple entries with ;`.</span></span>
    - <span data-ttu-id="edcc5-127">在文本 **字段** 下添加标签。</span><span class="sxs-lookup"><span data-stu-id="edcc5-127">Add a **Label** under the text field.</span></span> <span data-ttu-id="edcc5-128">将文本设置为 `Start` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-128">Set its text to `Start`.</span></span>
    - <span data-ttu-id="edcc5-129">在标签 **下添加** 日期选取器。</span><span class="sxs-lookup"><span data-stu-id="edcc5-129">Add a **Date Picker** under the label.</span></span> <span data-ttu-id="edcc5-130">将首选 **样式** 设置为 **"压缩**"，**将间隔** 设置为 **15 分钟**，将高度设置为 **35。**</span><span class="sxs-lookup"><span data-stu-id="edcc5-130">Set its **Preferred Style** to **Compact**, its **Interval** to **15 minutes**, and its height to **35**.</span></span>
    - <span data-ttu-id="edcc5-131">在日期 **选取** 器下添加标签。</span><span class="sxs-lookup"><span data-stu-id="edcc5-131">Add a **Label** under the date picker.</span></span> <span data-ttu-id="edcc5-132">将文本设置为 `End` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-132">Set its text to `End`.</span></span>
    - <span data-ttu-id="edcc5-133">在标签 **下添加** 日期选取器。</span><span class="sxs-lookup"><span data-stu-id="edcc5-133">Add a **Date Picker** under the label.</span></span> <span data-ttu-id="edcc5-134">将首选 **样式** 设置为 **"压缩**"，**将间隔** 设置为 **15 分钟**，将高度设置为 **35。**</span><span class="sxs-lookup"><span data-stu-id="edcc5-134">Set its **Preferred Style** to **Compact**, its **Interval** to **15 minutes**, and its height to **35**.</span></span>
    - <span data-ttu-id="edcc5-135">在日期 **选取器** 下添加文本视图。</span><span class="sxs-lookup"><span data-stu-id="edcc5-135">Add a **Text View** under the date picker.</span></span>

1. <span data-ttu-id="edcc5-136">选择 **"新建事件视图控制器** "，并使用 **连接检查** 器建立以下连接。</span><span class="sxs-lookup"><span data-stu-id="edcc5-136">Select the **New Event View Controller** and use the **Connection Inspector** to make the following connections.</span></span>

    - <span data-ttu-id="edcc5-137">将 **取消** 接收的操作连接到"取消栏 **"** 按钮。</span><span class="sxs-lookup"><span data-stu-id="edcc5-137">Connect the **cancel** received action to the **Cancel** bar button.</span></span>
    - <span data-ttu-id="edcc5-138">将 **createEvent** 接收的操作连接到 **"创建栏"** 按钮。</span><span class="sxs-lookup"><span data-stu-id="edcc5-138">Connect the **createEvent** received action to the **Create** bar button.</span></span>
    - <span data-ttu-id="edcc5-139">将 **主题出口** 连接到第一个文本字段。</span><span class="sxs-lookup"><span data-stu-id="edcc5-139">Connect the **subject** outlet to the first text field.</span></span>
    - <span data-ttu-id="edcc5-140">将 **与会者出口** 连接到第二个文本字段。</span><span class="sxs-lookup"><span data-stu-id="edcc5-140">Connect the **attendees** outlet to the second text field.</span></span>
    - <span data-ttu-id="edcc5-141">将 **开始出口** 连接到第一个日期选取器。</span><span class="sxs-lookup"><span data-stu-id="edcc5-141">Connect the **start** outlet to the first date picker.</span></span>
    - <span data-ttu-id="edcc5-142">将 **结束出口** 连接到第二个日期选取器。</span><span class="sxs-lookup"><span data-stu-id="edcc5-142">Connect the **end** outlet to the second date picker.</span></span>
    - <span data-ttu-id="edcc5-143">将 **正文出口** 连接到文本视图。</span><span class="sxs-lookup"><span data-stu-id="edcc5-143">Connect the **body** outlet to the text view.</span></span>

1. <span data-ttu-id="edcc5-144">添加以下约束。</span><span class="sxs-lookup"><span data-stu-id="edcc5-144">Add the following constraints.</span></span>

    - <span data-ttu-id="edcc5-145">**导航栏**</span><span class="sxs-lookup"><span data-stu-id="edcc5-145">**Navigation Bar**</span></span>
        - <span data-ttu-id="edcc5-146">安全区域前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-146">Leading space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="edcc5-147">安全区域尾部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-147">Trailing space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="edcc5-148">安全区域的顶部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-148">Top space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="edcc5-149">高度，值：44</span><span class="sxs-lookup"><span data-stu-id="edcc5-149">Height, value: 44</span></span>
    - <span data-ttu-id="edcc5-150">**主题标签**</span><span class="sxs-lookup"><span data-stu-id="edcc5-150">**Subject Label**</span></span>
        - <span data-ttu-id="edcc5-151">视图边距前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-151">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-152">查看边距的尾部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-152">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-153">导航栏的顶部空间，值：20</span><span class="sxs-lookup"><span data-stu-id="edcc5-153">Top space to Navigation Bar, value: 20</span></span>
    - <span data-ttu-id="edcc5-154">**主题文本字段**</span><span class="sxs-lookup"><span data-stu-id="edcc5-154">**Subject Text Field**</span></span>
        - <span data-ttu-id="edcc5-155">视图边距前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-155">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-156">查看边距的尾部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-156">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-157">主题标签的上空间，值：Standard</span><span class="sxs-lookup"><span data-stu-id="edcc5-157">Top space to Subject Label, value: Standard</span></span>
    - <span data-ttu-id="edcc5-158">**与会者标签**</span><span class="sxs-lookup"><span data-stu-id="edcc5-158">**Attendees Label**</span></span>
        - <span data-ttu-id="edcc5-159">视图边距前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-159">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-160">查看边距的尾部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-160">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-161">主题文本字段的上空间，值：Standard</span><span class="sxs-lookup"><span data-stu-id="edcc5-161">Top space to Subject Text Field, value: Standard</span></span>
    - <span data-ttu-id="edcc5-162">**与会者文本字段**</span><span class="sxs-lookup"><span data-stu-id="edcc5-162">**Attendees Text Field**</span></span>
        - <span data-ttu-id="edcc5-163">视图边距前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-163">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-164">查看边距的尾部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-164">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-165">与会者标签的上空间，值：Standard</span><span class="sxs-lookup"><span data-stu-id="edcc5-165">Top space to Attendees Label, value: Standard</span></span>
    - <span data-ttu-id="edcc5-166">**开始标签**</span><span class="sxs-lookup"><span data-stu-id="edcc5-166">**Start Label**</span></span>
        - <span data-ttu-id="edcc5-167">视图边距前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-167">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-168">查看边距的尾部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-168">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-169">主题文本字段的上空间，值：Standard</span><span class="sxs-lookup"><span data-stu-id="edcc5-169">Top space to Subject Text Field, value: Standard</span></span>
    - <span data-ttu-id="edcc5-170">**开始日期选取器**</span><span class="sxs-lookup"><span data-stu-id="edcc5-170">**Start Date Picker**</span></span>
        - <span data-ttu-id="edcc5-171">视图边距前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-171">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-172">查看边距的尾部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-172">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-173">与会者标签的上空间，值：Standard</span><span class="sxs-lookup"><span data-stu-id="edcc5-173">Top space to Attendees Label, value: Standard</span></span>
        - <span data-ttu-id="edcc5-174">高度，值：35</span><span class="sxs-lookup"><span data-stu-id="edcc5-174">Height, value: 35</span></span>
    - <span data-ttu-id="edcc5-175">**结束标签**</span><span class="sxs-lookup"><span data-stu-id="edcc5-175">**End Label**</span></span>
        - <span data-ttu-id="edcc5-176">视图边距前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-176">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-177">查看边距的尾部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-177">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-178">开始日期选取器的顶部空间，值：Standard</span><span class="sxs-lookup"><span data-stu-id="edcc5-178">Top space to Start Date Picker, value: Standard</span></span>
    - <span data-ttu-id="edcc5-179">**结束日期选取器**</span><span class="sxs-lookup"><span data-stu-id="edcc5-179">**End Date Picker**</span></span>
        - <span data-ttu-id="edcc5-180">视图边距前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-180">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-181">查看边距的尾部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-181">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-182">结束标签的上空间，值：Standard</span><span class="sxs-lookup"><span data-stu-id="edcc5-182">Top space to End Label, value: Standard</span></span>
        - <span data-ttu-id="edcc5-183">高度：35</span><span class="sxs-lookup"><span data-stu-id="edcc5-183">Height: 35</span></span>
    - <span data-ttu-id="edcc5-184">**正文文本视图**</span><span class="sxs-lookup"><span data-stu-id="edcc5-184">**Body Text View**</span></span>
        - <span data-ttu-id="edcc5-185">视图边距前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-185">Leading space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-186">查看边距的尾部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-186">Trailing space to View margin, value: 0</span></span>
        - <span data-ttu-id="edcc5-187">结束日期选取器的顶部空间，值：Standard</span><span class="sxs-lookup"><span data-stu-id="edcc5-187">Top space to End Date Picker, value: Standard</span></span>
        - <span data-ttu-id="edcc5-188">查看边距的底部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="edcc5-188">Bottom space to View margin, value: 0</span></span>

    ![情节提要上新事件表单的屏幕截图](images/new-event-form.png)

1. <span data-ttu-id="edcc5-190">选择 **日历场景**，然后选择连接 **检查器**。</span><span class="sxs-lookup"><span data-stu-id="edcc5-190">Select the **Calendar Scene**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="edcc5-191">在 **"触发的 Segues"** 下，将手动旁的未填充圆圈拖到情节提要上的"新建事件 **视图** 控制器"上。</span><span class="sxs-lookup"><span data-stu-id="edcc5-191">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **New Event View Controller** on the storyboard.</span></span> <span data-ttu-id="edcc5-192">在 **弹出菜单中** 选择"以模式方式显示"。</span><span class="sxs-lookup"><span data-stu-id="edcc5-192">Select **Present Modally** in the pop-up menu.</span></span>
1. <span data-ttu-id="edcc5-193">选择刚添加的 segue，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="edcc5-193">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="edcc5-194">将 **Identifier 字段** 设置为 `showEventForm` 。</span><span class="sxs-lookup"><span data-stu-id="edcc5-194">Set the **Identifier** field to `showEventForm`.</span></span>
1. <span data-ttu-id="edcc5-195">将 **showNewEventForm** 接收的操作连接到 **+** 导航栏按钮。</span><span class="sxs-lookup"><span data-stu-id="edcc5-195">Connect the **showNewEventForm** received action to the **+** navigation bar button.</span></span>
1. <span data-ttu-id="edcc5-196">保存更改并重新启动该应用。</span><span class="sxs-lookup"><span data-stu-id="edcc5-196">Save your changes and restart the app.</span></span> <span data-ttu-id="edcc5-197">转到日历页面，然后点击 **+** 该按钮。</span><span class="sxs-lookup"><span data-stu-id="edcc5-197">Go to the calendar page and tap the **+** button.</span></span> <span data-ttu-id="edcc5-198">填写表单并点击 **"创建** "创建新事件。</span><span class="sxs-lookup"><span data-stu-id="edcc5-198">Fill in the form and tap **Create** to create a new event.</span></span>

    ![新事件表单的屏幕截图](images/create-event.png)
