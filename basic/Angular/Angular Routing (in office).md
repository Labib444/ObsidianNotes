- Define base path
- Import Router
- Configure routes
- Place template
- Activate routes

### Base Path
![[Pasted image 20230126104849.png]]
```javascript
<base href="/"> //For development (localhost:4200) //This is defined in index.html
<base href="/APM/"> //For deployment (http://www.mySite.com/APM/)

//CLI
ng-build --base-href/APM/
```
![[Pasted image 20230126110803.png]]
- Router module is an external module which provides routerService to manipulate routes and redirecting. We can do configurations also can show in the UI using the stated Directives.
- RouterLink makes the ui element a clickable route link.
- RouterLinkActive can be used to style the active links.
- RouterOutlet is used to where to show the template 

![[Pasted image 20230126111334.png]]

- There can only be one active RouterModule multiple imports will not work.
 ![[Pasted image 20230126113452.png]]
 ![[Pasted image 20230126114705.png]]
 - route searching is done from Top to Bottom. So put specific routes in top and less specific routes in bottom.
![[Pasted image 20230126115052.png]]
- Activating a Route
![[Pasted image 20230126115432.png]]
![[Pasted image 20230126122850.png]]

![[Pasted image 20230126124253.png]]

![[Pasted image 20230126122914.png]]

### Routing to Features
- import router
- configure routes
- activate routes
![[Pasted image 20230126135052.png]]
- No need to import the service again
![[Pasted image 20230126135644.png]]
- we should create routes names related to the parent or topic.
 ![[Pasted image 20230126140110.png]]
![[Pasted image 20230126142724.png]]
![[Pasted image 20230126143804.png]]
 ![[Pasted image 20230126145216.png]]

### Angular Parameters
![[Pasted image 20230126153736.png]]![[Pasted image 20230126154117.png]]
![[Pasted image 20230126154655.png]]
![[Pasted image 20230126154747.png]]
![[Pasted image 20230126160631.png]]
- here + means string to number
![[Pasted image 20230126161402.png]]
![[Pasted image 20230126161834.png]]
![[Pasted image 20230126161848.png]]
![[Pasted image 20230126161918.png]]

- optional parameters donot effect route matching.