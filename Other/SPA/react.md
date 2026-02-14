
# ⚛️ Core React Concepts

---

## React Rules (JSX Rules)

- Must return **one parent element**.
    
    - Use a `<div>` or `<> Fragment </>`.
        
- Cannot use:
    
    - `class` → use `className`
        
    - `for` → use `htmlFor`
        
- All tags must be closed:
    
    `<img src="img.png" />`
    
- JavaScript expressions go inside `{}`:
    
    `{user.name}`
    
- Inline styles:
    
    - Use camelCase:
        
        `style={{ backgroundColor: "red" }}`
        
    - Inline styles override CSS.
        

---

# 🧠 Virtual DOM

### Problem:

Direct DOM manipulation is slow because:

- DOM operations are expensive.
    
- JavaScript is single-threaded.
    

### Solution: Virtual DOM

- React creates a lightweight copy of the real DOM.
    
- When state changes:
    
    1. A new virtual DOM is created.
        
    2. React compares it with the previous one (diffing).
        
    3. Only the changed parts are updated in the real DOM.
        

### Why Keys Are Important

When rendering lists:

`items.map(item => <Card key={item.id} />)`

- Keys help React identify which item changed.
    
- Prevents unnecessary re-renders.
    
- Should be unique and stable.
    

---

# 📦 Props

- Props = data passed from parent → child.
    
- Props are read-only.
    
- Used to make components reusable.
    

### Children Prop

Allows wrapper components:

`function Card({ children }) {   return <div className="card">{children}</div>; }`

Usage:

`<Card>   <h2>Hello</h2> </Card>`

---

# 🎯 Event Handling

Used for interactivity.

Common events:

- `onClick`
    
- `onChange`
    
- `onBlur`
    
- `onSubmit`
    

### Inline

`<button onClick={() => alert("Hi")}>Click</button>`

### Function Reference

`<button onClick={handleClick}>Click</button>`

### Event Object

React provides event object:

`function handleSubmit(e) {   e.preventDefault();   console.log(e.target.value); }`

Useful methods:

- `preventDefault()`
    
- `stopPropagation()`
    
- `target`
    
- `type`
    

---

# 🔀 Conditional Rendering

Show/hide UI based on conditions:

`{isLoggedIn && <Dashboard />}`

`{isAdmin ? <Admin /> : <User />}`

Elements either exist in the DOM or they don’t.

---

# 🔁 Lists & Looping

Render lists using `.map()`:

`items.map(item => (   <Card key={item.id} title={item.title} /> ))`

- Must include a unique `key`.
    
- Avoid using array index as key unless no better option.
    

---

# 📝 Forms & Input Handling

### Controlled Components

Input value tied to state:

`const [name, setName] = useState("");  <input value={name} onChange={(e) => setName(e.target.value)} />`

React controls the input.

---

### Form Submission

`function handleSubmit(e) {   e.preventDefault();   // handle logic }`

- Prevents page reload.
    
- Usually:
    
    - Validate
        
    - Send API request
        
    - Navigate user
        

---

# 🧠 State Management

State = component memory.

- Determines when component re-renders.
    
- Stored using hooks.
    

---

## `useState`

`const [count, setCount] = useState(0);`

### Key Rules:

- State is not persisted on page reload.
    
- State updates are batched.
    
- Do not mutate state directly:  
    ❌ `count++`  
    ✅ `setCount(count + 1)`
    

### Functional Update (Important)

When new state depends on old state:

`setCount(prev => prev + 1);`

### Updating Objects

Must return a new object:

`setUser(prev => ({ ...prev, name: "John" }));`

---

## `useEffect`

Used for side effects:

- Data fetching
    
- Subscriptions
    
- Timers
    
- DOM manipulation
    
- Logging
    

### Syntax:

`useEffect(() => {   // logic    return () => {     // cleanup   }; }, [dependencies]);`

### 3 Types:

1. Runs every render:
    
    `useEffect(() => {})`
    
2. Runs once on mount:
    
    `useEffect(() => {}, [])`
    
3. Runs when dependency changes:
    
    `useEffect(() => {}, [value])`
    

---

## `useContext`

Solves prop drilling.

### Problem:

Passing props deeply through many components.

### Solution:

Context API

- Create context.
    
- Wrap component tree with provider.
    
- Access with `useContext`.
    

### Best Practices:

- Split contexts by concern.
    
- Avoid wrapping entire app unnecessarily.
    
- Avoid using for frequently changing local state.
    
- Create custom hooks:
    
    `const useAuth = () => useContext(AuthContext);`
    

---

# 🧭 React Router

Used in SPAs for navigation without full page reload.

### Benefits:

- URL-based navigation.
    
- Browser back/forward works.
    
- Shareable URLs.
    
- Proper routing structure.
    

---

# 🎣 Common React Hooks

|Hook|Purpose|
|---|---|
|`useState`|Manage state|
|`useEffect`|Side effects|
|`useContext`|Global/shared state|
|`useCallback`|Memoize functions|
|`useMemo`|Memoize computed values|
|`useRef`|Persist values without re-render|
|`useReducer`|Complex state logic|

---

# 🚀 Additional Important Concepts (Added Gaps)

---

## Re-rendering

A component re-renders when:

- Its state changes.
    
- Its props change.
    
- Its parent re-renders.
    

---

## Memoization

### `React.memo`

Prevents re-render if props haven’t changed.

`export default React.memo(MyComponent);`

---

### `useCallback`

Prevents function recreation:

`const handleClick = useCallback(() => {   console.log("Clicked"); }, []);`

---

### `useMemo`

Prevents expensive recalculations:

`const sortedList = useMemo(() => {   return items.sort(); }, [items]);`

---

# 🏗 Component Types

### Functional Components

Modern standard.

### Controlled vs Uncontrolled Components

- Controlled → React manages value.
    
- Uncontrolled → DOM manages value (useRef).
    

---

# 🔥 React Rendering Flow (Simplified)

1. State/props change.
    
2. React re-renders component.
    
3. Virtual DOM updated.
    
4. Diffing algorithm runs.
    
5. Real DOM updated minimally.