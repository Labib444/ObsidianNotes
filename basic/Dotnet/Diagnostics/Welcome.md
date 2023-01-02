
![[Pasted image 20221229155057.png]]

```C#
//Add
<PackageReference Include="Serilog.Sinks.Seq" Version="5.2.2" />
```

![[Pasted image 20221229155431.png]]

![[Pasted image 20221229155516.png]]

```C#
docker pull datalust/seq
docker run -d --name seq --restart unless-stopped -e ACCEPT_EULA=Y -p 5341:80          
	datalust/seq

```
 
### Part 2

```C#
var timer = new StopWatch();
timer.Start();

//some code

timer.Stop();
_logger.LogInformation("Time: {ElapsedMs}", timer.ElapsedMilliseconds);
```

### Part 3

![[Pasted image 20221229160557.png]]

### Part 4

```C#
dotnet tool install --global dotnet-trace
```

![[Pasted image 20221229160807.png]]

```C#
dotnet-tract --help
dotnet-trace collect --help
//go to your api or solution directory
dotnet run

//open new powershell tab
dotnet-trace ps
dotnet-trace collect -p 13224 //put your own process id

//now do some swagger or postman api calls then stop this running process
//and watch the result

//press Ctrl+C to cancel

```

![[Pasted image 20221229161316.png]]

in visual studio go to file -> open file -> select the file of your .nettrace.

### Part 5 (BenchMarking)

![[Pasted image 20221229163442.png]]

![[Pasted image 20221229163448.png]]

Create a console app named BenchMarking.
Right click on BenchMarking project and  set "Project Reference" for your project.


![[Pasted image 20221229164116.png]]

![[Pasted image 20221229164035.png]]

### Part 6 (Loading Testing)

![[Pasted image 20221229165607.png]]

 ![[Pasted image 20221229165449.png]]
 