
- Callbacks
- Generators/Coroutines
- Promises


```javascript
var a = function() {
	console.log("I am a function")
};

setTimerOut(a(), 1000); //Here a() is a callfunction for the timer.

```

```javascript
function* gen()
{
	console.log("Hello");
	yield null;
	console.log("World");
}

var it = gen();
it.next(); //will print "Hello"
it.next(); //will print "World"
```


![[Recording 20221230233523.webm]]

![[Pasted image 20221230233954.png]]


![[Recording 20221230234800.webm]]

![[Pasted image 20221230234613.png]]

### Promises

![[Pasted image 20221231145251.png]]

![[Pasted image 20221231145305.png]]

![[Pasted image 20221231145313.png]]


```javascript
//IIFE Functions
(function(){
    console.log("abc");
})();

  
//Closures
var abc = (function(){
    console.log("1 2 3 4 5");
    return {
        Data: "12345"
    }
});
console.log( abc().Data );
```

```javascript
//Parallel computation in async method.
var result = async function abc(){
    var a = getData();
    var b = getData2();
    //Method 1
    var c = await a;
    var d = await b;
    //Method 2
    var e = await Promise.all([c, d]);
}

//abc function returns a promise we then can catch errors
result().catch((a) => {
    console.log("Error!");
})
```