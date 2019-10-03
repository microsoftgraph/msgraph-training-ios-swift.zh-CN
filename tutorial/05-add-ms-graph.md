<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5c4fc-101">在本练习中，将把 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="5c4fc-102">对于此应用程序，您将使用[Microsoft GRAPH SDK For 目标 C](https://github.com/microsoftgraph/msgraph-sdk-objc)以调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-102">For this application, you will use the [Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="5c4fc-103">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="5c4fc-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="5c4fc-104">在本节中，您将扩展`GraphManager`类以添加一个函数，以获取用户的事件并更新`CalendarViewController`以使用这些新函数。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-104">In this section you will extend the `GraphManager` class to add a function to get the user's events and update `CalendarViewController` to use these new functions.</span></span>

1. <span data-ttu-id="5c4fc-105">打开**GraphManager** ，并将以下方法添加到`GraphManager`类中。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-105">Open **GraphManager.swift** and add the following method to the `GraphManager` class.</span></span>

    ```Swift
    public func getEvents(completion: @escaping(Data?, Error?) -> Void) {
        // GET /me/events?$select='subject,organizer,start,end'$orderby=createdDateTime DESC
        // Only return these fields in results
        let select = "$select=subject,organizer,start,end"
        // Sort results by when they were created, newest first
        let orderBy = "$orderby=createdDateTime+DESC"
        let eventsRequest = NSMutableURLRequest(url: URL(string: "\(MSGraphBaseURL)/me/events?\(select)&\(orderBy)")!)
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
    > <span data-ttu-id="5c4fc-106">考虑中`getEvents`的代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-106">Consider what the code in `getEvents` is doing.</span></span>
    >
    > - <span data-ttu-id="5c4fc-107">将调用的 URL 为`/v1.0/me/events`。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-107">The URL that will be called is `/v1.0/me/events`.</span></span>
    > - <span data-ttu-id="5c4fc-108">`select`查询参数将为每个事件返回的字段限制为仅应用程序实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-108">The `select` query parameter limits the fields returned for each events to just those the app will actually use.</span></span>
    > - <span data-ttu-id="5c4fc-109">`orderby`查询参数按其创建日期和时间对结果进行排序，最新项目最先开始。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-109">The `orderby` query parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="5c4fc-110">打开**CalendarViewController** ，并将其全部内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-110">Open **CalendarViewController.swift** and replace its entire contents with the following code.</span></span>

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

            GraphManager.instance.getEvents {
                (data: Data?, error: Error?) in
                DispatchQueue.main.async {
                    self.spinner.stop()

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

<span data-ttu-id="5c4fc-111">您现在可以运行应用程序，登录，然后点击菜单中的 "**日历**" 导航项。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-111">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="5c4fc-112">您应该会看到应用程序中的事件的 JSON 转储。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-112">You should see a JSON dump of the events in the app.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="5c4fc-113">显示结果</span><span class="sxs-lookup"><span data-stu-id="5c4fc-113">Display the results</span></span>

<span data-ttu-id="5c4fc-114">现在，您可以将 JSON 转储替换为以用户友好的方式显示结果的内容。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-114">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="5c4fc-115">在本节中，您将修改`getEvents`函数以返回强类型的对象，并进行修改`CalendarViewController`以使用表视图呈现事件。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-115">In this section, you will modify the `getEvents` function to return strongly-typed objects, and modify `CalendarViewController` to use a table view to render the events.</span></span>

### <a name="update-getevents"></a><span data-ttu-id="5c4fc-116">更新 getEvents</span><span class="sxs-lookup"><span data-stu-id="5c4fc-116">Update getEvents</span></span>

1. <span data-ttu-id="5c4fc-117">打开**GraphManager**。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-117">Open **GraphManager.swift**.</span></span> <span data-ttu-id="5c4fc-118">将`getEvents`函数声明更改为以下项。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-118">Change the `getEvents` function declaration to the following.</span></span>

    ```Swift
    public func getEvents(completion: @escaping([MSGraphEvent]?, Error?) -> Void)
    ```

1. <span data-ttu-id="5c4fc-119">将 `completion(eventsData, nil)` 行替换成以下代码。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-119">Replace the `completion(eventsData, nil)` line with the following code.</span></span>

    ```Swift
    do {
        // Deserialize response as events collection
        let eventsCollection = try MSCollection(data: eventsData)
        var eventArray: [MSGraphEvent] = []

        eventsCollection.value.forEach({
            (rawEvent: Any) in
            // Convert JSON to a dictionary
            guard let eventDict = rawEvent as? [String: Any] else {
                return
            }

            // Deserialize event from the dictionary
            let event = MSGraphEvent(dictionary: eventDict)!
            eventArray.append(event)
        })

        // Return the array
        completion(eventArray, nil)
    } catch {
        completion(nil, error)
    }
    ```

### <a name="update-calendarviewcontroller"></a><span data-ttu-id="5c4fc-120">更新 CalendarViewController</span><span class="sxs-lookup"><span data-stu-id="5c4fc-120">Update CalendarViewController</span></span>

1. <span data-ttu-id="5c4fc-121">在名为`CalendarTableViewCell.swift`的**GraphTutorial**项目中创建一个新的**Cocoa Touch 类**文件。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-121">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewCell.swift`.</span></span> <span data-ttu-id="5c4fc-122">在字段的 "**子类**" 中选择 " **UITableViewCell** "。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-122">Choose **UITableViewCell** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="5c4fc-123">打开**CalendarTableViewCell** ，并将以下代码添加到`CalendarTableViewCell`类中。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-123">Open **CalendarTableViewCell.swift** and add the following code to the `CalendarTableViewCell` class.</span></span>

    ```Swift
    @IBOutlet var subjectLabel: UILabel!
    @IBOutlet var organizerLabel: UILabel!
    @IBOutlet var durationLabel: UILabel!

    var subject: String? {
        didSet {
            subjectLabel.text = subject
        }
    }

    var organizer: String? {
        didSet {
            organizerLabel.text = organizer
        }
    }

    var duration: String? {
        didSet {
            durationLabel.text = duration
        }
    }
    ```

1. <span data-ttu-id="5c4fc-124">打开 "**情节提要**" 并找到**日历场景**。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-124">Open **Main.storyboard** and locate the **Calendar Scene**.</span></span> <span data-ttu-id="5c4fc-125">在**日历场景**中选择**视图**并将其删除。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-125">Select the **View** in the **Calendar Scene** and delete it.</span></span>

    ![日历场景中的视图的屏幕截图](./images/view-in-calendar-scene.png)

1. <span data-ttu-id="5c4fc-127">将**库**中的**表视图**添加到**日历场景**中。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-127">Add a **Table View** from the **Library** to the **Calendar Scene**.</span></span>
1. <span data-ttu-id="5c4fc-128">选择表格视图，然后选择 "**属性" 检查器**。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-128">Select the table view, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="5c4fc-129">将 "**原型" 单元格**设置为**1**。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-129">Set **Prototype Cells** to **1**.</span></span>
1. <span data-ttu-id="5c4fc-130">使用**库**向原型单元格添加三个**标签**。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-130">Use the **Library** to add three **Labels** to the prototype cell.</span></span>
1. <span data-ttu-id="5c4fc-131">选择 "原型" 单元格，然后选择 "**标识检查器**"。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-131">Select the prototype cell, then select the **Identity Inspector**.</span></span> <span data-ttu-id="5c4fc-132">将**类**更改为**CalendarTableViewCell**。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-132">Change **Class** to **CalendarTableViewCell**.</span></span>
1. <span data-ttu-id="5c4fc-133">选择 "**属性" 检查器**并将`EventCell`"**标识符**" 设置为。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-133">Select the **Attributes Inspector** and set **Identifier** to `EventCell`.</span></span>
1. <span data-ttu-id="5c4fc-134">选择**EventCell**后，选择 "**连接检查器**" 和`durationLabel`" `organizerLabel`连接" `subjectLabel` ，以及添加到情节提要上的单元格的标签。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-134">With the **EventCell** selected, select the **Connections Inspector** and connect `durationLabel`, `organizerLabel`, and `subjectLabel` to the labels you added to the cell on the storyboard.</span></span>

    ![原型单元格布局的屏幕截图](./images/prototype-cell-layout.png)

1. <span data-ttu-id="5c4fc-136">打开**CalendarViewController** ，并将其内容替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-136">Open **CalendarViewController.swift** and replace its contents with the following code.</span></span>

    ```Swift
    import UIKit
    import MSGraphClientModels

    class CalendarViewController: UITableViewController {

        private let tableCellIdentifier = "EventCell"
        private let spinner = SpinnerViewController()
        private var events: [MSGraphEvent]?

        override func viewDidLoad() {
            super.viewDidLoad()

            // Do any additional setup after loading the view.
            self.spinner.start(container: self)

            GraphManager.instance.getEvents {
                (eventArray: [MSGraphEvent]?, error: Error?) in
                DispatchQueue.main.async {
                    self.spinner.stop()

                    guard let events = eventArray, error == nil else {
                        // Show the error
                        let alert = UIAlertController(title: "Error getting events",
                                                      message: error.debugDescription,
                                                      preferredStyle: .alert)

                        alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
                        self.present(alert, animated: true)
                        return
                    }

                    self.events = events
                    self.tableView.reloadData()
                }
            }
        }

        // Number of sections, always 1
        override func numberOfSections(in tableView: UITableView) -> Int {
            return 1
        }

        // Return the number of events in the table
        override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
            return events?.count ?? 0
        }

        override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
            let cell = tableView.dequeueReusableCell(withIdentifier: tableCellIdentifier, for: indexPath) as! CalendarTableViewCell

            // Get the event that corresponds to the row
            let event = events?[indexPath.row]

            // Configure the cell
            cell.subject = event?.subject
            cell.organizer = event?.organizer?.emailAddress?.name

            // Build a duration string
            let duration = "\(self.formatGraphDateTime(dateTime: event?.start)) to \(self.formatGraphDateTime(dateTime: event?.end))"
            cell.duration = duration

            return cell
        }

        private func formatGraphDateTime(dateTime: MSGraphDateTimeTimeZone?) -> String {
            guard let graphDateTime = dateTime else {
                return ""
            }

            // Create a formatter to parse Graph's date format
            let isoFormatter = DateFormatter()
            isoFormatter.dateFormat = "yyyy-MM-dd'T'HH:mm:ss.SSSSSSS"

            // Specify the timezone
            isoFormatter.timeZone = TimeZone(identifier: graphDateTime.timeZone!)

            let date = isoFormatter.date(from: graphDateTime.dateTime)

            // Output like 5/5/2019, 2:00 PM
            let dateFormatter = DateFormatter()
            dateFormatter.dateStyle = .short
            dateFormatter.timeStyle = .short

            return dateFormatter.string(from: date!)
        }
    }
    ```

1. <span data-ttu-id="5c4fc-137">运行应用程序，登录，然后点击 "**日历**" 选项卡。您应该会看到事件列表。</span><span class="sxs-lookup"><span data-stu-id="5c4fc-137">Run the app, sign in, and tap the **Calendar** tab. You should see the list of events.</span></span>

    ![事件表的屏幕截图](./images/calendar-list.png)
