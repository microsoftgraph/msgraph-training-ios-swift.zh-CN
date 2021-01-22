<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将 Microsoft Graph 合并到应用程序中。 对于此应用程序，你将使用 [适用于目标 C](https://github.com/microsoftgraph/msgraph-sdk-objc) 的 Microsoft Graph SDK 调用 Microsoft Graph。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

在此部分中，您将扩展类以添加一个函数，获取当前一周的用户事件，并 `GraphManager` 更新为 `CalendarViewController` 使用这些新函数。

1. 打开 **GraphManager.swift，** 将以下方法添加到 `GraphManager` 类中。

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
    > 考虑代码正在 `getCalendarView` 执行哪些工作。
    >
    > - 将调用的 URL 为 `/v1.0/me/calendarview` 。
    >   - 和 `startDateTime` `endDateTime` 查询参数定义日历视图的开始和结束。
    >   - 查询 `select` 参数将每个事件返回的字段限制为视图将实际使用的字段。
    >   - 查询 `orderby` 参数按开始时间对结果进行排序。
    >   - 查询 `top` 参数请求每页 25 个结果。
    >   - 标头使 Microsoft Graph 返回用户时区中每个事件的 `Prefer: outlook.timezone` 开始时间和结束时间。

1. 在 **GraphTu一****l** 项目中新建一个名为 **GraphToIana.swift 的 Swift 文件**。 将以下代码添加到文件中。

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/GraphToIana.swift" id="GraphToIanaSnippet":::

    这将进行简单的查找，以根据 Microsoft Graph 返回的时区名称查找 IANA 时区标识符。

1. 打开 **CalendarViewController.swift，** 并将其全部内容替换为以下代码。

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

现在，你可以运行应用、登录和点击菜单中的 **日历** 导航项。 你应该会看到应用中事件的 JSON 转储。

## <a name="display-the-results"></a>显示结果

现在，可以将 JSON 转储替换为某些内容，以用户友好的方式显示结果。 在此部分中，您将修改函数以返回强类型对象，并修改为使用表视图 `getCalendarView` `CalendarViewController` 呈现事件。

1. 打开 **GraphManager.swift**。 将现有的 `getCalendarView` 函数替换为以下内容。

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/GraphManager.swift" id="GetEventsSnippet" highlight="3,28-49":::

1. 在 **GraphTu一** l 项目中 **新建一** 个名为 Cocoa Touch 类的文件 `CalendarTableViewController.swift` 。 在 **字段的子类中选择 UITableViewController。** 

1. 打开 **CalendarTableViewController.swift，** 并将其内容替换为以下内容。

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewController.swift" id="CalendarTableViewControllerSnippet":::

1. 在 **GraphTu一** l 项目中 **新建一** 个名为 Cocoa Touch 类的文件 `CalendarTableViewCell.swift` 。 在字段的子类 **中选择** **UITableViewCell。**

1. 打开 **CalendarTableViewCell.swift，** 然后向该类添加以下 `CalendarTableViewCell` 属性。

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.swift" id="PropertiesSnippet":::

1. 打开 **Main.storyboard** 并找到 **日历场景**。 从根视图中删除滚动视图。
1. 使用 **库**，将 **导航栏** 添加到视图的顶部。
1. 双击导航 **栏中** 的标题，并更新为 `Calendar` 。
1. 使用 **库**，将 **栏按钮项** 添加到导航栏的右侧。
1. 选择新的栏按钮，然后选择 **属性检查器**。 将 **图像更改为****加**。
1. 将 **"库"中的** 容器 **视图** 添加到导航栏下的视图中。 调整容器视图的大小以占用视图中的所有剩余空间。
1. 设置导航栏和容器视图的约束，如下所示。

    - **导航栏**
        - 添加约束：高度，值：44
        - 添加约束：安全区域前导空格，值：0
        - 向安全区域添加约束：尾随空格，值：0
        - 添加约束：安全区域的顶部空间，值：0
    - **容器视图**
        - 添加约束：安全区域前导空格，值：0
        - 向安全区域添加约束：尾随空格，值：0
        - 添加约束：导航栏底部的顶部空间，值：0
        - 添加约束：安全区域的底部空间，值：0

1. 在添加容器视图时，找到添加到情节提要的第二个视图控制器。 它通过嵌入 segue **连接到日历** 场景。 选择此控制器并使用 **标识检查** 器将 **类** 更改为 **CalendarTableViewController。**
1. 从 **日历表** 视图 **控制器中删除视图**。
1. 将库 **的表** 视图 **添加到****日历表视图控制器**。
1. 选择表视图，然后选择 **属性检查器**。 将 **原型单元格设置为** **1。**
1. 拖动原型单元格的底部边缘，以为您提供一个更大的区域来使用。
1. 使用 **库向** 原型单元格 **添加** 三个标签。
1. 选择原型单元格，然后选择 **标识检查器**。 将 **类** 更改为 **CalendarTableViewCell**。
1. 选择 **属性检查器，** 将 **标识符设置为** `EventCell` 。
1. 选中 **EventCell** 后，选择 **连接** 检查器并连接 ，并连接到添加到情节提要上的单元格 `durationLabel` `organizerLabel` `subjectLabel` 的标签。
1. 按如下所示设置三个标签的属性和约束。

    - **主题标签**
        - 添加约束：将空格前导到内容视图前导边距，值：0
        - 添加约束：内容视图尾部边距的尾部空格，值：0
        - 添加约束：内容视图上边距的上边距空间，值：0
    - **组织者标签**
        - 字体：System 12.0
        - 添加约束：高度，值：15
        - 添加约束：将空格前导到内容视图前导边距，值：0
        - 添加约束：内容视图尾部边距的尾部空格，值：0
        - 添加约束：主题标签底部的顶部空间，值：标准
    - **持续时间标签**
        - 字体：System 12.0
        - 颜色：深灰色
        - 添加约束：高度，值：15
        - 添加约束：将空格前导到内容视图前导边距，值：0
        - 添加约束：内容视图尾部边距的尾部空格，值：0
        - 添加约束：将顶部空间添加到组织者标签底部，值：Standard
        - 添加约束：内容视图下边距的底部空间，值：0

1. 选择 **EventCell，** 然后选择 **大小检查器**。 启用 **自动** 行 **高**。

    ![日历和日历表视图控制器的屏幕截图](images/calendar-table-storyboard.png)

1. 打开 **CalendarViewController.swift，** 并将其内容替换为以下代码。

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarViewController.swift" id="CalendarViewSnippet":::

1. 运行应用、登录并点击 **"日历"** 选项卡。应看到事件列表。

    ![事件表的屏幕截图](images/calendar-list.png)
