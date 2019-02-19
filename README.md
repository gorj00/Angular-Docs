# Angular 7+ Cheat Sheet

## # Components Databinding (Communication)
Binding to custom properties and biding to custom events is suitable for input and output, for more complex component communcation use **_services_**.
### ## Binding to Custom Properties
#### Component Informed by PARENT Component
**1.a. step** 
  - In child component **.ts file**, define property with assigned type as a javascript object (or any other type), 
  - add decorator **@Input()** (don't forget the braces) before property name, 
  - import **Input** (without braces) from Angular core at the beginning of the file:
```typescript
import { Component, OnInit, Input } from '@angular/core';

// more TypeScript code

@Input() element: {type: string, name: string, content: string};
```
**1.b. step** _(optional)_
  - Assign **ALIAS**: Add alias name inside braces:
```typescript
@Input('srvElement') element: {type: string, name: string, content: string};
```
**2. step** 
  - In parent component **.ts file**, assign values to Javascript object literal:
```typescript
serverElements = [{type: 'server', name: 'Testserver', content: 'Just a test'}];
```
**3.a. step** 
  - In parent component **.html file**, bind the property to HTML element:
```html
<app-server-element [element]="serverElement"></app-server-element>
```
**3.b. step** _(optional)_ 
  - If alias added in 1.b., bind the property to HTML element using the alias:
```html
<app-server-element [element]="srvElement"></app-server-element>
```


### ## Binding to Custom Events
#### Component Informed by CHILD Component
We want to "listen" to some events, for example, we want to inform parent component that new server was created. 

**1. step** 
  - In parent component **.ts file**, define function(s):
```typescript
// After we've clicked and new server was added
onServerAdded(serverData: {serverName: string, serverContent: string}) {
	this.serverElements.push({
    	type: 'server',
      	name: serverData.serverName,
      	content: serverData.serverContent
    });
}
```
**2.a. step** 
  - In parent component **.html file**, bind the event to HTML element:
```html
<app-cockpit (serverCreated)="onServerAdded($event)"></app-cockpit>
```
**2.b. step**  _(ptional)_ 
  - With alias (name of alias from _3.b._):
```html
<app-cockpit (srvCreated)="onServerAdded($event)"></app-cockpit>
```
**3.a. step** 
  - Defigning **EVENT EMITTER:** In child component **.ts file**, define new property informing about the event by assigning to it **new EventEmitter<_define type of event data_>()** (a generic type) with a constructor **()** at the end,
  - import **EventEmitter** from Angular core at the beginning of the file. 
  - add decorator **@Output()** (don't forget the braces) before property name, 
  - import **Output** (without braces) from Angular core at the beginning of the file:
```typescript
import { Component, OnInit, EventEmitter, Output } from '@angular/core';

// more TypeScript code

@Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```
**3.b. step** _(optional)_ 
  - Assign **ALIAS:** Add alias name inside braces:
```typescript
@Output('srvCreated') serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```
**4. step** 
  - Calling **EVENT EMITTER:** In child component **.ts file**, in a method, call and emit the property with  **.emit()**,
  - put inside contracted current values of parameters (by using **this**):
```typescript
// When creating new server
onAddServer() {
  this.serverCreated.emit({serverName: this.serverName, serverContent: this.serverContent});
}
```

## # View Encapsulation
Styles are applicable only isnide the concrete components by default. Meaning, styles defined in component **.css file** will not affect the style of other components.  

It is due to Angular generating unique properties **_ngcontent-ejo-x** to HTML elements belonging to the concrete component.

- Example: 
```html
<div _ngcontent-ejo-0 class="row"></div>
```

### ## Overwriting Style Encapsulation
It is possible to overwrite these default styles encapsulation settings. 

**1. step** 
  - In the component **.ts file**, import **ViewEncapsulation** from Angular core:
```typescript
import { Component, OnInit, Input, ViewEncapsulation } from '@angular/core';
```

**2. step** 
  - Add **encapsulation** property to the **@Component** decorator, possible values of this property are: _ViewEncapsulation.None_, _ViewEncapsulation.Native_, and _ViewEncapsulation.Emulated_.
  
**2.a. ViewEncapsulation.None**
- **None** turns off the styles encapsulation and doesn't generate **_ngcontent-ejo-x** properties to the component HTML elements. Therefore, the styles are now applied globally:
```typescript
@Component({
  // Other compononet properties
  encapsulation: ViewEncapsulation.None
})
```

**2.b. ViewEncapsulation.Native**
- **Native** uses Shadow DOM technology, similar behavior to default encapsulation, only not supported by all the browsers, not recommended using:
```typescript
@Component({
  // Other compononet properties
  encapsulation: ViewEncapsulation.Native
})
```
**2.c. ViewEncapsulation.Emulated**
- **Emulated** encapsulates the style and, therefore, not necessary to define because it is set as default:
```typescript
@Component({
  // Other compononet properties
  encapsulation: ViewEncapsulation.Emulated
})
```

## # Local Reference in Templates
Get the whole HTML element from template. 

Can be used as an alternative to **two-way-binding** to get a **value** of HTML element (for example user's input).

- local reference can be placed on any HTML element,
- local reference holds reference on the **whole HTML element** with its **properties** and **value**,
- local reference can be used everywhere in the template **.html file**, never in .ts file.

**1. step**
  - In **.html file**, add local reference to HTML element with **#** _(hashtag)_:
```html
<input type="text" #try>
```

**2. step**
  - Pass the local reference as **an argument** to **event method** in the template:
```html
  <button (click)="onClickTry(try)">Click me</button>
```

**3.a. step**
  - In **.ts file**, receive the HTML element (and define its type as such) in your method,
  - name the parameter of the method **differently** than the local reference in your template:
```typescript
  onClickTry(tryReference: HTMLElement) {
    console.log(tryReference);
  }
```

**3.b. step**
  - If the HTML element referenced has a value (such as input), redefine the type of the parameter (since HTMLElement alone does not have a _value_ property) and use **HTMLInputElement** instead (or any other suitable type with a _value_ property),
  - call the value with **.value**:
```typescript
onClickTry(tryReference: HTMLInputElement) {
    console.log(tryReference.value);
  }
```

## # Access Template DOM with ViewChild
Apart from calling local references inside methods, there is Another way of accessing HTML element (or its value) with **ViewChild** regardless whether it is called in a method.

**1. step**
  - In **.html file**, add local reference to HTML element with **#** _(hashtag)_:
```html
<input type="text" #try>
```

**2. step** 
  - In **.ts file**, import **VieChild** and **ElementRef** from Angular core:
```typescript
import { Component, OnInit, Input, ViewChild, ElementRef } from '@angular/core';
```

**3. step**
  - In **.ts file**, add attribute to the class of a **ElementRef** type,
  - add **@ChildView** decorator to the attribute,
  - add **selector** as the decorator's property:
      - this selector is not like a selector in CSS,
      - local reference as the selector is written in .ts file without the # (hashtag),
      - whole component can also be used as the selector, this way, the first occurance of this component is accessed.
```typescript
@ChildView('try') try: ElementRef;
```

**4. step**
- In **.ts file**, you can access the element or its value with ElementRef's property **nativeElement**:
```typescript
onClickTry(tryReference: HTMLInputElement) {
    console.log(try.nativeElement.value);
  }
```

## # Projecting Content with ng-content
By default, everything placed in beetween the opening and closing tag of your own component will be ignored. This can be changed thanks to a directive **ng-content** (that looks like a HTML tag).

It serves as **a hook** that can be placed in the component to **mark the place** for Angular, where it should add any content it finds between opening and closing tag of the component selector tag.

**1. step**
  - In component 1 **.html file**, add **ng-content** opening and closing tag where the content is uspposed to be rendered:
```html
<!-- some html code -->
  	<p>Hello!</p>
	<ng-content></ng-content>
<!-- some html code -->
```

**2. step**
  - In component 2 **.html file**, add your code (that is supposed to be rendered in .html file with ng-content directive tag) in between the **component selector tag**: 
```html
<!-- some html code -->
	<app-component-1>
  		<p>{{ data }}</p>	<!-- this is going to be rendered where ng-content is placed -->
 	</app-component-1>
<!-- some html code -->
```

Final rendered content of component 1 .html file: 
```html
<!-- some html code -->
  	<p>Hello!</p>
	<p>{{ data }}</p>
<!-- some html code -->
```

## # Component Lifecycle Hooks
These are methods that Angular calls when a component's lifecycle phases occur, we can execute our code base on the phase (event):

- **ngOnChanges**
  - Called **mutltiple times**:
    - right after a component is screated, 
    - whenever a bound input property changes (properties with a **@Input** decorator):
```typescript
ngOnChanges() {
}
```

- **ngOnInit**
  - Called once a component has been initialized,
  - component  hasn't been yet added to the DOM (component is not displayed yet), only **the object has been created** (properties are initialized and are accessible to us),
  - runs **after the constructor**:
```typescript
ngOnInit() {
}
```

- **ngDoCheck**
  - Called **multiple times**, whenever **any change detection runs** (not only visible but every change):
```typescript
ngDoCheck() {
}
```

- **ngAfterContentInit**
  - Called after content **(ng-content)** has been projected into view:
```typescript
ngAfterContentInit() {
}
```