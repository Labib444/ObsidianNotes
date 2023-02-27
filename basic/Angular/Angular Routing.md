- Primary routes
- Child routes
- Secondary routes

### query params

- we pass complex data in such params.
![[Pasted image 20230128154501.png]]
![[Pasted image 20230128154736.png]]

![[Pasted image 20230128154652.png]]
![[Pasted image 20230128154903.png]]
![[Pasted image 20230128162701.png]]

![[Recording 20230201010103.webm]]

![[Pasted image 20230128162712.png]]

![[Pasted image 20230128201610.png]]
![[Pasted image 20230128202758.png]]
- resolving errors
![[Pasted image 20230128203842.png]]
![[Pasted image 20230128204410.png]]
![[Pasted image 20230128204913.png]]
![[Pasted image 20230128205147.png]]

### Child Routes
![[Pasted image 20230201015858.png]]

![[Pasted image 20230201013400.png]]
![[Pasted image 20230201013422.png]]


![[Pasted image 20230201011656.png]]
![[Pasted image 20230201011808.png]]

### Grouping and Component less Routes
![[Pasted image 20230201020035.png]]
![[Pasted image 20230201020128.png]]
- Children gets extended by the Parent.
![[Pasted image 20230201020442.png]]

### Styles
![[Pasted image 20230204192250.png]]
![[Pasted image 20230204201345.png]]
![[Pasted image 20230204204053.png]]
![[Pasted image 20230204204403.png]]
![[Pasted image 20230204205800.png]]
![[Pasted image 20230204205810.png]]

### Secondary Routes

![[Pasted image 20230205004425.png]]
![[Pasted image 20230205004554.png]]
![[Pasted image 20230205004850.png]]
![[Pasted image 20230205004959.png]]
![[Pasted image 20230205005230.png]]
![[Pasted image 20230205005300.png]]
![[Pasted image 20230205005829.png]]

### Route Guards
![[Pasted image 20230205010337.png]]
![[Pasted image 20230205011232.png]]
- canDeactive is checked first then ..... lastly resolved is called.
- ![[Pasted image 20230205011335.png]]
![[Pasted image 20230205011421.png]]
- adding guard in the parent route automatically guards the call routes.
- ng generate guard user/auth
![[Pasted image 20230205021158.png]]
![[Pasted image 20230205021317.png]]
![[Pasted image 20230205021349.png]]
### Lazy Loading
![[Pasted image 20230206202126.png]]
- required to do lazy loading.
![[Pasted image 20230206214359.png]]
![[Pasted image 20230206214932.png]]
![[Pasted image 20230206215012.png]]
- need to remove products from the right side also canActivate and remove "children: ["
