### current state of learning

![[Pasted image 20230909163610.png]]

### What happens when you an object

![[Pasted image 20230909222025.png]]

> You need one action for actual update, depending on the success of update we have two actions one for updateSuccess and one for updateFailure

![[Pasted image 20230909235027.png]]

### Architecture
###### Presentational and Container Component
![[Pasted image 20230909235822.png]]

![[Pasted image 20230910000048.png]]

![[Pasted image 20230910002208.png]]

###### Some UI related technical topic
![[Pasted image 20230910001036.png]]

> By default .OnPush is not used by default .Default is used, learn more about them in angular docs.

###### Barrel Format

> Basically create a new file named index.ts file in your state folder and put interfaces and selectors inside of it.
![[Pasted image 20230910005627.png]]
###### Formatting Actions

![[Pasted image 20230910005805.png]]

> Those Actions which deal with API call are kept in product-api.actions.ts and those don't are in product-page.actions.ts. 
> 
> Give [Product Page ...] names to product-page.actions.ts
> Give [Product API ...] names to product-api.actions.ts

###### How to structure state modules

![[Pasted image 20230910013042.png]]


![[Recording 20230910013248.webm]]


