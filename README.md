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
  - In the component **.ts file**, import **ViewEncapsulation** from Angular core.
```typescript
import { Component, OnInit, Input, ViewEncapsulation } from '@angular/core';
```

**2. step** 
  - Add **encapsulation** property to the **@Component** decorator, possible values of this property are: _ViewEncapsulation.None_, _ViewEncapsulation.Native_, and _ViewEncapsulation.Emulated_.
  
**2.a. ViewEncapsulation.None**
- **None** turns off the styles encapsulation and doesn't generate **_ngcontent-ejo-x** properties to the component HTML elements. Therefore, the styles are now applied globally.
```typescript
@Component({
  // Other compononet properties
  encapsulation: ViewEncapsulation.None
})
```

**2.b. ViewEncapsulation.Native**
- **Native** uses Shadow DOM technology, similar behavior to default encapsulation, only not supported by all the browsers, not recommended using.
```typescript
@Component({
  // Other compononet properties
  encapsulation: ViewEncapsulation.Native
})
```
**2.c. ViewEncapsulation.Emulated**
- **Emulated** encapsulates the style and, therefore, not necessary to define because it is set as default.
```typescript
@Component({
  // Other compononet properties
  encapsulation: ViewEncapsulation.Emulated
})
```
