###### Testing Custom Pipes

```Typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
	name: 'strength'
})

export class StrengthPipe implements PipeTransform{
	transform(value: number): string {
		if(value < 10){
			return value + " (weak)";
		} else if(value >= 10 && value < 20){
			return value + " (strong)";
		}else {
			return value + " (unbelievable)";
		}
	}
}
```

```Typescript
describe('StrengthPipe', () => {
	it('should display weak if strength is 5', () {
		let pipe = new StrengthPipe();
		expect(pipe.transform(5)).toEqual('5 (weak)');
	})

	//...more it functions
})
```

###### Testing Services

![[Pasted image 20230104105519.png]]

###### Testing Components

Cannot pass services in test. Therefore we need a mock;

![[Pasted image 20230104124930.png]]
We declare ( let mockHeroService; )

![[Pasted image 20230104144348.png]]
xit means that skip this test.
![[Pasted image 20230104144904.png]]
![[Pasted image 20230104144931.png]]

###### Shallow Tests
- naming file: file.component.shallow.spec.ts
![[Pasted image 20230104151055.png]]

![[Pasted image 20230104151350.png]]

![[Pasted image 20230104153733.png]]
![[Pasted image 20230104154308.png]]
![[Pasted image 20230104173939.png]]
![[Pasted image 20230104174558.png]]

![[Pasted image 20230104175724.png]]
![[Pasted image 20230104175708.png]]
![[Pasted image 20230104180416.png]]
