### What are services

- Reusable piece of funcitonality shared across components
- Responsible for a single, discrete piece of functionality
- Able to be delievered when and where it is needed
- 
### When to use angular services

###### Do limit logic in a component to only that required for the view. All other logic should be delegated to services. -Angular Style Guide

- The necessary functionality is not required by the view.
- You need to share logic or business rules across components
- You need to share data across components

### Creating and using services

- Class, @Injectable Decorator, Provider
- Angular Services are singletons, It gets created and then cached whenever any component requires the service it is then provided from the cache.

```Typescript
//Add Service postfix as an convention.
//in data.service.ts
import { Injectable } from "@angular/core";
@Injectable()
/*
	If this is used then we donot need to add in the provides of app modules    
	file. If this service is not used then the compiler will throw it out thus 
	a small bundle will be sent to the browser. (use when needed!)
	@Injectable({
		providedIn: 'root'
	})
*/
export class DataService 
{
	getData(){};
}
//in app modules
@NgModule({
	declarations: [
		AppComponent,
	],
	imports: [
		BrowserModule
	],
	bootstrap: [AppComponent],
	providers: [DataService]
})

export class AppModule {}
```

```Typescript
export class DashboardCompnent {
	constructor(private dataService: DataService){}
	ngOnInit(){
		this.allBooks = this.dataService.getAllBooks();
	}
	allBooks: Book[];
}
```
![[Pasted image 20230103004502.png]]


```AngularCli
ng generate service core/data --skip-tests true
ng g s core/data --skip-tests true
```

```Typescript
import { Injectable } from "@angular/core";
@Injectable({
	providedIn: 'root'
})
export class DataService 
{
	getData(){};
}
```

- We can have other services into a service.

### Services are singleton

In angular services are singleton which means that throughout the application lifetime it will be alive and any components which got injected with the services will get the same property values from the service. If some component changes any property value from the service then all other components will get the latest changes.
![[Pasted image 20230103010349.png]]

![[Pasted image 20230103011353.png]]

### Configuring Servicees

![[Pasted image 20230103011750.png]]
Doesn't need an interface like in other oop languages. (Please learn more about Typescript!)

###### Providers: A dependency provider configures an injector with a DI token, which that injector uses to provide the runtime version of a dependency value.

![[Pasted image 20230103012343.png]]

![[Pasted image 20230103012327.png]]

![[Pasted image 20230103012612.png]]

![[Pasted image 20230103012745.png]]
![[Pasted image 20230103013250.png]]
![[Pasted image 20230103012907.png]]


![[Pasted image 20230103013004.png]]


![[Pasted image 20230103013215.png]]
![[Pasted image 20230103013130.png]]


### Injectors
![[Pasted image 20230103020533.png]]
![[Pasted image 20230103021750.png]]


### Providers

![[Pasted image 20230103021229.png]]

### Http Services

![[Pasted image 20230103201502.png]]

![[Pasted image 20230103204740.png]]

![[Pasted image 20230103204818.png]]


- async functions always return a Promise so we can use "catch" to catch errors.