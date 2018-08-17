# Databinding

## Property Binding

Property binding is when you bind a specific property of a html tag.

```html
// Changes the value of the input to foo.name
<input [value]={{ foo.name }}/>
```

## Custom Property Binding

This is used when passing props from parent to child components.

`app.component.ts`
```js
import { Input } from '@angular/core';
...
export class AppComponent {
 @Input() element = {name: string, type: string}
}
```

`app.component.html`
```html
<app-component
  *ngFor="let newElement of elements"
  [element]="newElement" />
```

### Notes
@Input() can use aliases for it's name. e.g. 
`@Input('newElement')`

```html
<app-component
  *ngFor="let newElement of elements"
  [newElement]="newElement" />
```

## Event Binding
Event binding is when you bind to a specific event on a html tag.

```html
//On click the button will run the addItem() function
<button (click)="addItem()">Add Item</button>
```

## Custom Event Binding
Used to pass props from child to parent.

`child.component.ts`
```js
import { Output, EventEmitter } from '@angular/core';
...
export class AppComponent {
  @Output() passFromChildToParent = new EventEmitter<string>();

  outputFunc() {
    //Emit the data on a function 
    this.passFromChildToParent.emit('emit something');
  }
}
```
`child.component.html`
```html
<button (click)="outputFunc()">Emit</button>
```

`parent.component.html`
```html
//The passFromChildToParent EventEmitter is emitted when the
//outputFunc is run.
<app-child-component
  (passFromChildToParent)="parentFunc($event)"/>

<button>
```

`parent.component.ts`

```js

parentFunc(event) {
  //Do whatever you want with the event data.
  console.log(event)
} 
```




## Two way databinding
Two way databinding is when you use property and event binding in one directive.

```html
//This will initalize the value of input to foo (a variable in the component)
// It will also change the variable foo to whatever the user inputs into the field.
<input [(ngModel)]="foo">

//This is also the same as doing
<input [value]="foo" (input)="fooChange($event.target.value)">
```
Remember to import `FormsModule` from `@angular/forms` in the module to be able to use `ngModel`.