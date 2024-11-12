```C#
//Wrong Example
void printOwing(){
	printBanner();

	//print details
	Console.WriteLine("name: " + name);
	Console.WriteLine("amount:" + getOutStanding());
}

//Solution Example
void printOwing(){
	printBanner();
	printDetails(getOutStanding());
}
void printDetails(double outstanding){
	Console.WriteLine("name: " + name);
	Console.WriteLine("amount:" + getOutStanding());
}
```

# Replace Temp With Query

```C#
//Wrong Example
double calculateTotal() { 
	double basePrice = quantity * itemPrice; 
	if (basePrice > 1000) { 
		return basePrice * 0.95;
	} else { 
		return basePrice * 0.98;
	}  
}

//Solution Example
double calculateTotal(){
	if( basePrice() > 1000 ){
		return basePrice() * 0.95;
	}else{
		return basePrice() * 0.98;
	}
}
double basePrice(){
	return quantity * itemPrice;
}
```

# Introduce Parameter

```C#
//Wrong Example
amountInvoiceIn(DateTime start, DateTime end);
amountReceivedIn(DateTime start, DateTime end);

//Solution Example
amountInvoiceIn(DateRange dateRange);
amountReceivedIn(DateRange dateRange);
```

# Preserve Whole Object

```C#
//Wrong Example
int low  = daysTempRange.getLow();
int high = daysTempRange.getHigh();
boolean withinPlan = plan.withinRange(low, high);

//Solution Example
boolean withinPlan = plan.withinRange(daysTempRange);
```

# Replace Method with Method Object

If in long methods the local variable are too intertwined than Extract Method might not work.

```C#
//Wrong Example
class Order{
	//...
	public double price(){
		double primaryBasePrice;
		double secondaryBasePrice;
		double tertiaryBasePrice;
		//Perform long computation.
	}
}

//Solution Example
class Order {
	//...
	public double price(){
		return new PriceCalculator(this).compute();
	}
}

class PriceCalculator {
	private double primaryBasePrice;
	private double secondaryBasePrice;
	private double tertiaryBasePrice;

	public PriceCalculator(Order order){
		//Copy relevant information from the order object.
	}

	public double compute(){
		//Perform long computation.
	}
}
```

# Decompose Conditional

```C#
//Wrong Example
if(date.before(SUMMER_START) || date.after(SUMMER_END)){
	...
}else{
	...
}

//Solution Example
if((isSummer(date)){
	...
}else{
	...
}
```