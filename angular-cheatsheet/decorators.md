# Decorators
## @Component
  * **selector**: 
    * `app-component` - use a component in a html tag
    * `[app-component]` - use a component in a html attribute.
  * **templateURL** - tells angular where the template file is.
  * **template** - defines a html template inside of the component.
  * **styleURLs** - an array of styles used in.
  * **styles** - used to define styles inside of the component.

```js
@Component({
  selector: 'app-recipe-list',
  templateUrl: './recipe-list.component.html',
  styleUrls: ['./recipe-list.component.css']
})
```

## @Injectable
  Used to inject a service into another service.

`recipe.service.ts`
```js

import { IngredientService } from 'ingredient.service.ts';

@Injectable() 
export class RecipeService {

  constructor(priavte ingredientSerivce: IngredientService) {}

}
```
