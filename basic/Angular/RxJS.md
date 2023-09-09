Reactive Extensions for Javascript

"RxJS is a library for composing asynchronous and event-based programs by using observable sequences." -official definition

"RxJS is a library for observing and reacting to data and events by using observable sequences." - our definition.

### Term and Syntaxes

![[Pasted image 20230113160329.png]]

```javascript
start -> emit items -> filtering -> next -> combining -> complete
```

- Observer: a collection of callbacks that knows how to listen to values delivered by the Observable.
- An Observer is a consumer of values delivered by an Observable.
- Obersavable emits notifications and Observer reacts to those notifications. [next(), error(), complete()]

```javascript
const observer = {
	next: apple => console.log(),
	error: err => console.log(),
	complete: () => console.log()
}
```

Observable: A collection of events or values emitted over time
- User actions 
- Application events (routing, forms)
- Response from an HTTP request
- Internal structures
- objects: classes
- arrays
- http

```javascript
//this can be done using of, from more easily but we need to know the background.
const apples$ = new Observable(appleSubscriber => {
	appleSubscriber.next('Apple 1');
	appleSubscriber.next('Apple 2');
	appleSubscriber.complete();
})

const sub = apples$.subscribe(observer);

//or

const sub = apples$.subscribe({
	next: apple => console.log(`Apple emitted ${apple}`),
	error: err => console.log(`Error occurred: ${err}`),
	complete: () => console.log(`No more apples, go home`)
});

sub.unsubscribe();
```


![[Pasted image 20230113163456.png]]

- Uncaught errors will stop auto stop the subscribtion

###### Creation functions
```javascript
const apples = of(['apple1', 'apple2']);
from(apples);
of(apples);
of(...apples) = from(apples)
```

- Creating Observable from events
![[Pasted image 20230113164603.png]]
```javascript
const num = interval(1000).subscribe(console.log);
```

- To get data we can have cold observable which have to subscribed to get data.
- Another type of observable is where we can get data without subscribing.

### Operators

```javascript
of(2, 4, 6)
	.pipe(
		map(item => item * 2),
		tap(item => console.log(item)),
		take(3)
	).subscribe(item => console.log(item));
```

![[Pasted image 20230113191942.png]]

![[Pasted image 20230113192505.png]]
![[Pasted image 20230113192925.png]]
![[Pasted image 20230113192943.png]]

- take() takes the specified number of observables and completes the flow.
![[Pasted image 20230113193251.png]]

- If we need to write a bigger function then....remember to return.
![[Pasted image 20230113193559.png]]

![[Pasted image 20230113193849.png]]

### Reactive Development

 ###### Procedural Code
 ![[Pasted image 20230114161421.png]]

###### async pipe
![[Pasted image 20230114180804.png]]

![[Pasted image 20230114161835.png]]
![[Pasted image 20230114161849.png]]

###### Handling Errors
![[Pasted image 20230114162407.png]]
![[Pasted image 20230114162424.png]]
![[Pasted image 20230114162442.png]]
![[Pasted image 20230114162454.png]]

###### Error handling code
![[Pasted image 20230114175015.png]]
![[Pasted image 20230114175043.png]]
- Can also use throw in place of throwError()

###### Improving change detection

![[Pasted image 20230114181045.png]]

###### Declarative Approach
![[Pasted image 20230114181438.png]]

- In service file
- ![[Pasted image 20230114181502.png]]
- In component file
![[Pasted image 20230114181519.png]]
- binding in ui file
![[Pasted image 20230114181546.png]]

### Mapping
![[Pasted image 20230114182418.png]]

- Mapping data where the function returns an array
![[Pasted image 20230114194414.png]]
![[Pasted image 20230114194904.png]]

### Combing Data Streams

- combineLatest
- forkJoin
- withLatestFrom

![[Pasted image 20230114203956.png]]

### Caching

- share
- shareReply


### Combing data streams

- combineLatest ( it is not pipable )
- All of the observables need to emit then one output will be provided by combineLatest

![[Pasted image 20230117132930.png]]

![[Pasted image 20230117132638.png]]
![[Pasted image 20230117132854.png]]
- forkJoin
![[Pasted image 20230117133142.png]]
![[Pasted image 20230117133246.png]]
![[Pasted image 20230117133326.png]]
- withLatestFrom
- It is a pipable operator.
![[Pasted image 20230117150715.png]]
![[Pasted image 20230117150834.png]]
- A1 didn't emit because all other observables didn't emit.
![[Pasted image 20230117151038.png]]
- It ends when source completes.
![[Pasted image 20230117151126.png]]
- If all sources emit then combineLatest will emit.
- If all sources complete then forkJoin will emit.
- If source emits and all other sources have emitted then withLatestFrom will emit. If source completes and others didn't complete then there will be no new emits.
![[Pasted image 20230117152603.png]]
![[Pasted image 20230117161641.png]]
![[Pasted image 20230117161654.png]]
![[Pasted image 20230117161829.png]]

### Reacting to Actions

- filter
- startWith
- Subject
- BehaviorSubject
![[Pasted image 20230222210508.png]]
![[Pasted image 20230222210705.png]]
![[Pasted image 20230222211428.png]]
![[Pasted image 20230222211531.png]]
![[Pasted image 20230222221732.png]]
![[Pasted image 20230222221655.png]]


![[Pasted image 20230222221614.png]]

![[Pasted image 20230222221538.png]]

![[Pasted image 20230222221501.png]]

![[Pasted image 20230222221428.png]]
![[Pasted image 20230222223233.png]]
![[Pasted image 20230222223711.png]]

  ![[Pasted image 20230316194642.png]]
  
  ![[Pasted image 20230316194737.png]]
  ![[Pasted image 20230316194711.png]]

 ![[Pasted image 20230316211128.png]]
 ![[Pasted image 20230316211204.png]]
 ![[Pasted image 20230316212820.png]]
 ![[Pasted image 20230316213020.png]]
 ![[Pasted image 20230316213101.png]]
 ![[Pasted image 20230316213209.png]]
 ![[Pasted image 20230316213249.png]]
 ![[Pasted image 20230316213359.png]]
 ![[Pasted image 20230316213953.png]]
 ![[Pasted image 20230316221830.png]]
 ![[Pasted image 20230316223501.png]]
 ![[Pasted image 20230316225646.png]]

### Caching

