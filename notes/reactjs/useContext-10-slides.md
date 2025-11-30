# useContext Hook - 10 Key Slides

### 1. **What is useContext?** - A React Hook that reads and subscribes to context from your component

```javascript
import { useContext } from 'react';

const useContext(SomeContext);
```

### 2. **Basic Usage** - Call `useContext(SomeContext)` at the top level of your component to access context values

```javascript
function MyComponent() {
  const theme = useContext(ThemeContext);
  return <div>{theme}</div>;
}
```

### 3. **Return Value** - Returns the value from the closest matching Provider above in the component tree

```javascript
function Button() {
  const theme = useContext(ThemeContext); // Returns "dark" from closest provider
  return <button style={{ background: theme }}>Click me</button>;
}
```

### 4. **No Provider?** - If no provider exists, returns the `defaultValue` specified in `createContext()`

```javascript
const ThemeContext = createContext('light'); // default value

function Component() {
  const theme = useContext(ThemeContext); // Returns 'light' if no provider above
  return <div>{theme}</div>;
}
```

### 5. **Passing Data Deep** - Eliminates prop drilling by allowing data to flow through the component tree without manual passing

```javascript
<ThemeContext.Provider value="dark">
  <Form>
    <Button /> {/* Receives "dark" without prop drilling */}
  </Form>
</ThemeContext.Provider>
```

### 6. **Updating Context** - Combine with `useState` to create dynamic context that updates and triggers re-renders

```javascript
function MyApp() {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={theme}>
      <label>
        <input
          type="checkbox"
          onChange={(e) => setTheme(e.target.checked ? 'dark' : 'light')}
        />
        Dark mode
      </label>
      <Button />
    </ThemeContext.Provider>
  );
}
```

### 7. **Overriding Context** - Nest providers to override context values for specific parts of your component tree

```javascript
<ThemeContext.Provider value="dark">
  <Header /> {/* Uses "dark" */}
  <ThemeContext.Provider value="light">
    <Footer /> {/* Uses "light" - overridden */}
  </ThemeContext.Provider>
</ThemeContext.Provider>
```

### 8. **Performance Optimization** - Use `useMemo` and `useCallback` to prevent unnecessary re-renders when passing objects/functions

```javascript
function MyApp() {
  const [currentUser, setCurrentUser] = useState(null);
  
  const login = useCallback((response) => {
    setCurrentUser(response.user);
  }, []);
  
  const value = useMemo(() => ({ currentUser, login }), [currentUser, login]);
  
  return (
    <AuthContext.Provider value={value}>
      <Component />
    </AuthContext.Provider>
  );
}
```

### 9. **Common Pitfall** - `useContext()` searches upward only - doesn't consider providers in the calling component itself

```javascript
function Component() {
  const theme = useContext(ThemeContext); // Searches ABOVE this component
  
  return (
    <ThemeContext.Provider value="light">
      {/* This provider won't be used by useContext above */}
      <div>{theme}</div>
    </ThemeContext.Provider>
  );
}
```

### 10. **Troubleshooting** - A Provider with `value={undefined}` overrides default values; default only applies when NO provider exists above

```javascript
const ThemeContext = createContext('light'); // default value

// ❌ This passes undefined, overriding the default
<ThemeContext.Provider value={undefined}>
  <Component /> {/* Receives undefined, not 'light' */}
</ThemeContext.Provider>

// ✅ Correct - no provider, so default is used
<Component /> {/* Receives 'light' */}
```
