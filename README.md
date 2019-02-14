# Angular 7+ Cheat Sheet

## Components Databinding
### Binding to Custom Properties
#### Component Accessed by PARENT Component
**1.** In child component **.ts file**, define property with assigned type as a javascript object (or any other type), add decorator **@Input()** before property name, and import **Input** from Angular core at the beginning of the file:
```typescript
import { Component, OnInit, **Input** } from '@angular/core';
	...
element: {type: string, name: string, content: string};
```
**2.** In parent component **.ts file**, assign values to Javascript object literal:
```typescript
serverElements = [{type: 'server', name: 'Testserver', content: 'Just a test'}];
```
**3.** In parent component **.html file**, bind the property to HTML element.
```html
<app-server-element [element]="serverElement"></app-server-element>
```





