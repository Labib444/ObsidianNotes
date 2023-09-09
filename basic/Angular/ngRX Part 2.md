### Current state of the learning
![[Pasted image 20230908161907.png]]

### Strongly Typing Actions

```TypeScript
export const toggleProductCode = createAction('[Product] Toggle Product Code');

export const productReducer = createReducer(
	initialState,
	on(toggleProductCode, state => {
		return {...};
	})
);

this.store.dispatch(toggleProductCode());
```

```TypeScript 
//if an action contains data or provides data to someone else.
export const setCurrentProduct = createAction(
	'[Product] Set Current Product',
	props<{ product: Product }>()
);
```


> Create a new file inside of product > state folder > product.action.ts


```TypeScript
//product.actions.ts
export const toggleProductCode = createAction(
	'[Product] Toggle Product Code'
);

export const setCurrentProduct = createAction(
	'[Product] Set Current Product',
	props<{ product: Product }>()
);
```

```Typescript
//product.reducer.ts
import * as ProductActions from './product.actions'
export const productReducer = createReducer<ProductState>(
	initial,
	on(ProductActions.toggleProductCode, (state): ProductState => {
		return {...};
	}),
	on(ProductActions.setCurrentProduct, (state, action): ProductState => {
		return {
			...state,
			currentProduct: action.product
		};
	})
);
```

```TypeScript
//product-list.component.ts
import * as ProductActions from '../state/product.actions';
this.store.dispatch(ProductActions.toggleProductCode());
```

```TypeScript
//product-list.component.ts
productSelected(product: Product): void {
	this.store.dispatch(ProductActions.setCurrentProduct(
		{ product : product } 
		//OR
		{ product } //shorthand syntax
	));
}
```

### Complex scenarios

> When you are loading data from a server

![[Pasted image 20230908224344.png]]

> Effects Workflow
> ng add @ngrx/effects

> (app.module.ts)
> Effects.Module.forRoot([])
> 
> Create product.effects.ts inside of products > state 


![[Pasted image 20230909005037.png]]


![[Recording 20230909005219.webm]]

```TypeScript
import { Actions } from '@ngrx/effects'
import * as ProductActions from './product.actions';

@Injectable()
export class ProductEffects {
	constructor(private actions$: Actions,
				private productService: ProductService
	)
	loadProducts$ = createEffect(() => {
		return this.actions$.pipe(
			ofType(ProductActions.loadProducts),
			mergeMap(action => 
				this.productService.getProducts().pipe(
					map(products => ProductActions.loadProductsSuccess( {products} )),
					catchError(error => of(ProductActions.loadProductsFailure({error})))
				)
			)
		),
	});
}
```
```TypeScript
//product.reducer.ts
//inside of the reducer
...
on(ProductActions.loadProductsSuccess, (state,action): ProductState => {
	return {
		...state,
		products: action.products
	}
})
...
```

![[Pasted image 20230909145608.png]]

```TypeScript
//app.module.ts
EffectsModule.forRoot([])

//feature modules (product.module.ts)
EffectsModule.forFeature([ProductEffets])
```
### A small talk on RxJs

![[Pasted image 20230909151337.png]]

###### Using the effects
![[Pasted image 20230909161750.png]]

###### now handling error

![[Pasted image 20230909162824.png]]
![[Pasted image 20230909162856.png]]

