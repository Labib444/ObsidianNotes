dotnet test

![[Pasted image 20230331231151.png]]

- We are in the Test project so that project will run.
- dotnet test --filter "Category=EmployeeFactory_CreateEmployee_Salary"

- Running tests in parallel allows a set of tests to finish faster, locally and on your build server.
- classes in the same Collection will not run parallely. [Collection("No parallel")]

![[Pasted image 20230331232621.png]]
![[Pasted image 20230331233355.png]]
- If you want to test in multiple versions.