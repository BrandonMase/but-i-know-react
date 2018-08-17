# Local References
Hold a value of the html element.
Uses HTMLElement or HTMLInputElement (to gain access to the value of the input) as a type.
`app.component.html`
```html
<input 
  type="text"
  #localRef>

<button (click)="useLocalRef(localRef)">
```