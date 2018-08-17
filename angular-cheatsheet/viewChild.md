# View Child
Used to gain access to a element before it is passed in a function
Uses type `ElementRef`
Don't change the element within ViewChild. Use rendererV2 instead.



`app.component.html`

```html
<input
  type="text"
  #localRef
/>
```

`app.component.ts`

```js
import { ViewChild } from '@angular/core';
...
//pass the name of the local reference into view child
//If you want to pass a component don't use a string.
@ViewChild('localRef') localRef: ELementRef;

useLocalRef() {
  //Gets access to the actual element.
  console.log(this.localRef.nativeElement)
}

```