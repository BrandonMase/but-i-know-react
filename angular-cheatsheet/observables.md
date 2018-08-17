# Observables

## Custom Observables

### Observable
```js
const myObservable = Observable.create((observer: Observer<string>) => {
      setTimeout(() => {
        // next pushes the next data package
        observer.next('first package');
      }, 2000);


      setTimeout(() => {
        observer.next('second package');
      }, 4000);


      setTimeout(() => {
        //error send an error
        observer.error('this does not work');
        //this completes the observable.
        observer.complete();
      }, 5000);

      setTimeout(() => {
        observer.next('third package');
      }, 6000);

    });

```

### Observer
```js
    this.customObsSubscription  = myObservable.subscribe(
      //What to do on next.
      (data: string) => { console.log(data); },
      //What to do on error.
      (error: string) => { console.log(error); },
      //What to do on complete.
      () => { console.log('completed'); },
    );
```

### Subject
Subject are observer and observable together.

```js
userActivated = new Subject();
```

```js 
this.usersService.userActivated.subscribe(
  (id: number) => {
    if (id === 1) {
      this.user1Activated = true;
    } else if (id === 2) {
      this.user2Activated = true;
    }
  }
);
```

