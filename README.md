# Angular 7+ Cheat Sheet

## # Components Databinding
### ## Binding to Custom Properties
#### Component Informed by PARENT Component
**1a.** In child component **.ts file**, define property with assigned type as a javascript object (or any other type), add decorator **@Input()** (don't forget the braces) before property name, and import **Input** (without braces) from Angular core at the beginning of the file:
```typescript
import { Component, OnInit, Input } from '@angular/core';

// more TypeScript code

@Input() element: {type: string, name: string, content: string};
```
**1b.** **Assign ALIAS:** Add alias name inside braces:
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
**3b.** With alias:
```html
<app-server-element [element]="srvElement"></app-server-element>
```

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
**2.** In parent component **.html file**, bind the event to HTML element:
```html
<app-cockpit (serverCreated)="onServerAdded($event)"></app-cockpit>
```



