Here are five one-line "do not" slides with code examples:

1. **Do not expect immediate state changes - the set function only updates for the next render**
```javascript
setCount(count + 1);
console.log(count); // ❌ Still shows old value, not updated yet
```

2. **Do not mutate objects or arrays directly - always create new copies using spread syntax or methods**
```javascript
obj.name = 'New'; setObj(obj); // ❌ Wrong
setObj({...obj, name: 'New'}); // ✅ Correct
```

3. **Do not call set functions unconditionally during render - it causes infinite loops and crashes**
```javascript
function Component() {
  const [count, setCount] = useState(0);
  setCount(count + 1); // ❌ Infinite loop!
  return <div>{count}</div>;
}
```

4. **Do not pass function calls as initializers `useState(expensive())` - pass the function itself `useState(expensive)`**
```javascript
useState(createInitialTodos()); // ❌ Runs every render
useState(createInitialTodos); // ✅ Runs only once
```

5. **Do not read state immediately after calling set function - you'll still get the old value from current render**
```javascript
setAge(age + 1);
setAge(age + 1); // ❌ Still uses old age, becomes age + 1, not age + 2
setAge(a => a + 1); // ✅ Uses pending state
```
