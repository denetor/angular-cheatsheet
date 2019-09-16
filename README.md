# angular-cheatsheet
Angular 2+ cheatsheet

## Command line

### Create application
```
ng new <appname>
```

### Generate new component
```
ng gc <componentname>
```

## Interpolation

// test.component.ts
```
export class TestComponent {
    public name = 'World';
    
    getName() {
        return this.name;
    }
}
```

// test.component.html
```
<p>Hello {{ name }}</p>
<p>{{ name }} is {{ name.length }} chars long.</p>
<p>{{ name.toUpperCase() }} can be uppercase.</p>
<p>{{ getName() }}</p>
```

## Property interpolation
// test.component.ts
```
export class TestComponent {
    public elementId = 'nameInput';
    public isDisabled = true;
}
```

// test.component.html
```
<input [id]="elementId" type="text">
<input id="{{ elementId }}" type="text">
<input bind-id="elementId" type="text">

<input [disabled]="isDisabled" type="text">
<input bind-disabled="isDisabled" type="text">
```

## Class binding
// test.component.ts
```
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

// test.component.html
```
<p [class]="elementClass">Some message</p>

<!-- this applues text-fanger class if hasError returns true -->
<p [class.text-danger]="hasError">Danger text</p>

<!-- this applies the classes that evaluates true -->
<p [ngClass]="objClasses">Has multiple classes</p>
```

## Style binding
It's similar to class binding

// test.component.ts
```
export class TestComponent {
    public mutedColor = "#aaaaaa";
    public paragraphStyles = {
       color: 'blue',
       fontStyle: 'italic',
    };
}
```

// test.component.html
```
<p [style-color]="'red'">Some message</p>
<p [style-color]="mutedColor">Some muted message</p>
<p [ngStyle]="paragraphStyles">Some muted message</p>
```
