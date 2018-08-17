# ng-content
Passes whatever is inside ng-content into the child component.
`parent.component.html`

```html
<app-child-component>
  <ng-content>
    //content you want to pass into the child
  </ng-content>
</app-child-component>
```

`child.component.html`

```html
<div>
  <ng-content></ng-content>
</div>
```