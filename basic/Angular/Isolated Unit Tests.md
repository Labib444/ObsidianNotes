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

![[Pasted image 20230104124930.png]]
