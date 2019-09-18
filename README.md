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
