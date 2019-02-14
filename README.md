# Angular 7+ Cheat Sheet

## Components Databinding
### Binding to Custom Properties
#### Component Accessed by PARENT
**1.** Property with assigned type as a javascript object:
```typescript
element: {type: string, name: string, content: string};
```
**2.** In parent component **.ts file**, assign values to Javascript object literal:
```typescript
serverElements = [{type: 'server', name: 'Testserver', content: 'Just a test'}];
```
**3.** In parent component **.html file**, assign values to Javascript object literal:
```typescript
serverElements = [{type: 'server', name: 'Testserver', content: 'Just a test'}];
```





