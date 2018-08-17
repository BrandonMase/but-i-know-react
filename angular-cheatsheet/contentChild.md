# ContentChild
Used when you want to access to a local reference inside of ng-content

`parent.component.html`

```html
<input
  type="text"
  #localRef>
```

`child.component.ts`

```js
import { ContentChild } from '@angular/core';

@ContentChild('localRef') localRef: ElementRef

ngAfterContentInit() {
  //Has to be run after ngAfterContentInit to get access to the element.
  console.log(this.localRef.nativeElement)
}