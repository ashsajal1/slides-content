# useId Hook: 10 Key Points with Code

1. **useId generates unique IDs** for accessibility attributes in React components
```javascript
import { useId } from 'react';

function MyComponent() {
  const id = useId();
  return <input aria-describedby={id} />;
}
```

2. **Call it at the top level** of your component or custom Hooks only
```javascript
function CorrectUsage() {
  const id = useId(); // ✓ Top level
  return <div id={id}>Content</div>;
}

function IncorrectUsage() {
  if (condition) {
    const id = useId(); // ✗ Inside condition
  }
}
```

3. **Returns a unique string** associated with that specific useId call in that component
```javascript
function Example() {
  const id = useId();
  console.log(id); // Output: ":r1:" or similar unique string
  return <div id={id}>Unique ID: {id}</div>;
}
```

4. **Perfect for aria-describedby** and other HTML accessibility attributes that link elements
```javascript
function PasswordField() {
  const passwordHintId = useId();
  return (
    <>
      <label>Password:</label>
      <input type="password" aria-describedby={passwordHintId} />
      <p id={passwordHintId}>Must be at least 8 characters</p>
    </>
  );
}
```

5. **Never use useId for list keys** - keys should always be generated from your data
```javascript
// ✗ Wrong
function ItemList({ items }) {
  return items.map(() => <Item key={useId()} />);
}

// ✓ Correct
function ItemList({ items }) {
  return items.map(item => <Item key={item.id} />);
}
```

6. **Solves the hardcoded ID problem** when components render multiple times on a page
```javascript
// ✗ Hardcoded (problematic if rendered multiple times)
function Input() {
  return (
    <>
      <label htmlFor="input-1">Name:</label>
      <input id="input-1" />
    </>
  );
}

// ✓ Using useId
function Input() {
  const id = useId();
  return (
    <>
      <label htmlFor={id}>Name:</label>
      <input id={id} />
    </>
  );
}
```

7. useId currently cannot be used in async Server Components.
```javascript
// ✗ Wrong - useId in async Server Component
async function AsyncServerComponent() {
  const id = useId(); // Error: Cannot use useId in async Server Component
  
  return (
    <>
    ....
    </>
  );
}
```

8. **Generate shared prefixes** by calling useId once and appending suffixes for related elements
```javascript
function Form() {
  const id = useId();
  return (
    <>
      <label htmlFor={`${id}-firstName`}>First Name:</label>
      <input id={`${id}-firstName`} />
      <label htmlFor={`${id}-lastName`}>Last Name:</label>
      <input id={`${id}-lastName`} />
    </>
  );
}
```

9. **Use identifierPrefix option** in createRoot/hydrateRoot to prevent ID clashes between multiple React apps
```javascript
// App 1
createRoot(document.getElementById('app1')).render(
  <App />,
  { identifierPrefix: 'app1-' }
);

// App 2
createRoot(document.getElementById('app2')).render(
  <App />,
  { identifierPrefix: 'app2-' }
);
```

10. **Better than incrementing counters** because it works reliably with server rendering and concurrent features
```javascript
// ✗ Incrementing counter (unreliable with SSR)
let counter = 0;
function Component() {
  const id = `id-${counter++}`;
  return <div id={id}>Content</div>;
}

// ✓ useId (reliable with SSR and concurrent rendering)
function Component() {
  const id = useId();
  return <div id={id}>Content</div>;
}
```

---

# Linkedin post - React's useId Hook: Stop Hardcoding IDs! 🎯

Ever struggled with duplicate IDs when a component renders multiple times? React's **useId** hook is here to save the day!

## What is useId?
A simple hook that generates unique IDs for accessibility attributes - perfect for linking form inputs with their descriptions using `aria-describedby`.

## Why It Matters:
✅ **No more ID conflicts** when components render multiple times  
✅ **Server-side rendering friendly** - IDs match between server and client  
✅ **Better than counters** - works flawlessly with React's concurrent features  
✅ **Accessibility first** - makes your apps more inclusive

## Quick Example:
```javascript
function PasswordField() {
  const passwordHintId = useId();
  return (
    <>
      <input aria-describedby={passwordHintId} />
      <p id={passwordHintId}>Password must be 8+ characters</p>
    </>
  );
}
```

## Pro Tips:
🔹 Call it at component top level only  
🔹 Never use for list keys (use data-based keys instead)  
🔹 Generate shared prefixes for related elements  
🔹 Use `identifierPrefix` to prevent clashes in multi-app pages

Building accessible React apps just got easier! 🚀

#React #WebDevelopment #Accessibility #Frontend #JavaScript #A11y #WebAccessibility
