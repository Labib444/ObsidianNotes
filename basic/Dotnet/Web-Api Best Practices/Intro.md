![[Pasted image 20230324183433.png]]

![[Pasted image 20230324191813.png]]

![[Pasted image 20230324191949.png]]
![[Pasted image 20230324192537.png]]
- GET, HEAD are considered as safe methods because they donot change the state.
- Idempotence: Can be repeated without changing system state.
- Safe: Doesn't change state of the system.

![[Pasted image 20230324193240.png]]
![[Pasted image 20230324193425.png]]

### Status Codes

![[Pasted image 20230324194101.png]]
![[Pasted image 20230324194223.png]]
![[Pasted image 20230324194313.png]]
![[Pasted image 20230324194353.png]]

![[Pasted image 20230324194435.png]]

![[Pasted image 20230324194506.png]]

### Return Types

![[Pasted image 20230324194950.png]]

![[Pasted image 20230324195015.png]]

![[Pasted image 20230324195125.png]]

- IActionResult, ActionResult(Generic) List(Generic).
- In terms of API documentation ActionResult is recommended.

### Model

![[Pasted image 20230325005218.png]]
- use DTOs and use .NET class annotations (maxlength, required etc)
- use records instead of classes (not super important)
- try to accept anything from the client but return very little.

![[Pasted image 20230325012553.png]]
