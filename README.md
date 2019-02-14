# Angular 7+ Cheat Sheet

## # Components Databinding
### ## Binding to Custom Properties
#### Component Informed by PARENT Component
**1.a. step** 
  - In child component **.ts file**, define property with assigned type as a javascript object (or any other type), 
  - add decorator **@Input()** (don't forget the braces) before property name, 
  - import **Input** (without braces) from Angular core at the beginning of the file.
```typescript
import { Component, OnInit, Input } from '@angular/core';

// more TypeScript code

@Input() element: {type: string, name: string, content: string};
```
**1.b. step** _Optional_ 
	- **Assign ALIAS:** Add alias name inside braces:
```typescript
@Input('srvElement') element: {type: string, name: string, content: string};
```
**2.** In parent component **.ts file**, assign values to Javascript object literal:
```typescript
serverElements = [{type: 'server', name: 'Testserver', content: 'Just a test'}];
```
**3a.** In parent component **.html file**, bind the property to HTML element:
```html
<app-server-element [element]="serverElement"></app-server-element>
```
**3b.** _Optional_ With alias:
```html
<app-server-element [element]="srvElement"></app-server-element>
```


### ## Binding to Custom Events
#### Component Informed by CHILD Component
We want to "listen" to some events, for example, we want to inform parent component that new server was created. 

**1.** In parent component **.ts file**, define function(s):
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
**2a.** In parent component **.html file**, bind the event to HTML element:
```html
<app-cockpit (serverCreated)="onServerAdded($event)"></app-cockpit>
```
**2b.**  _Optional_ With alias (name of alias from _3b_):
```html
<app-cockpit (srvCreated)="onServerAdded($event)"></app-cockpit>
```
**3a.** **DEFINING EVENT EMITTER:** In child component **.ts file**, define new property informing about the event by assigning to it **new EventEmitter<_define type of event data_>()** (a generic type) with a constructor **()** at the end and import **EventEmitter** from Angular core at the beginning of the file. 

Also, add decorator **@Output()** (don't forget the braces) before property name and import **Output** (without braces) from Angular core at the beginning of the file:
```typescript
import { Component, OnInit, EventEmitter, Output } from '@angular/core';

// more TypeScript code

@Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```
**3b.** _Optional_ **Assign ALIAS:** Add alias name inside braces:
```typescript
@Output('srvCreated') serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
```
**4.** **CALLING EVENT EMITTER:** In child component **.ts file**, in a method, call and emit the property with emitter with contracted current values of parameters (by using **this**):
```typescript
// When creating new server
onAddServer {
  this.serverCreated.emit({serverName: this.serverName, serverContent: this.serverContent});
}

```
