```
import { HttpClientModule } 
...
//app.module.ts
imports: [
	HttpClientModule
]
...
```

### Resolvers

> Route Resolvers are services

```TypeScript
//a service file
@Injectable({
	providedIn: 'root'
})
export class BooksResolverService implements Resolve<Book[] | BookTrackerError> {
	constructor(private dataService: DataService){ }
	resolve(route: ActivatedRouteSnapshot, 
			state: RouterStateSnapshot): Observable<Book[] | BookTrackerError {
				return this.dataService.getAllBooks().pipe(
					catchError(err => of(err))
				);
			}
}
```

```TypeScript
//app.route.module.ts
const routes: Routes = [
	{ path: 'dashboard', component: DashboardComponent, resolve: {resolvedBooks: BooksResolverService} }
];
```

```Typescript
//inside the component.ts file
import { ActivatedRoute } from '@angular/router'
...
constructor(private route: ActivatedRoute){}

ngOnInit(){
	let resolvedData: Book[] | BookTrackerError = this.route.snapshot.data['resolvedBooks'];

	if(resolvedData instanceof BookTrackerError){
		console.log('Dashboard component error: ${resolvedData.friendlyMessage}')
	}else{
		this.allBooks = resolvedData;
	}
}
```

### HttpInterceptor

![[Pasted image 20230912041322.png]]

![[Pasted image 20230912041515.png]]


![[Pasted image 20230912042020.png]]

![[Pasted image 20230912042348.png]]

![[Pasted image 20230912042826.png]]
- Above image only deals with http request that means api calls made to the server
![[Pasted image 20230912044119.png]]
- Above image is for http response sent by the api.





