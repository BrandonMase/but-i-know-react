# Day 2
## Creating a new component with `ng generate`.
Angular makes it easy to create a new components/services/etc within the Angular CLI. It automatically creates the folder and the file structure that is needed for most type of components. First let's create a new Angular app and call it angular-day-2. I'm really creative. Now with this we want to make a new component. In git bash `cd` into your angular-day-2 folder and type `ng generate component to-do-item`. Angular creates the folder `to-do-item` with this html, css, functionality and, the spec file for you. 
### A quicker way to `ng generate`.
With almost any type of command line there is a faster way to do things. Type `ng g c component-name-here`. `g c` is the quick way to do `generate component`.

## Breaking down the `@Component` decorator
Open up the `to-do-item.component.ts` file. See the `@Component` decorator? This is the way to tell Angular that this class is component and also gives the component some meta data to tell Angular how the component is processed and used in your app. The `@Component` takes in an object and that object tell Angular what to do and where to look for things that make your component run. Let's break down the most common keys you will see in the `@Component`:

* selector - tells Angular what type of selector to use. The one used here is what is called a tag selector. You can write in your .html files `<app-to-do-item></app-to-do-item>` and that will display your `to-do-item` wherever you place that tag. Go place it at the end of the `app.component.html`. We will talk about the other types of selectors later.     
* templateUrl - This tells Angular where to look for your template(.html file). There is another template key called **template**. This lets you place html directly into your component file. It's useful when you want to have a component that doesn't have that much html, you can just place it directly in there and then have one less file to compile later on. 

* styleUrls: This tell Angular where to find the styles for your html of that component. Angular encapsulates CSS meaning, CSS doesn't cascade into other files but only the CSS defined the styleUrls will be loaded into that component. StyleUrls takes in an array so, you can have multiple CSS files that work together inside one component. There is another style key called **style**. Instead of an external CSS file you can write styles directly into the class, much like template.

There are other keys for the `@Component` but, for right now, this are the ones that you need to know. 

## A word about `ng generate`
I forgot to tell you one thing. Head over to the `app.module.ts` file. Every component that we use in our module (app is the only one currently) needs to be imported into the `@NgModule` decorator to be able to use. `ng generate` imports the file and, adds it to the declarations part of `@NgModule`. If you create your own components without the use of `ng generate` you have to import the components yourself. Fair warning. 

## *ngFor to the resuce.
Head over the to `to-do-items/to-do-items.component.ts` file. Make a new variable called `items` and make it an array. Give a couple items for a todo list that you want to accomplish (with the keys being name and hours). So right now our variable looks likes this:
```js
items = [{name: 'keep writing', hours: 2},
{name: 'learn more Angular', hours: 5}]
```
Now how do we loop through all of these to make a list? In React it goes a little something like this: 
```js
render(){
let itemsArr = this.state.items.map((e,i) => {
                return (<div key={i}>
                          <div>{e.name}</div>
                          <div>{e.hours}</div>
                        </div>)
                }); 
      return {
        <div>
          {itemsArr}
        </div>
      }
    }
```
Angular does this much simplar. Go into the `to-do-item.component.html` file. Erase what is there and add this: 
```js
<div *ngFor="let item of items">
  <div>{{ item.name }}</div>
  <div>{{ item.hours }}</div>
</div>
```
What is `*ngFor` doing? Quite simply, this tells Angular to loop through the array variable called items and add the parent div (the one with the `*ngFor` in it) and it's children to the html file for each iteration in in the array. It is basically a `for/in` loop in Javascript but, replacing the in with of. It's saying loop through the variable `items` (the collection) and use the value `item` as the name of the element inside the loop. There isn't much more to it then that.

## Your first foray into Typescript.
So our `items` array is all well and good but, what if we want more control of it? What if we want to make sure the name is a string and the hours is a number? In Javascript, it will take 'sdlfkjsdlkfjsdfsdfjs' happily as an hour and be on it's merry way until you try to do some math on it then, and only then, does it throw a error. If you've ever coded in a language like Java or C++  you know if you do that, it throws an error even before the application runs. This is where Typescript can help.

### More control. More code. Typescript.
Make a new folder called `shared` in the app folder. Make a Typescript file called `item.model.ts`. We are going to make a class for the items we are putting into the items array.

#### Interfaces
Typescript lets you do something called an Interface. An Interface just tells Typescript that with this variable/function/class we expect to have these keys. Interfaces are written like this:

```js
export interface Item {
  name: string;
  hours: number;
  completed?: boolean;
}
```

If Item, whatever is it, is ever used then Typescript looks for a interface  matching that name and checks to make sure the variable/function/class is passed what is supposed to be passed. 

Lets break down what all of this means in the interface:
* `export interface Item` - exports the interface called Item so it can be used in other files.
* `name: string` - This is **not** an object declaration. It may look that way but what this is telling Typescript is that the key `name` should be a string. If `name` is ever passed anything that isn't a string, Angular will throw a warning in your terminal where `ng serve` is running. 
* `hours: number` - works the same way as `name` but telling you that hours should be a number.
* `completed?: boolean` - This one is a little weird. But before I tell you, can you guess what is happening here? The `?:` is telling Typescript that there may be a key called completed but, if it is there check to make sure it's a boolean and if it's not there well, don't worry about it.

One last thing to note about Interfaces. They don't get complied into Javascript. They are just used as a reference for Typescript. An Interface is used for, what Typescript calls, "duck-typing". If it acts like a duck, quacks like a duck... You get the message.

### Creating a class WITH an Interface
So an Interface is all nice and dandy but it doesn't *do* anything for the functionality of our app. It's only used as a reference within Typescript. We *could* write our interface and class like this: 

```js
export interface Item {
  name: string;
  hours: number;
  completed?: boolean;
}

export class Item {
  constructor(name: string, hours: number, completed:? boolean){
    this.name = name;
    this.hours = hours;
    this.completed = completed;
  }
}
```

This...this is far too much code to write. I'm lazy. We can actually completely get rid of the interface and put it inside of the class like this:

```js
export class Item {
  name: string;
  hours: number;
  completed?: boolean;

  constructor(name: string, hours: number, completed:? boolean){
    this.name = name;
    this.hours = hours;
    this.completed = completed;
  }
}
```

Yup that works exactly the same way as before but, we can do better.

```js
export class Item {
  constructor(public name: string, public hours: number, public completed:? boolean){}
}
```

What just happened? How do we get the variables like `name` and `hours`? From the beauty that is Typescript. 

#### Understanding the modifiers
What is this public? Why aren't I assigning `this.name = name`? Typescript enables a shortcut for all of this by using the "public" modifier. The public modifier just tells Typescript that these parameters can be accessed from outside of the class. So we can call `Item.name` and it won't throw a fit about it. When you type public in front of a parameter inside a constructor Typescript automatically assigns this.**parameter** = **parameter** for you. There are two other modifiers:
* private - This says we *can't* access the variable from outside of the class. So calling `Item.name` won't work. We can use it inside the class itself, but everywhere else you can't.
* protected - This is just like private but if a class extends the Item class then it can still use the `name` variable. 

#### Writing the actual class and using it.
Now that we understand what is going on, we can make our class.

```js
export class Item {
  constructor(public name: string, public hours: number){}
}
```

We just got rid of the completed parameter because we won't be using it. Head back to the `to-do-item.component.ts` file. We need to import the class from the `item.model.ts` file, so Typescript knows where to look for the `Item` class. So at the top we 

```js 
import { Item } from '../shared/item.model'
```

Now that is imported we can start using it. Delete the `items` array. We are still going to call the variable `items`. After `items` we put a `: Item[]`. As we learned in the part before this, `: Item[]` just tells Typescript that the `items` variable will be of type `Item`. Adding the `[]` to the end of `Item` just says that it will be an array of type `Item`. Now just add an array of `Item` to the variable just like before but, instead of using objects we will just `Item`.

```js
items: Item[] = [
    new Item('keep writing', 2),
    new Item('learn Angular', 5),
  ];
```
Save your files and reload your app. It's the exact same thing! It works! That was a lot of work for the same thing but, now we have more control over what a `Item` actually is. In larger scale applications this becomes really useful.

## No component is an island 
I wish larger scale applications was as simple as having the array you need in the component you want but, most child components will need to get some information from it's parent component. So what happens when we need to get the array from the parent component? 

### @Input works
So in React, when you have a parent component that needs to pass props to child you do it like this: 

```js
render(){
let itemsArr = this.state.items.map((e,i) => {
                return (<div key={ i }>
                        <ItemComponent item = { e } />
                      </div>)
                }); 
      return {
        <div>
          {itemsArr}
        </div>
      }
    }
```

We aren't using map, we're using `*ngFor` so we need to do things a little differently. Angular needs to know exactly which variables are going to be assigned by another component. That is where `@Input` comes in. Import `Input` from `@angular/core` in the `to-do-item.component.ts` file.

```js
import { Component, OnInit, Input } from '@angular/core';
```

Copy the `items` array into the `app.component.ts` file. Delete the variable in the `to-do-list.component.ts` file and make a variable called `item` with `@Import()` in the front and we want it to be of type `Item`.

```js
@Import() item: Item;
```
We don't need the `[]` anymore because `item` will now only hold one item at a time instead of the entire array. This is where type-checking shines. Because we don't know what will *actually* be passed to the `item` variable, but Typescript now knows that it **needs** to be of type `Item`.

Delete the `<div>` with the `*ngFor` in it from the `to-do-list.component.html` file. That leaves us with this:

```js
  <div>{{ item.name }}</div>
  <div>{{ item.hours }}</div>
```

That's cool but, if we refresh the page we see nothing. `item` has been initalized but it currently not assigned to anything so, we see nothing.

#### We just did something cool and you didn't even know it
Head over the the `app.component.html` file. We only have our `app-to-do-item` in our html file. Now that we have the `items` array inside the app we can `*ngFor` here. So add a `*ngFor` in the `app-to-do-list`.

```js
<app-to-do-item
  *ngFor="let singleItem of items">
</app-to-do-item>
```

Refresh the page. What happened? Everything still blank? Yup but, we looped through the `app-to-do-item` with all of the items inside the `items` array. We're getting somewhere. 

What is happening is that, we're looping through the `items` array but we aren't passing anything to the `to-do-item`. Now remember how we had an `@Import()` for the item in the `to-do-item` component? What we made is actually a property binding for the property `item`. We need to add this to the `app-to-do-item` in the `app.component.ts` file. And what will the property be equal to? The `singleItem` variable that we are looping through in our `*ngFor`.

```js
<app-to-do-item
*ngFor="let singleItem of items"
[item]="singleItem"></app-to-do-item>
``` 

We did it! Back to square one! But what if we need to pass something from our child component backwards to our parent component?

### @Output works.
In React, there is the idea of unidirectional data flow. Information can only flow downwards, sort of. To pass a variable back from from a child component to a parent component you do something like this:

`app.js`
```js

addItem = (item) => {
  let items = this.state.items.slice();
  items.push(new Item( item.name, item.hours ));

  this.setState({ items:items });
}

render(){
      let itemsArr = this.state.items.map((e,i) => {
                return (<div key={ i }>
                        <ItemComponent item = { e } addItem = { this.addItem } />
                      </div>)
                }); 
      return {
        <div>
          {itemsArr}
        </div>
      }
    }
```

`items.js`

```js
render(){
  return{
    <div>
      <input type="text" onChange={(e) => this.setState({ name: e.target.value })} />
      <input type="number" onChange={(e) => this.setState({ hours: +e.target.value })} />
      <button onClick={() => this.props.addItem({ name: this.state.name, hours: this.state.hours })} />Add Item </button>
    </div>
  }
}
```

In Angular, it's different obviously. Let's make a new component called input:

```js
ng g c input
```

Now that we have the new input component add it to the top of `app.component.html` file. After checking that the `input` component works, we need to actually do something with it. Head over the the `input.component.html` file and add two inputs and a button to so we can add the new item to the `items` array in the app.

```js
<input type="text">
<input type="number">
<button>Add</button>
```
Now that we have the the inputs and the button, we need to do something with them. 

#### Local References
At the end of the inputs add a `#reference-name-here`.
```js
<input type="text" #name>
<input type="number" #hours>
<button>Add</button>
```
What this is does is let you reference a HTML element from inside your template. It references the whole HTML element and all of it's underlying properties.

#### Let's see Local References in use.
We need to add something to the click event of the button. Let's add a function called `addItem()` and have `name` and `hours` as arguments to the button.

```js
<button (click)="addItem(name,hours)">
```

So with this we are passing the local references name and, hours to the function.

Go into the `input.component.ts` file and create a new function called `addItem()`. Let's console log the name to see what we get back.

```js
addItem(name,hours) {
    console.log(name);
  }
```
Now add something to the inputs and click Add. When you open up your console log you should see the input that is assosiated with name. Ok so we know it's an HTML element but we should really tell Typescript what kind of variables `name` and `hours` are. These two variable are of the type `HTMLInputElement`.

```js
  addItem(name: HTMLInputElement, hours: HTMLInputElement) {
    console.log(name);
  }
```

`HTMLInputElement` gives you an extra property that the input has, value. Value is what we need to get the *value* of the input. If you console log `name.value` we should get back whatever we typed into the input. 

Ok so we can get our local references into our function we need to get them out of our component. Before the constructor we are gonna make a new variable called `item`. We need to put `@Output` before `item`. Remember to import `Output` from `@angular/core`.

```js
@Output() item
```

Now how do we tell the rest of the app that this variable can be used somewhere else? With `EventEmitter`.

##### EventEmitter
`EventEmitter` is literally just a way, when an event happens, that event screams out like, "HEY I'M HERE FOR ANYBODY THAT CARES!", like a lighthouse off in the distance. So let's add the screaming event to our `item`.

```js
@Output() item = new EventEmitter<{name: string, hours: number}>();
```

So basically we are saying that item is an event that emits an object with the keys name and hours. `EventEmitter` looks wierd right? What are with the angled brackets (<>)? This is called a Generic.

### Generics 
Generics are weird when you first start looking them. Basically what they are, is to create a component that can work over many different types. With the class Generic we're using we are just saying when we emit a `item` event, we are going to emit an object that has a name and hours with their respective types. There is way more to Generics but, we'll go over it later when the complexity of Generics comes into play.

### Now we have our emitter, what do we do with it?
So we have the pieces to emit an event. Go into the `addItem` function. We have a event called `item` what do you think we do with it? 

```js
addItem(name: HTMLInputElement, hours: HTMLInputElement) {
    this.item.emit({name: name.value, hours: +hours.value});
  }
```

This is code that makes the lighthouse come on. We emit to the rest of the app that we have a object with a `name` and `hours` in it. A quick word about the `+hours.value`. Since we defined the type of `hours` to be a number and an input only returns a string, we use the Unary plus operator to convert the string into a number. It's just a quicker way then using `parseInt()`.

Great, so we're emitting the `item` on click. How do we get `app` to listen for it?

Head into the `app.component.html` file. We have our `app-input` already displaying in the `app` template. So we created a event called item, which is now an event binding in the `app-input` so how do you think we use the `item` event?

```js 
<app-input (item) = "onAddItem($event)"></app-input>
```

When we click the Add button, it emits the `item` event, which in turn will run the `onAddItem` with the `item` event as a argument. `$event` is what Angular captures as the event.

Go into the `app.component.ts` file. Let's add a `onAddItem` function.

```js
  onAddItem(event: {name: string, hours: number}) {
    this.items.push(new Item(event.name, event.hours));
  }
```

So we're going to be taking an event that is an object with `name` and `hours` as keys. Then we create a new `Item` class and push it into the `items` array. The cycle is complete.






