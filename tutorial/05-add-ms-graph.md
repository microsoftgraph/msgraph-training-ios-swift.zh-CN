<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7d8cf-101">在此练习中，你将 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="7d8cf-102">对于此应用程序，你将使用 [适用于目标 C](https://github.com/microsoftgraph/msgraph-sdk-objc) 的 Microsoft Graph SDK 调用 Microsoft Graph。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-102">For this application, you will use the [Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="7d8cf-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="7d8cf-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="7d8cf-104">在此部分中，您将扩展类以添加一个函数，获取当前一周的用户事件，并 `GraphManager` 更新为 `CalendarViewController` 使用这些新函数。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-104">In this section you will extend the `GraphManager` class to add a function to get the user's events for the current week and update `CalendarViewController` to use these new functions.</span></span>

1. <span data-ttu-id="7d8cf-105">打开 **GraphManager.swift，** 将以下方法添加到 `GraphManager` 类中。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-105">Open **GraphManager.swift** and add the following method to the `GraphManager` class.</span></span>

    ```Swift
    public func getCalendarView(viewStart: String,
                                viewEnd: String,
                                completion: @escaping(Data?, Error?) -> Void) {
        // GET /me/calendarview
        // Set start and end of the view
        let start = "startDateTime=\(viewStart)"
        let end = "endDateTime=\(viewEnd)"
        // Only return these fields in results
        let select = "$select=subject,organizer,start,end"
        // Sort results by when they were created, newest first
        let orderBy = "$orderby=start/dateTime"
        // Request at most 25 results
        let top = "$top=25"

        let eventsRequest = NSMutableURLRequest(url: URL(string: "\(MSGraphBaseURL)/me/calendarview?\(start)&\(end)&\(select)&\(orderBy)&\(top)")!)

        // Add the Prefer: outlook.timezone header to get start and end times
        // in user's time zone
        eventsRequest.addValue("outlook.timezone=\"\(self.userTimeZone)\"", forHTTPHeaderField: "Prefer")

        let eventsDataTask = MSURLSessionDataTask(request: eventsRequest, client: self.client, completion: {
            (data: Data?, response: URLResponse?, graphError: Error?) in
            guard let eventsData = data, graphError == nil else {
                completion(nil, graphError)
                return
            }

            // TEMPORARY
            completion(eventsData, nil)
        })

        // Execute the request
        eventsDataTask?.execute()
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="7d8cf-106">考虑代码正在 `getCalendarView` 执行哪些工作。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-106">Consider what the code in `getCalendarView` is doing.</span></span>
    >
    > - <span data-ttu-id="7d8cf-107">将调用的 URL 为 `/v1.0/me/calendarview` 。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-107">The URL that will be called is `/v1.0/me/calendarview`.</span></span>
    >   - <span data-ttu-id="7d8cf-108">和 `startDateTime` `endDateTime` 查询参数定义日历视图的开始和结束。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-108">The `startDateTime` and `endDateTime` query parameters define the start and end of the calendar view.</span></span>
    >   - <span data-ttu-id="7d8cf-109">查询 `select` 参数将每个事件返回的字段限制为视图将实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-109">The `select` query parameter limits the fields returned for each events to just those the view will actually use.</span></span>
    >   - <span data-ttu-id="7d8cf-110">查询 `orderby` 参数按开始时间对结果进行排序。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-110">The `orderby` query parameter sorts the results by start time.</span></span>
    >   - <span data-ttu-id="7d8cf-111">查询 `top` 参数请求每页 25 个结果。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-111">The `top` query parameter requests 25 results per page.</span></span>
    >   - <span data-ttu-id="7d8cf-112">标头使 Microsoft Graph 返回用户时区中每个事件的 `Prefer: outlook.timezone` 开始时间和结束时间。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-112">the `Prefer: outlook.timezone` header causes the Microsoft Graph to return the start and end times of each event in the user's time zone.</span></span>

1. <span data-ttu-id="7d8cf-113">在 **GraphTu一\*\*\*\*l** 项目中新建一个名为 **GraphToIana.swift 的 Swift 文件**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-113">Create a new **Swift File** in the **GraphTutorial** project named **GraphToIana.swift**.</span></span> <span data-ttu-id="7d8cf-114">将以下代码添加到文件中。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-114">Add the following code to the file.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/GraphToIana.swift" id="GraphToIanaSnippet":::

    <span data-ttu-id="7d8cf-115">这将进行简单的查找，以根据 Microsoft Graph 返回的时区名称查找 IANA 时区标识符。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-115">This does a simple lookup to find an IANA time zone identifier based on the time zone name returned by Microsoft Graph.</span></span>

1. <span data-ttu-id="7d8cf-116">打开 **CalendarViewController.swift，** 并将其全部内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-116">Open **CalendarViewController.swift** and replace its entire contents with the following code.</span></span>

    ```Swift
    import UIKit
    import MSGraphClientModels

    class CalendarViewController: UIViewController {

        @IBOutlet var calendarJSON: UITextView!

        private let spinner = SpinnerViewController()

        override func viewDidLoad() {
            super.viewDidLoad()

            // Do any additional setup after loading the view.
            self.spinner.start(container: self)

            // Calculate the start and end of the current week
            let timeZone = GraphToIana.getIanaIdentifier(graphIdentifer: GraphManager.instance.userTimeZone)
            let now = Date()
            var calendar = Calendar(identifier: .gregorian)
            calendar.timeZone = TimeZone(identifier: timeZone)!

            let startOfWeek = calendar.dateComponents([.calendar, .yearForWeekOfYear, .weekOfYear], from: now).date!
            let endOfWeek = calendar.date(byAdding: .day, value: 7, to: startOfWeek)!

            // Convert start and end to ISO 8601 strings
            let isoFormatter = ISO8601DateFormatter()
            let viewStart = isoFormatter.string(from: startOfWeek)
            let viewEnd = isoFormatter.string(from: endOfWeek)

            GraphManager.instance.getCalendarView(viewStart: viewStart, viewEnd: viewEnd) {
                (data: Data?, error: Error?) in
                DispatchQueue.main.async {
                    self.spinner.stop()

                    // TEMPORARY
                    guard let eventsData = data, error == nil else {
                        self.calendarJSON.text = error.debugDescription
                        return
                    }

                    let jsonString = String(data: eventsData, encoding: .utf8)
                    self.calendarJSON.text = jsonString
                    self.calendarJSON.sizeToFit()
                }
            }
        }
    }
    ```

<span data-ttu-id="7d8cf-117">现在，你可以运行应用、登录和点击菜单中的 **日历** 导航项。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-117">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="7d8cf-118">你应该会看到应用中事件的 JSON 转储。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-118">You should see a JSON dump of the events in the app.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="7d8cf-119">显示结果</span><span class="sxs-lookup"><span data-stu-id="7d8cf-119">Display the results</span></span>

<span data-ttu-id="7d8cf-120">现在，可以将 JSON 转储替换为某些内容，以用户友好的方式显示结果。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-120">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="7d8cf-121">在此部分中，您将修改函数以返回强类型对象，并修改为使用表视图 `getCalendarView` `CalendarViewController` 呈现事件。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-121">In this section, you will modify the `getCalendarView` function to return strongly-typed objects, and modify `CalendarViewController` to use a table view to render the events.</span></span>

1. <span data-ttu-id="7d8cf-122">打开 **GraphManager.swift**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-122">Open **GraphManager.swift**.</span></span> <span data-ttu-id="7d8cf-123">将现有的 `getCalendarView` 函数替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-123">Replace the existing `getCalendarView` function with the following.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/GraphManager.swift" id="GetEventsSnippet" highlight="3,28-49":::

1. <span data-ttu-id="7d8cf-124">在 **GraphTu一** l 项目中 **新建一** 个名为 Cocoa Touch 类的文件 `CalendarTableViewController.swift` 。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-124">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewController.swift`.</span></span> <span data-ttu-id="7d8cf-125">在 **字段的子类中选择 UITableViewController。** </span><span class="sxs-lookup"><span data-stu-id="7d8cf-125">Choose **UITableViewController** in the **Subclass of** field.</span></span>

1. <span data-ttu-id="7d8cf-126">打开 **CalendarTableViewController.swift，** 并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-126">Open **CalendarTableViewController.swift** and replace its contents with the following.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewController.swift" id="CalendarTableViewControllerSnippet":::

1. <span data-ttu-id="7d8cf-127">在 **GraphTu一** l 项目中 **新建一** 个名为 Cocoa Touch 类的文件 `CalendarTableViewCell.swift` 。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-127">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewCell.swift`.</span></span> <span data-ttu-id="7d8cf-128">在字段的子类 **中选择** **UITableViewCell。**</span><span class="sxs-lookup"><span data-stu-id="7d8cf-128">Choose **UITableViewCell** in the **Subclass of** field.</span></span>

1. <span data-ttu-id="7d8cf-129">打开 **CalendarTableViewCell.swift，** 然后向该类添加以下 `CalendarTableViewCell` 属性。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-129">Open **CalendarTableViewCell.swift** and add the following properties to the `CalendarTableViewCell` class.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.swift" id="PropertiesSnippet":::

1. <span data-ttu-id="7d8cf-130">打开 **Main.storyboard** 并找到 **日历场景**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-130">Open **Main.storyboard** and locate the **Calendar Scene**.</span></span> <span data-ttu-id="7d8cf-131">从根视图中删除滚动视图。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-131">Delete the scroll view from the root view.</span></span>
1. <span data-ttu-id="7d8cf-132">使用 **库**，将 **导航栏** 添加到视图的顶部。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-132">Using the **Library**, add a **Navigation Bar** to the top of the view.</span></span>
1. <span data-ttu-id="7d8cf-133">双击导航 **栏中** 的标题，并更新为 `Calendar` 。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-133">Double-click the **Title** in the navigation bar and update it to `Calendar`.</span></span>
1. <span data-ttu-id="7d8cf-134">使用 **库**，将 **栏按钮项** 添加到导航栏的右侧。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-134">Using the **Library**, add a **Bar Button Item** to the right-hand side of the navigation bar.</span></span>
1. <span data-ttu-id="7d8cf-135">选择新的栏按钮，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-135">Select the new bar button, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="7d8cf-136">将 **图像更改为\*\*\*\*加**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-136">Change **Image** to **plus**.</span></span>
1. <span data-ttu-id="7d8cf-137">将 **"库"中的** 容器 **视图** 添加到导航栏下的视图中。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-137">Add a **Container View** from the **Library** to the view under the navigation bar.</span></span> <span data-ttu-id="7d8cf-138">调整容器视图的大小以占用视图中的所有剩余空间。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-138">Resize the container view to take all of the remaining space in the view.</span></span>
1. <span data-ttu-id="7d8cf-139">设置导航栏和容器视图的约束，如下所示。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-139">Set constraints on the navigation bar and container view as follows.</span></span>

    - <span data-ttu-id="7d8cf-140">**导航栏**</span><span class="sxs-lookup"><span data-stu-id="7d8cf-140">**Navigation Bar**</span></span>
        - <span data-ttu-id="7d8cf-141">添加约束：高度，值：44</span><span class="sxs-lookup"><span data-stu-id="7d8cf-141">Add constraint: Height, value: 44</span></span>
        - <span data-ttu-id="7d8cf-142">添加约束：安全区域前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-142">Add constraint: Leading space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="7d8cf-143">向安全区域添加约束：尾随空格，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-143">Add constraint: Trailing space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="7d8cf-144">添加约束：安全区域的顶部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-144">Add constraint: Top space to Safe Area, value: 0</span></span>
    - <span data-ttu-id="7d8cf-145">**容器视图**</span><span class="sxs-lookup"><span data-stu-id="7d8cf-145">**Container View**</span></span>
        - <span data-ttu-id="7d8cf-146">添加约束：安全区域前导空格，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-146">Add constraint: Leading space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="7d8cf-147">向安全区域添加约束：尾随空格，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-147">Add constraint: Trailing space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="7d8cf-148">添加约束：导航栏底部的顶部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-148">Add constraint: Top space to Navigation Bar Bottom, value: 0</span></span>
        - <span data-ttu-id="7d8cf-149">添加约束：安全区域的底部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-149">Add constraint: Bottom space to Safe Area, value: 0</span></span>

1. <span data-ttu-id="7d8cf-150">在添加容器视图时，找到添加到情节提要的第二个视图控制器。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-150">Locate the second view controller added to the storyboard when you added the container view.</span></span> <span data-ttu-id="7d8cf-151">它通过嵌入 segue **连接到日历** 场景。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-151">It is connected to the **Calendar Scene** by an embed segue.</span></span> <span data-ttu-id="7d8cf-152">选择此控制器并使用 **标识检查** 器将 **类** 更改为 **CalendarTableViewController。**</span><span class="sxs-lookup"><span data-stu-id="7d8cf-152">Select this controller and use the **Identity Inspector** to change **Class** to **CalendarTableViewController**.</span></span>
1. <span data-ttu-id="7d8cf-153">从 **日历表** 视图 **控制器中删除视图**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-153">Delete the **View** from the **Calendar Table View Controller**.</span></span>
1. <span data-ttu-id="7d8cf-154">将库 **的表** 视图 **添加到\*\*\*\*日历表视图控制器**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-154">Add a **Table View** from the **Library** to the **Calendar Table View Controller**.</span></span>
1. <span data-ttu-id="7d8cf-155">选择表视图，然后选择 **属性检查器**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-155">Select the table view, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="7d8cf-156">将 **原型单元格设置为** **1。**</span><span class="sxs-lookup"><span data-stu-id="7d8cf-156">Set **Prototype Cells** to **1**.</span></span>
1. <span data-ttu-id="7d8cf-157">拖动原型单元格的底部边缘，以为您提供一个更大的区域来使用。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-157">Drag the bottom edge of the prototype cell to give you a larger area to work with.</span></span>
1. <span data-ttu-id="7d8cf-158">使用 **库向** 原型单元格 **添加** 三个标签。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-158">Use the **Library** to add three **Labels** to the prototype cell.</span></span>
1. <span data-ttu-id="7d8cf-159">选择原型单元格，然后选择 **标识检查器**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-159">Select the prototype cell, then select the **Identity Inspector**.</span></span> <span data-ttu-id="7d8cf-160">将 **类** 更改为 **CalendarTableViewCell**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-160">Change **Class** to **CalendarTableViewCell**.</span></span>
1. <span data-ttu-id="7d8cf-161">选择 **属性检查器，** 将 **标识符设置为** `EventCell` 。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-161">Select the **Attributes Inspector** and set **Identifier** to `EventCell`.</span></span>
1. <span data-ttu-id="7d8cf-162">选中 **EventCell** 后，选择 **连接** 检查器并连接 ，并连接到添加到情节提要上的单元格 `durationLabel` `organizerLabel` `subjectLabel` 的标签。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-162">With the **EventCell** selected, select the **Connections Inspector** and connect `durationLabel`, `organizerLabel`, and `subjectLabel` to the labels you added to the cell on the storyboard.</span></span>
1. <span data-ttu-id="7d8cf-163">按如下所示设置三个标签的属性和约束。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-163">Set the properties and constraints on the three labels as follows.</span></span>

    - <span data-ttu-id="7d8cf-164">**主题标签**</span><span class="sxs-lookup"><span data-stu-id="7d8cf-164">**Subject Label**</span></span>
        - <span data-ttu-id="7d8cf-165">添加约束：将空格前导到内容视图前导边距，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-165">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="7d8cf-166">添加约束：内容视图尾部边距的尾部空格，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-166">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="7d8cf-167">添加约束：内容视图上边距的上边距空间，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-167">Add constraint: Top space to Content View Top Margin, value: 0</span></span>
    - <span data-ttu-id="7d8cf-168">**组织者标签**</span><span class="sxs-lookup"><span data-stu-id="7d8cf-168">**Organizer Label**</span></span>
        - <span data-ttu-id="7d8cf-169">字体：System 12.0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-169">Font: System 12.0</span></span>
        - <span data-ttu-id="7d8cf-170">添加约束：高度，值：15</span><span class="sxs-lookup"><span data-stu-id="7d8cf-170">Add constraint: Height, value: 15</span></span>
        - <span data-ttu-id="7d8cf-171">添加约束：将空格前导到内容视图前导边距，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-171">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="7d8cf-172">添加约束：内容视图尾部边距的尾部空格，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-172">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="7d8cf-173">添加约束：主题标签底部的顶部空间，值：标准</span><span class="sxs-lookup"><span data-stu-id="7d8cf-173">Add constraint: Top space to Subject Label Bottom, value: Standard</span></span>
    - <span data-ttu-id="7d8cf-174">**持续时间标签**</span><span class="sxs-lookup"><span data-stu-id="7d8cf-174">**Duration Label**</span></span>
        - <span data-ttu-id="7d8cf-175">字体：System 12.0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-175">Font: System 12.0</span></span>
        - <span data-ttu-id="7d8cf-176">颜色：深灰色</span><span class="sxs-lookup"><span data-stu-id="7d8cf-176">Color: Dark Gray Color</span></span>
        - <span data-ttu-id="7d8cf-177">添加约束：高度，值：15</span><span class="sxs-lookup"><span data-stu-id="7d8cf-177">Add constraint: Height, value: 15</span></span>
        - <span data-ttu-id="7d8cf-178">添加约束：将空格前导到内容视图前导边距，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-178">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="7d8cf-179">添加约束：内容视图尾部边距的尾部空格，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-179">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="7d8cf-180">添加约束：将顶部空间添加到组织者标签底部，值：Standard</span><span class="sxs-lookup"><span data-stu-id="7d8cf-180">Add constraint: Top space to Organizer Label Bottom, value: Standard</span></span>
        - <span data-ttu-id="7d8cf-181">添加约束：内容视图下边距的底部空间，值：0</span><span class="sxs-lookup"><span data-stu-id="7d8cf-181">Add constraint: Bottom space to Content View Bottom Margin, value: 0</span></span>

1. <span data-ttu-id="7d8cf-182">选择 **EventCell，** 然后选择 **大小检查器**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-182">Select the **EventCell**, then select the **Size Inspector**.</span></span> <span data-ttu-id="7d8cf-183">启用 **自动** 行 **高**。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-183">Enable **Automatic** for **Row Height**.</span></span>

    ![日历和日历表视图控制器的屏幕截图](images/calendar-table-storyboard.png)

1. <span data-ttu-id="7d8cf-185">打开 **CalendarViewController.swift，** 并将其内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-185">Open **CalendarViewController.swift** and replace its contents with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarViewController.swift" id="CalendarViewSnippet":::

1. <span data-ttu-id="7d8cf-186">运行应用、登录并点击 **"日历"** 选项卡。应看到事件列表。</span><span class="sxs-lookup"><span data-stu-id="7d8cf-186">Run the app, sign in, and tap the **Calendar** tab. You should see the list of events.</span></span>

    ![事件表的屏幕截图](images/calendar-list.png)
