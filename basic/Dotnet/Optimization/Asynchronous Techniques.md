
### Part 1

- One at a time, please! (Synchronous).
- Keeps Main Thread free and uses other threads to do background task.(Asynchronous)

### Part 2

```C#

```

### Part 3

When there is no remote calls we just want to make normal functions asynchronous then use the following method.

```C#
public string RemoteContent {get; set;}

public Task<string> GetRemoteContentAsync()
{
	return Task.FromResult("Some content from a remote API or DB call!");
}

//another method
public async Task OnGetAsync()
{
	RemoteContent = await GetRemoteContentAsync();
}
```

### Part 4

Try to avoid such code as it behaves like synchronous code. This kind of codes can be found in Startup of a project where async/await will not work.

```C#
return client.GetData()
	.GetAwaiter()
	.GetResult()
```

### Part 5

Both methods are not running concurrently.  SetData() is will start when GetData() is finished.

```C#
 var getTask = await _extraLogic.GetData();
 var setTask = await _extraLogic.SetData();
```

Both Methods will now run concurrently. SetData() is no longer dependant on GetData() to finish.

 ```C#
 var getTask = _extraLogic.GetData();
 var setTask = _extraLogic.SetData();
 await Task.WhenAll(getTask, setTask);

var getData = await getTask;
var setData = await setTask;

```

```C#
//sleep method
//synchronous sleep
Thread.Sleep(5000)

//better sleep method
//asynchronous alternative for sleep
/*  
	if it is used in a method then we have to use "async" keyword and return 
	type should be "Task". 
*/
await Task.Delay(5000)
```

### Part 6

Cancellation Tokens
- Useful for stopping heavy operations
- Good for read
- Bad for update 

### Part 7

```C#
/* 
	Cancellation Token
	In middle of a API request if the user decides to change page or quits
	then cancelToken can be used to stop or cancel the request else
	the request will do its job. Think about large/heavy API calls. 
*/
public async Task<IActionResult> GetData(CancellationToken cancelToken, int Id)
{
	var data = _dbContext.ToListAsnyc(cancelToken);
	return data;
}
```