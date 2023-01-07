### Tools
- ASP.NET Core 6.0
- xUnit 2.4.1
- Moq 4.17.2
- .NET 6.0
- Visual Studio 2022

### Three Types of Testing
- Unit Tests
- Integration Tests
- Functional (end-to-end) tests

###### Unit Test
- Should have low complexities.
- Should be fast
- Should be well encapsulated

###### Integration Test
- A integration test is an automated test taht tests whether or not two or more components work together correctly.
- Can test a full request/response cycle but not required.
- Optionally combined with Microsoft TestHost and TestServer
- less isolated
- slow
- medium complexity
- not so well encapsulated
- There should be more unit tests than integration tests

###### Functional Test
- A unit test is an automated test that tests the full request/response cycle of an application.
- Can be automated with
	- Selenium
	- Postman
	- Microsoft TestHost and TestServer
- high complexity
- very slow
- badly encapsulated
- Unit test numbers > Integration test numbers > functional tests

### Adding an Unit Test Project

 - Create new XUnit Project from visual studio 2022.
 - Add csproj by clicking on the project name. (this helps us stop the using the "using" keyword everytime when importing.)
 ```XML
 <PropertyGroup>
	 ...
	 <ImplicitUsings>true</ImplicitUsings> //add this line
 </PropertyGroup>
```

- Right Click on the project name -> Project References -> Choose your API project.
- Add new class in Test Project.
- "EmployeeFactoryTests.cs"
```C#
namespace EmployeeManagement.Test
{
	public class EmployeeFactoryTests
	{
		[Fact] //Fact means Unit Test
		public void CreateEmployee_ConstructionInternalEmployee_SalaryMustBe2500()
		{
			
		}
	}
}
```
- Open Text Explorer (can run tests on it)
```C#
namespace EmployeeManagement.Test
{
	public class EmployeeFactoryTests
	{
		[Fact] //Fact means Unit Test
		public void CreateEmployee_ConstructionInternalEmployee_SalaryMustBe2500()
		{
			//Arrange
			var employeeFactory = new EmployeeFactory();
			//Act
			var employee = (InternalEmployee) employeeFactory.
				CreateEmployee("Kevin", "Docks");
			//Assert
			Assert.Equal(2500, employee.Salary);
		}
	}
}
```
- Now run the test from test explorer.

<span style='color: red'>CreateEmployee</span> _<span style="color: green">ConstructionInternalEmployee</span>_SalaryMustBe2500
- Red: Unit Name
- Green: Scenario
- Pink: Expectation

### Different Types of Unit Testing

- We should be using one Asset in one method (in theory). But in theory we can use multiple Asserts but if you have 20 Asserts in a function then you might be doing something wrong.
- We should be testing one behaviour in a function not multiple.

**Wrong**:

![[Pasted image 20230107024848.png]]

###### Assert Keyword

```C#
//Boolean
Assert.True(course.IsNew);
Assert.False(course.IsNew);

//String
Assert.Equal("Lucia Shelton", employee.FullName);
Assert.NotEqual("Lucia Shelton", employee.FullName);

Assert.Equal("Lucia Shelton", employee.FullName, ignoreCase: true);
Assert.StartsWith(employee.FirstName, employee.FullName, StringComparison.);
Assert.StartsWith(employee.FirstName, employee.FullName);
Assert.EndsWith(employee.LastName, employee.FullName)
Assert.Contains("is Sh", employee.FullName)
Assert.DoesNotContains("is Sh", employee.FullName)

Assert.Matches("Lu(c|s|z)ia Shel(t|d)on", employee.FullName);
Assert.DoesNotMatches("Lu(c|s|z)ia Shel(t|d)on", employee.FullName);

Assert.Empty(...)
Assert.NotEmpty(...)

//Numeric
Assert.Equal(2500, employee.Salary);
Assert.True(salary >= 2500 && salary <= 3500, "salary not in range")
Assert.InRange(salary, 3000, 3500);


//Float
salary = 2500.123m;
Assert.Equal(2500, salary, 0); //True will use 0 precision

//Arrays
Assert.Contains(internalEmployee.AttendedCourses,
	course => course.Id == Guid.Parse("27324-ad352")
			   );
Assert.Equal(obligatoryCourses, internalEmployee.AttendedCourses)
Assert.All(internalEmployee.AttendedCourses,
		course => Assert.False(course.IsNew)
		  );
		  
//Exceptions
await Assert.ThrowsAsync<EmployeeInvalidRaiseException>(
	async() => {
		await employeeService.GiveRaiseAsync(internalEmployee, 50)
	}
)
Assert.Throws<T>(...)
Assert.ThrowsAny<T>(...)

//Event
Assert.Raises<EmployeeIsAbsentEventArgs>(
	handler => employeeService.EmployeeIsAbsent += handler,
	handler => employeeService.EmployeeIsAbsent -= handler,
	() => employeeService.NotifyOfAbsence(internalEmployee));
)
Assert.RaisesAny(Async)<T>(...)
Assert.Raises<T>(...)

Assert.IsType<ExternalEmployee>(employee);
Assert.IsNotType<ExternalEmployee>(employee);
Assert.IsAssignableFrom<Employee> //Is employee or derivated type of Employee
Assert.IsNotAssignableFrom<Employee>

```

- Test the behaviour of the public method which uses the private method. Donot test private methods as it is private. There always some public method that uses the private method. DONOT make the private method public. Use [InternalsVisible] to test private method but not advised.

### Setting up tests and sharing test context

- Constructor and dispose
- Class fixture
- Collection fixture

