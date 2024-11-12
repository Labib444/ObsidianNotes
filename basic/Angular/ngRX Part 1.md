![[Pasted image 20230222225453.png]]
![[Pasted image 20230222225506.png]]

### Installing NgRx

- npm install
- the store is runtime use only. If you refresh the page or get out of the web application the store will be destroyed.
- ng add @ngrx/store
- store variables are immutable, it is done like because of performance issues.
```javascript
//(app.module.ts)
import { StoreModule } from '@ngrx/store'
StoreModule.forRoot({},{}) //automatically adds it in imports
// the first {} is for list of reducers
// the second {} is for configuration and it is optional parameter can omit
```

```javascript
StoreModule.forRoot(reducer) //In app module
StoreModule.forFeature('products', productReducer) //In feature modules
```

```javascript
StoreModule.forFeature('products',
	{
		//Key : Value Key is the object name and   Value is the name of the reducer that object is using
		productList: listReducer, 
		productData: dataReducer
	}  
)
```

![[Pasted image 20230908034405.png]]

### Reducer

- Reducers must be a pure function which means f(a, b) => a+b, if you put a=1,b=1 it will always output 2 but in f(a) => a+b, here b is a global variable and the value of b may vary therefore the output of f(a) is not always same for the same parameter, so that is a impure function.

```javascript
//product.reducer.ts
export const productReducer = createReducer(
	initialState,
	on(ProductActions.toggleProductCode, state => {
		return{
			...state,
			showProductCode: !state.showProductCode
		};
	}),
	on(ProductActions.hideProductCode, state => {
		return {
			...state,
			showProductCode: false
		};
	})
	on(ProductActions.hideProductCode,
		ProductActions.resetDefaults, // can put multiple actions
		state => {
		return {
			...state,
			showProductCode: false
		};
	})
);
```

- add a "state" folder in your feature module.
- add product.reducer.ts

```javascript
//product.reducer.ts
import { createReducer, on, createAction } from '@ngrx/store';
export const productReducer = createReducer(
	{ showProductCode: true },
	on(createAction('[Product] Toggle Product Code'), state => {
		return {
			...state,
			showProductCode: !state.showProductCode
		};
	})
);
```

```javascript
//product.module.ts
import { productReducer } from './state/product.reducer';
...
...
StoreModule.forFeature('products', productReducer)
```

![[Pasted image 20230908041200.png]]


![[Recording 20230908043027.webm]]

![[Recording 20230908043540.webm]]

```javascript
//product-list.component.ts
import { Store } from '@ngrx/store'
...
...
constructor(private store: Store<any>){

}

buttonClicked(){
	this.store.dispatch(
		{ type: '[Product] Toggle Product Code' }
	);
}
```

```javascript
/* Retriving data from the store */
this.store.select('products');
// OR
this.store.pipe(select('products')) //since the store is an observable

//TODO: Unsubscribe
this.store.select('products').subscribe(
	products => {
		if(products) this.displayCode = products.showProductCode;
	}
);
```

### Tools
- ng add @ngrx/store-devtools

### Strongly Typing the State

- We need to create typescript interfaces for the objects that are stored in the Store
```TypeScript
// (make a folder named state inside of app then store the model files.)
// app.state.ts
export interface State {
	products: ProductState;
}

```
![[Pasted image 20230908151503.png]]

```TypeScript
//due to some lazy loading issues we have to do it like this.
//app.state.ts
export interface State {
	user: any; //removed the ProductState property
}

//product.reduser.ts
import * as AppState from '../../state/app.state';

export interface ProductState {
	showProductCode: boolean;
	currentProduct: Product;
	products: Product[];
}

export interface State extends AppState.State {
	products: ProductState;
}
```

```TypeScript
//product.reduser.ts

import { createReducer, on, createAction } from '@ngrx/store';
export const productReducer = createReducer<ProductState>(
	{ showProductCode: true } as ProductState ,
	on(createAction('[Product] Toggle Product Code'), (state): ProductState => {
		return {
			...state,
			showProductCode: !state.showProductCode
		};
	})
);
```

```Typescript
//product-list.component.ts
import { Store } from '@ngrx/store'
import { State } from '../state/product.reducer' // important!
// here we are not importing the State class from app.state.ts
...
...
constructor(private store: Store<State>){
}

buttonClicked(){
	this.store.dispatch(
		{ type: '[Product] Toggle Product Code' }
	);
}
```

### Setting up initial state

```Typescript
//product.reducer.ts
...
...
//initialState value is of type const because no data can change here plus we already now that Store objects are immutable.
const initialState: ProductState = {
	showProductCode: true,
	currentProduct: null,
	products: []
};
...
...
export const productReducer = createReducer<ProductState>(
	initialState,
	on(createAction('[Product] Toggle Product Code'), (state): ProductState => {
		return {
			...state,
			showProductCode: !state.showProductCode
		};
	})
);
```

### Using selectors

- selectors are cache which means it will not change anything if the state is not changed.
- selectors should be a pure function
![[Pasted image 20230908154338.png]]
![[Pasted image 20230908160417.png]]

```Typescript
//product.reducer.ts
...
const getProductFeatureState = createFeatureSelector<ProductState>('products');

export const getShowProductCode = createSelector(
	getProductFeatureState,
	state => state.showProductCode
);

export const getProducts = createSelector(
	getProductFeatureState,
	state => state.products
);
...
```
```TypeScript
//product-list.component.ts
...
this.store.select(getShowProductCode).subscribe(
	showProductCode => this.displayCode = showProductCode
);
...
```
###### Composing Selectors
![[Pasted image 20230908161706.png]]

