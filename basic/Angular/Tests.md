- Isolated tests
- Integration tests
- Asynchronous Code

###### Automated Testing

- Unit Testing
- End to End Testing
- Integration or functional testing

```Angular

```

![[Pasted image 20230103225858.png]]

![[Pasted image 20230103230028.png]]

![[Pasted image 20230103230017.png]]

- first-test.spec.ts (spec = specification) the spec keyword is required in the file angular will know it a test file.

### Jasmine

```Typescript
describe('my first test', () => {
	let sut;

	beforeEach(() => {
		sut = {}
	})

	it('should be true if true', () => {
		//Write Test
		//arrange
		sut.a = false;
		//act
		sut.a = true;
		//assert
		expect(sut.a).toBe(true);
	})
})
```

Linguistics should match with the test!

![[Pasted image 20230103233518.png]]

```Powershell
npm test
```

![[Pasted image 20230104004436.png]]

![[Pasted image 20230104004550.png]]

- We use DAMP in testing.