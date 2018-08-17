# Lifecycles

* ngOnChanges - Called after a bound input property changes.
* ngOnInit - Called once the component is initalized. Runs after the constructor
* ngDoCheck - Called during every change detection run.
* ngAfterContentInt - Called after content (ng-content) has been projected into view.
* ngAfterContentChecked - Called every time the projected content has been checked.
* ngAfterViewInt - Called after the component's view (and child views) has been initialized. Can get access to template elements after this runs.
* ngAfterViewChecked - Called everytime the view (and child views) has been checked.
* ngOnDestory - Called once the component is about to be destoryed.