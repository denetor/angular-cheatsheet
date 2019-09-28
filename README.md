# angular-cheatsheet
Angular 2+ cheatsheet

## Command line

### Create application
```bash
ng new <appname>
```

### Generate new component
```bash
ng gc <componentname>
```

## Interpolation
```ts
// test.component.ts
export class TestComponent {
    public name = 'World';
    
    getName() {
        return this.name;
    }
}
```

```html
<!-- test.component.html -->
<p>Hello {{ name }}</p>
<p>{{ name }} is {{ name.length }} chars long.</p>
<p>{{ name.toUpperCase() }} can be uppercase.</p>
<p>{{ getName() }}</p>
```

## Property interpolation
```ts
// test.component.ts
export class TestComponent {
    public elementId = 'nameInput';
    public isDisabled = true;
}
```

```html
<!-- test.component.html -->
<input [id]="elementId" type="text">
<input id="{{ elementId }}" type="text">
<input bind-id="elementId" type="text">

<input [disabled]="isDisabled" type="text">
<input bind-disabled="isDisabled" type="text">
```

## Class binding
```ts
// test.component.ts
export class TestComponent {
    public elementClass = "text-success";
    public hasError = true;
    public objClasses = {
        "text-danger": hasError,
        "text-success": !hasError,
        "text-special: true,
    };
}
```

```html
<!-- test.component.html -->
<p [class]="elementClass">Some message</p>

<!-- this applues text-fanger class if hasError returns true -->
<p [class.text-danger]="hasError">Danger text</p>

<!-- this applies the classes that evaluates true -->
<p [ngClass]="objClasses">Has multiple classes</p>
```

## Style binding
It's similar to class binding

```ts
// test.component.ts
export class TestComponent {
    public mutedColor = "#aaaaaa";
    public paragraphStyles = {
       color: 'blue',
       fontStyle: 'italic',
    };
}
```

/```html
<!-- test.component.html -->
<p [style-color]="'red'">Some message</p>
<p [style-color]="mutedColor">Some muted message</p>
<p [ngStyle]="paragraphStyles">Some muted message</p>
```

## Event binding
```html
<!-- test.component.html -->
<button (click)="onClick()"></button>
<button (click)="onClickWithEvent($event)"></button>
```

```ts
// test.component.ts
export class TestComponent {
    onClick() {
        // do something
    }
    
    onClickWithEvent(event) {
        console.log(event);
    }
}
```

## Template reference variables
```html
<!-- test.component.html -->
<input #myInput type="text">
<button (click)="onClick(myInput.value)"></button>
```

```ts
// test.component.ts
export class TestComponent {
    onClick(s) {
        console.log(s);
    }
}
```

## Two way binding
```html
<!-- test.component.html -->
<input [(ngModel)]="name" type="text">
Hello {{ name }}
```

```ts
// test.component.ts
export class TestComponent {
    public name = 'Denetor';
}
```

But we have to import the forms module
```ts
// app.module.ts
import { FormsModule } from '@angular/forms';

@NgModule({
    imports: [
        FormsModule
    ]
})
```

## Structural directives

### ngIf
```html
<!-- test.component.html -->
<div *ngIf="hasError()">You have an error</div>

<div *ngIf="name.length > 0; then hasNameBlock; else noNameBlock"></div>
<ng-template #hasNameBlock>Hello {{ name }}!</ng-template>
<ng-template #noNameBlock>Hello unnamed!</ng-template>
```

### ngSwitch
```html
<!-- test.component.html -->
<div [ngSwitch]="alertLevel">
    <span *ngSwitchCase="'red'">Red alert</span>
    <span *ngSwitchCase="'yellow'">Yellow alert</span>
    <span *ngSwitchCase="'green'">No alert</span>
</div>
```

## ngFor
```html
<!-- test.component.html -->
<div [ngFor]="let name of names">
    {{ name }},
</div>

<div [ngFor]="let name of names; index as i"> <!-- even as e; odd as o; ... -->
    {{ i }}: {{ name }}<br />
</div>

<div [ngFor]="let name of names; last as lastItem">
    {{ name }}<span *ngIf="!lastItem">,</span>
</div>
```

```ts
// test.component.ts
export class TestComponent {
    public names = ['Alice', 'Bob', 'Charlie', 'Dan'];
}
```

## Components interaction

## Sending parameters to a child component
```ts
// app.component.ts
export class AppComponent {
    public name = 'Alice';          // parameter to pass to TestComponent
    public lastName = 'Smith';      // another parameter
}
```

```html
<!-- app.component.html -->
<test-component [nameFromApp="name"] [lastName="lastname"]></test-component>
```

```ts
// test.component.ts
import { ..., Input } from '@angular/core';
export class TestComponent {
    @Input() public nameFromApp;            // by default the parameter has the property name
    @Input('lastName') public familyName;   // internally I can use a different name then the parameter name
}
```

```html
<!-- test.component.html -->
<p>Hello, {{ nameFromApp }} {{ familyName }}!</p>
```

### Sending data to parent component
```ts
// test.component.ts
import { ..., Output, EventEmitter } from '@angular/core';
export class TestComponent {
    @Output() public childEvent = new EventEmitter();
    
    fireEvent() {
        this.childEvent.emit('Hi app!');
    }
}
```

```html
<!-- test.component.html -->
<button (click)="fireEvent()">Send data to App</button>
```

```html
<!-- app.component.html -->
<test-component (childEvent)="messsage=$event"></test-component>
Message received from TestComponent: {{ message }}
```

## Pipes
Used to transform output in a template. Each one can have more parameters
```html
<!-- test.component.html -->
{{ name | uppercase }} <!-- lowercase; titlecase; ... }}
{{ 'abcdefg' | slice:3:5 }} <!-- cd -->
{{ someObject | json }} <!-- { property: value, ... } -->
{{ 1.2345 | number:'2.2-3' }} <!-- '01.234' (integers, minDecimals, maxDecimals) -->
{{ 0.25 | percent }}
{{ 320.25 | currency : 'EUR' }}
{{ birthDate | date:'short' }}
```

## Services
A service isa a class usad to share data, implementin application logic or connect to external services.

### 18. Dependency injection
It's used to inject dependency in a class instead of let them being defined by the class itself.

Class without DI:
```ts
class Car {
    engine;
    tyres;
    constructor() {
        this.engine = new Engine();
        this.tyres = new Tyres();
    }
}
```

Class with DI:
```ts
class Car {
    engine;
    tyres;
    constructor(engine, tyres) {
        this.engine = engine();
        this.tyres = tyres();
    }
}
```

## 19. Using a service
To create a service, use the console
```bash
ng c s test
```

```ts
// app.module.ts
import { TestService  } from '.test.service';
    ...
    providers: [TestService],
    ...
```

Then use it in a component
```ts
// test.component.ts
export class TestComponent {
    contructor (private _testService: TestService) {
    ngOnInit() {
        let a = this._testService.getSomething();
    }
}
```

## 20 HTTP and Observables
Sometimes you need to make a component subscribe to a service in order to fetch it via a http call and then display it in the component's view.

First import http module in the app.
```ts
// app.module.ts
import { HttpClientModule  } from '@angular/common/http';
    ...
    imports: [HttpClientModule],
    ...
```

The use http module in the service
```ts
// test.service.ts
import { HttpClient } from '@angular/common/http';
import { IMyData } from './mydata';
import { Observable } from 'rxjs/Observable';

    constructor(private http: HttpClient) {
    }
    
    getSomeData():Observable<IMyData[]> {
        // this cast will return an observable of type array containing IMyData data
        return this.http.get<IMyData[]>('https://serverurl.domain/servicepath');
    }
```

Now let's create an interface file for the data returned by the observable
```ts
// mydata.ts
export interface IMyData {
    id: number,
    name: string
}
```

Now we are ready to subscribe the Observable from a component
```ts
// test.component.ts
    ngOnInit() {
        this._testService.getSomething().subscribe(data => this.myData = data);
    }
```

### Error handling
We need to catch errors fetching data.
```ts
// test.service.ts
import 'rxjs/add/operator/catch';
import 'rxjs/add/observable/throw';

    getSomeData():Observable<IMyData[]> {
        return this.http.get<IMyData[]>('https://serverurl.domain/servicepath')
                        .catch(this.errorHandler);
    }
    
    errorHandler(error: HttpErrorResponse) {
        return Observable.throw(error.message || 'Generic error');
    }
```

And manage the error
```ts
// test.component.ts
    ngOnInit() {
        this._testService.getSomething().subscribe(
            data => this.myData = data,
            error => this.errroMsg = data
        );
    }
```

## 23. Routing
```ts
// app-routing.module.ts
import { MyComponent } from './my/my.component';
const routes: Routes = [
  { path: 'myroute', component: MyComponent },              // catch myserver:port/myroute
  { path: 'user/:id', component: UserDetailComponent },     // route with detail
  { path: '**', component: PageNotFoundComponent }          // catch every other path
];
```

### ParamMap Observable
If you try to change a route parameter, the browser URL changes, but the page remains at the previous page.
Example:
```ts
// user-detail.module.ts
import { ActivatedRoute, Router, ParamMap } from '@angular/router';
ngOnInit() {
    let id = parseInt(this.route.snapshot.paramMap.get('id'));  // read route parameter
    this.userId = id;
}
gotoNext() {
    let nextId = this.userId + 1;
    this.router.navigate(['/users', nextId]);       // this will not work if i'm already in a /users/:id route
}
```

I must use an Observable:
```ts
// user-detail.module.ts
import { ActivatedRoute, Router, ParamMap } from '@angular/router';
ngOnInit() {
    // this.router.paramMap returns an Observable
    this.router.paramMap.subscribe((params: ParamMap) => {
        let id = parseInt(params.get('id');
        this.userId = id;
    });
}

gotoNext() {
    let nextId = this.userId + 1;
    this.router.navigate(['/users', nextId]);       // this will not work if i'm already in a /users/:id route
}
```

### Optional route parameters
To have an optional route parameter, thr route declaration can remain the same, we read the parameter via paramMap like the previous example.


