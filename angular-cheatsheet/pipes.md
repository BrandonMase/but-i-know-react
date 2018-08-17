# Pipes
Transforms output in a template.
Remember to add the the pipe to the declarations in the module.

```html
<p> {{ username | uppercase }}</p>
```

##Custom Pipes
```ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'shorten',
  //pure: false means it will always rerender the output on change.
  pure: false,
})
export class ShortenPipe implements PipeTransform {
  // The first is the value that is passed into the pipe
  // The second parameter is optional but can be used with pipe-name: second-parameter
  //seperate additional parameters with an extra colon
  // pipe-name: second-param: third-param
  transform(value: any, limit: number) {
    if (value.length > limit) {
      return value.substr(0, limit) + ' ...';
    } else {
      return value;
    }

  }
};
```

```html
<p>{{ username | shorten: 15}}</p>
```