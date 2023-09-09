
### Step 1

This is the launchSetting.json
```C#
{
  "profiles": {
    "RealTimeCharts.Server": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "https://localhost:5001;http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

### Step 2

Adding CORS Policy

```C#
builder.Services.AddCors(options =>
{
    options.AddPolicy("CorsPolicy", builder => builder
        .WithOrigins("http://localhost:4200")
        .AllowAnyMethod()
        .AllowAnyHeader()
        .AllowCredentials());
});
builder.Services.AddControllers();
var app = builder.Build();
// Configure the HTTP request pipeline.
app.UseHttpsRedirection();
app.UseCors("CorsPolicy");
app.UseAuthorization();
// Plesae remember to call UseCors method before UseAuthorization.
```

### Step 3

We will now install SignalR in angular.

```bash
npm install @microsoft/signalr --save
```

### Step 4

Create Folder "Models" > Create New Class "ChartModel"

```C#
public class ChartModel
{
    public List<int> Data { get; set; }
    public string? Label { get; set; }
    public string? BackgroundColor { get; set; }

    public ChartModel()
    {
        Data = new List<int>();
    }
}
```

### Step 5

Create Folder "HubConfig" > Create New Class "ChartHub"

```C#
// We need to inherit from Hub
public class ChartHub : Hub
{
}
```

Well, a `Hub` is a high-level pipeline that allows communication between client and server to call each other methods directly. So basically, a `Hub` is a communication foundation between client and server while using SignalR.

### Step 6

In Program.cs

```C#
builder.Services.AddSignalR();
...
app.MapControllers();
app.MapHub<ChartHub>("/chart");
```

In the first part, we add SignalR to the `IService` collection using the `AddSignalR` method. And then, we add SignalR to the request pipeline by pointing to our `ChartHub` with the provided `/chart` path.

### Step 7

To simulate a real-time data flow from the server, we are going to implement a `Timer` class from the `System.Threading` namespace. Let’s create a new folder `TimerFeatures` and inside it a new class `TimerManager`:

```C#
public class TimerManager
{
    private Timer? _timer;
    private AutoResetEvent? _autoResetEvent;
    private Action? _action;
    public DateTime TimerStarted { get; set; }
    public bool IsTimerStarted { get; set; }

    public void PrepareTimer(Action action)
    {
        _action = action;
        _autoResetEvent = new AutoResetEvent(false);
        _timer = new Timer(Execute, _autoResetEvent, 1000, 2000);
        TimerStarted = DateTime.Now;
        IsTimerStarted = true;
    }

    public void Execute(object? stateInfo)
    {
        _action();

        if ((DateTime.Now - TimerStarted).TotalSeconds > 60)
        {
            IsTimerStarted = false;
            _timer.Dispose();
        }
    }
}
```

### Step 8

Create New Folder "DataStorage" > Create New Class "DataManager" We are going to use this class to fake our data:

```C#
public class DataManager
{
    public static List<ChartModel> GetData()
    {
        var r = new Random();
        return new List<ChartModel>()
        {
            new ChartModel { Data = new List<int> { r.Next(1, 40) }, Label = "Data1", BackgroundColor = "#5491DA" },
            new ChartModel { Data = new List<int> { r.Next(1, 40) }, Label = "Data2", BackgroundColor = "#E74C3C" },
            new ChartModel { Data = new List<int> { r.Next(1, 40) }, Label = "Data3", BackgroundColor = "#82E0AA" },
            new ChartModel { Data = new List<int> { r.Next(1, 40) }, Label = "Data4", BackgroundColor = "#E5E7E9" }
        };
    }
}
```

```C#
builder.Services.AddSingleton<TimerManager>();
```

### Step 9

Finally, to complete this section, we are going to create a new controller file `ChartController` inside the `Controllers` folder:

```C#
[Route("api/[controller]")]
[ApiController]
public class ChartController : ControllerBase
{
    private readonly IHubContext<ChartHub> _hub;
    private readonly TimerManager _timer;
        
    public ChartController(IHubContext<ChartHub> hub, TimerManager timer)
    {
        _hub = hub;
        _timer = timer;
    }

    [HttpGet]
    public IActionResult Get()
    {
        if (!_timer.IsTimerStarted)
            _timer.PrepareTimer(() => _hub.Clients.All.SendAsync("TransferChartData", DataManager.GetData()));
        return Ok(new { Message = "Request Completed" });
    }
}
```

In this controller class, we are using the `IHubContext` interface to create its instance via dependency injection. By using that instance object, we are able to access and call the hub methods. This is the reason why we don’t have any method in our `ChartHub` class. We don’t need any yet, because we are providing just one-way communication (the server is sending data to the client only), and we can access all the hub methods with `IHubContext` interface.

Now, we have to pay attention to the `_hub.Clients.All.SendAsync("transferchartdata", DataManager.GetData())` expression. With it, we are sending generated data to all subscribed clients to the `transferchartdata` event. This means that every client if it has a listener on the `transferchartdata` event, will receive data generated by the `DataManager` class. And that is exactly what we are going to do in the next section.
