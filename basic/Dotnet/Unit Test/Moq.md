- install Moq from NuGet packages.
	- create new class "MoqTests.cs"

```C#
public class MoqTests
{
	[Fact]
	public void Test()
	{
		var employeeManagementTestDataRepository = new EmployeeManagementTestDataRepository();
		var employeeFactoryMock = new Mock<EmployeeFactory>();
		
		var employeeService = new EmployeeService(
			employeeManagementTestDataRepository,
			employeeFactoryMock.Object
		);

		employeeFactoryMock.Setup(m => m.CreateEmployee(
			It.IsAny<string>(),
			It.IsAny<string>(),
			null,
			false
		)).Returns(new InternalEmployee("Kevin", "Dockx", 5, 2500, false, 1));

		employeeFactoryMock.Setup(m => m.CreateEmployee(
			"Kevin",
			It.IsAny<string>(),
			null,
			false
		)).Returns(new InternalEmployee("Kevin", "Dockx", 5, 2500, false, 1));

		employeeFactoryMock.Setup(m => m.CreateEmployee(
			It.Is<string>(value => value.Contains("a")),
			It.IsAny<string>(),
			null,
			false
		)).Returns(new InternalEmployee("Kevin", "Dockx", 5, 2500, false, 1));
	}
}
```

### Mocking interfaces

```C#
var employeeManagementTestDataRepositoryMock = new Mock<IEmployeeManagementRepository>();
```
![[Pasted image 20230111145313.png]]
- For mocking async methods do all that is necessary and then use the "ReturnsAsync" method.