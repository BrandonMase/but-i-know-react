# Directives
## Attribute Directive
These sit on an element and affect only that element.

* `[ngClass]` - Dynamically add a class to an element.
```html
<p [ngClass]="{odd: odd % 2 !== 0}">
```

* `[ngStyle]` - Dynamically add styles to an element.
```html
<p [ngStyle]="{background-color: odd % 2 !== 0 ? 'yellow' : 'transparent' }>
```

## Custom Attribute Directives
Add directive into the module declarations

`app.directive.ts`
```js
import { Directive, 
ElementRef, 
OnInit, 
Renderer2, 
HostListener, 
HostBinding,
Input() } from '@angular/core';

@Directive ({
  selector: '[appDirective]'
})

export class AppDirective implements ngOnInit {
  //used to bind to a property
  @Input() defaultColor = 'transparent';
  @Input() highlightColor = 'green'

  //Can also use this to bind to appDirective as a property
  @Input('[appDirective]') highlightColor = 'green';
  @HostBinding('style.backgroundColor') backgroundColor: string;

  constructor(private renderer: Renderer2, private elementRef: ElementRef) {}

  ngOnInit() {
    //Don't use this. Angular can render without a DOM so sometimes it wont' work.
    this.elementRef.nativeElement.style.backgroundColor = 'green'

    this.backgroundColor = this.defaultColor;
  }

  //Used to react to an event that occurs on that element.
  @HostListener('mouseenter') mouseover(eventData: Event) {
    this.backgroundColor = this.highlightColor;
  }

  @HostListener('mouseleave') mouseover(eventData: Event) {
    this.backgroundColor = this.defaultColor;
  }
}

```

`app.component.html`
```html
<p 
  appDirective
  [defaultColor]="'yellow'"
  [highlightColor]="'blue'"
>

//use this if you binded the appDirective to a property
<p
  [appDirective]="'blue'"
  [defaultColor]="'yellow'"
>
This will be styled blue on hover.
</p>
```

## Structural Directive
These sit on an element and affects the whole DOM. Can only have one structural directive on a single element.

`*ngFor` - loop through an array to display html for each array element.

```html
<div *ngFor="let element of array; let i = index">
  // Everything that will be displayed for the *ngFor
</div>
```

`*ngIf` - displays child html tags if *ngIf return true.

```html
<div *ngIf="foo">
  //Everything that will be displayed if foo === true 
</div>
```

`*ngSwitch` - displays something based on the value passed in.

```html 
<div *ngSwitch="10">
  <p *ngSwitchCase="5">5</p>
  <p *ngSwitchCase="10">10</p>
  <p *ngSwitchCase="100">100</p>
  <p *ngSwitchDefault>Default</p>
</div>
```

## Custom Structural Directive
Remember to add the directive to the module declarations.

`appStruc.directive.ts`

```js
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appUnless]'
})

export class AppStrucDirective {
  // the set keyword turns a property into a method that gets executed
  // everytime the property changes
  // The property and the directive name has to be the same.
  @Input() set appUnless(condition: boolean) {
    if(!condition) {
      //this just adds the template to the view container
      this.vcRef.createEmbeddedView(this.templateRef)
    } else {
      //this clears the view container of any DOM elements we passed to it.
      this.vcRef.clear();
    }
  }

  //TemplateRef gives us access to the template
  //ViewContainerRef gets the reference to where the template should be rendered.
  constructor(private templateRef: TemplateRef<any>, private vcRef: ViewContainerRef) {}
}

```

`app.component.html`

```html
<div *appUnless="onlyOdd">
  <li *ngFor="let num of numbers">
    <p>{{ num }}</p>
  </li>
</div>

```