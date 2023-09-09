
### What are attribute directives

- Directives are classes that add additional behavior to elements in your Angular applications. Use Angular's built-in directives to manage forms, lists, styles, and what users see. Attribute directives listen to and modify the behavior of other HTML elements, attributes, properties, and components.

### Adding directives to component

```javascript
/* Part 1 */
@Component({
  selector: 'admin-menu',
  template: 'admin-menu.html',
  hostDirectives: [MenuBehavior],
})
export class AdminMenu { }

/* Part2 */
@Component({
  selector: 'admin-menu',
  template: 'admin-menu.html',
  hostDirectives: [{
    directive: MenuBehavior,
    inputs: ['menuId'],
    outputs: ['menuClosed'],
  }],
})
export class AdminMenu { }

/* html code */
<admin-menu menuId="top-menu" (menuClosed)="logMenuClosed()">
```
- **Angular applies host directives statically at compile time.** You cannot dynamically add directives at runtime.
- **Directives used in `hostDirectives` must be `standalone: true`.**
- **Angular ignores the `selector` of directives applied in the `hostDirectives` property.**

###### Aliasing input output name

```javascript
@Component({
  selector: 'admin-menu',
  template: 'admin-menu.html',
  hostDirectives: [{
    directive: MenuBehavior,
    inputs: ['menuId: id'],
    outputs: ['menuClosed: closed'],
  }],
})
export class AdminMenu { }

/* html code */
<admin-menu id="top-menu" (closed)="logMenuClosed()">
```

###### Adding directives to another directives

```javascript
@Directive({...})
export class Menu { }

@Directive({...})
export class Tooltip { }

// MenuWithTooltip can compose behaviors from multiple other directives
@Directive({
  hostDirectives: [Tooltip, Menu],
})
export class MenuWithTooltip { }

// CustomWidget can apply the already-composed behaviors from MenuWithTooltip
@Directive({
  hostDirectives: [MenuWithTooltip],
})
export class SpecializedMenuWithTooltip { }
```

- Host directives go through the same lifecycle as components and directives used directly in a template. However, host directives always execute their constructor, lifecycle hooks, and bindings _before_ the component or directive on which they are applied.

### Order of execution

```javascript
@Component({
  selector: 'admin-menu',
  template: 'admin-menu.html',
  hostDirectives: [MenuBehavior],
})
export class AdminMenu { }
```

The order of execution here is:
1. `MenuBehavior` instantiated
2. `AdminMenu` instantiated
3. `MenuBehavior` receives inputs (`ngOnInit`)
4. `AdminMenu` receives inputs (`ngOnInit`)
5. `MenuBehavior` applies host bindings
6. `AdminMenu` applies host bindings

- This order of operations means that components with `hostDirectives` can override any host bindings specified by a host directive.

```javascript
@Directive({...})
export class Tooltip { }

@Directive({
  hostDirectives: [Tooltip],
})
export class CustomTooltip { }

@Directive({
  hostDirectives: [CustomTooltip],
})
export class EvenMoreCustomTooltip { }
```

In the example above, the order of execution is:
1. `Tooltip` instantiated
2. `CustomTooltip` instantiated
3. `EvenMoreCustomTooltip` instantiated
4. `Tooltip` receives inputs (`ngOnInit`)
5. `CustomTooltip` receives inputs (`ngOnInit`)
6. `EvenMoreCustomTooltip` receives inputs (`ngOnInit`)
7. `Tooltip` applies host bindings
8. `CustomTooltip` applies host bindings
9. `EvenMoreCustomTooltip` applies host bindings

### Dependency Injection (Not sure what it meant)

A component or directive that specifies `hostDirectives` can inject the instances of those host directives and vice versa.

When applying host directives to a component, both the component and host directives can define providers.

If a component or directive with `hostDirectives` and those host directives both provide the same injection token, the providers defined by class with `hostDirectives` take precedence over providers defined by the host directives.

### Performance

When you have too many hostDirectives then you will probably face performance issues because you are loading a lot of objects.

```javascript
@Component({
  hostDirectives: [
    DisabledState,
    RequiredState,
    ValidationState,
    ColorState,
    RippleBehavior,
  ],
})
export class CustomCheckbox { }
```
- It will create like 6 objects 5 for the hostDirectives and 1 for the component. If your page has a 100+ checkboxes the page will become very slow so use it with caution.
