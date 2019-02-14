# Angular 7+ Cheat Sheet

## Components Databinding
### Binding to Custom Properties
#### Component Accessed by PARENT
- Property with assigned type as a javascript object:
```typescript
element: {type: string, name: string, content: string};
```
- In parent component, assign values to Javascript object literal:
```typescript
serverElements = [{type: 'server', name: 'Testserver', content: 'Just a test'}];
```





