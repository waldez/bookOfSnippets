# Useful code snippets

## Javascript 
```javascript
// to get an actual type of the value
const type = typeof value == 'object' && value !== null ?
        Object.getPrototypeOf(value).constructor.name :
        typeof value;
```
