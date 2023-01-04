## Do just one thing

**Wrong**
```javascript
function readFile(){
	//reading file
	//writing file
	//storing some random data in somewhere else
}
```

**Correct**
```javascript
function readFile(){
	//reading a file
} 
```

## Order Matters

Relavant function calls inside a function should be placed just below the function also the sequence of those functions should mirror the parent function.

***Wrong*(Order of call not mainted)**
```javascript
function getAndPreprocessData(){
	initializeAPI();         // call no: 1
	data = preprocessData(); // call no: 2
	data = sortData(data);   // call no: 3
	return data;
}

/*  
    The order of function calls in getData() is not maintained in the below declared 
    functions
*/
function sortData(data){   //call no: 3
	...
}
function preprocessData(){ //call no: 2
	...
}
function initializeAPI(){  //call no: 1
	...
}
```

**Correct**
```javascript
function getAndPreprocessData(){
	initializeAPI();
	data = preprocessData();
	data = sortData(data);
	return data;
}

/*  
    The order of function calls in getData() is maintained
*/
function initializeAPI(){
	...
}
function preprocessData(){
	...
}
function sortData(data){
	...
}
```

## No hidden effects

***Wrong(With side effects)***
```javascript
function checkPasswordMatched(userName, password){
	User user = DB.getUser(username);
	if(user.password == password){
		prepareHomePage(); // <--- ??? This task doesn't support the function name
		return true;
	}
	return false;
}
```

***Correct(No side effects)***
```javascript
function checkPasswordMatched(userName, password){
	User user = DB.getUser(username);
	if(user.password == password){
		return true; // side effect removed
	}
	return false;
}
```



## Either do or answer

The defined function should do something or answer something. In the following example the function *setValue()* should only be used for setting a value. We should not be returning something else or answering something. 

Assume a function named *isValid()*, this function will return true or false as it is answering something. This *isValid()* function should not be doing other than answering.

***Correct***
```javascript
setValue(value){
	value = 10;
}
isValid(data){
	if( data == valid )
		return true;
	else{
		return false;
	}
}
```

***Wrong***
```javascript
setValue(value){
	value = 10;
	return true;         //irrelavant
}
isValid(data){
	doSomeCalculation(); //irrelavant
	if( data == valid ){
		return true;
	}else{
		return false;	
	}
}
```