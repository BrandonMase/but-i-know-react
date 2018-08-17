# Routing

## appRoutes
Remember to import `RouterModule.forRoot(appRoutes)` into `app.module.ts`.

Use /*route* for absolute routes and *route* for relative pathing.

`app-routing.module.ts`

```ts
const appRoutes: Routes = [
  { path: '/users', component: UsersComponent, children: [
    { path: ':id', component: UserComponent }
  ]}, 
  { path: '/servers', 
  component: ServersComponent,

  //run before a route is loaded to see if a user can access route. 
  canActivate: [AuthGuard],
  //same as canActivate but, used to limit access to child routes.
  canActivateChild: [AuthGuard],

  //make sure something happens before the user clicks off the component.
  canDeactivate: [CanDeactivateGuard], }
  //used to pass data to that component on load.
  //this.route.data.subscribe()
  {path: 'not-found', 
  component: ErrorPageComponent, 
  data: {message: 'Page not found'}},
  //Used when angular can't find a route path then redirects somehwere
  {path: '**', redirectTo: '/not-found' }
];

@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes)
  ],
  exports: [RouterModule]
})
export class AppRoutingModule {

}


```

`app.component.html`

```html
<a 
  //The route the link goes to seperated in an array
  //So this route would be /users/id
  [routerLink]="['/users', id]"

  //Object of all of the query strings to pass. 
  [queryParams]="{ allowEdit: true }"

  //# at the end of the route. Can only be 1 string. 
  [fragment]="loading"

  //Add class when route path is active.
  [routerLinkActive]="active"
>

// This is where the routes will be rendered.
<router-outlet></router-outlet>
```

`app.component.ts`

```ts
import { ActivatedRoute, Params, QueryParams, Fragment, Data } from '@angular/router';

...
server: {id: number, name: string, status: string};

constructor(private route: ActivatedRoute, private router: Router) {}

ngOnInit() {
  //You have to get the information from the snapshot first then
  //subscribe for the rest of the changes
  this.server.name = this.route.snapshot.params.name;
  
  this.route.params.subscribe(
    (params: Params) => {
      this.server.name = params.name;
    }
  )
}

onEdit() {
  this.router.navigate(['edit'], 
    //You need this to be able to navigate relative to the path you're on.
    {relativeTo: this.route,

    //preserve, preservers all of the queryParams from the
    //previous route. We get rid of any new queryParams.

    //merge will merge the previous and the new queryParams.
    queryParamsHandling: 'preserve'})
}


```

## Auth Guard
```js
import { AuthService } from './auth.service';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router, CanActivateChild } from '@angular/router';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate, CanActivateChild {
  constructor(private authService: AuthService, private router: Router) {}
  
  canActivate(route: ActivatedRouteSnapshot,
              state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean  {
                return this.authService.isAuthenticated()
                  .then(
                    (authenticated: boolean) => {
                      if (authenticated) {
                        return true;
                      } else {
                        this.router.navigate(['/']);
                        return false;
                      }
                    }
                  );
  }

  canActivateChild(route: ActivatedRouteSnapshot,
                  state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean  {
                    return this.canActivate(route, state);
}
}
```