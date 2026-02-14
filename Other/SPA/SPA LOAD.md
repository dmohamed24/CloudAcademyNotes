Here is a **clear, step-by-step flow** of what happens when a user loads a **Single Page Application (SPA)** — from download → parsing → execution → rendering → hydration → interaction.  
This is the _full lifecycle_ in the correct order, simplified.

---

# ⭐ **SPA Load Process (Step-by-Step)**

### _(from the moment the user types the URL → until the app is running)_

---

# **0. BEFORE USER LOADS → Build Step (Done by Dev Tools)**

This happens **on the developer’s machine / CI**, _not_ in the user’s browser.

- Bundler (Vite/Webpack/Rollup) does:
    
    - bundle JS files into chunks
        
    - tree shaking (remove unused code)
        
    - minify JS
        
    - compile JSX/TS → JS
        
    - process CSS
        
    - split code into dynamic chunks
        

📌 Result:  
**Optimized static files** → `index.html`, `main.js`, `vendor.js`, CSS, assets.

---

# **1. User Loads the SPA (First Request)**

### **Step 1A — Browser requests `index.html`**

- User hits: `https://example.com/`
    
- Server returns:
    
    - a very simple **index.html** file
        
    - usually contains only:
        
        `<div id="root"></div> <script src="/assets/main.js"></script>`
        

📌 Server CPU load: **tiny**  
It is just serving static files.

---

# **2. Browser Starts Processing**

### **Step 2A — Browser parses HTML**

- HTML is parsed quickly (very small)
    
- The browser discovers the `<script>` tag and prepares to download JavaScript.
    

### **Step 2B — Browser downloads JS bundles**

- `main.js`
    
- vendor chunks (frameworks)
    
- additional lazy chunks (not always at first)
    

These files were created **earlier by the bundler**.

---

# **3. Browser Executes JavaScript (The Heavy Part)**

This is where the **client CPU load increases**.

### **Step 3A — JavaScript initializes the SPA framework**

For example:

- React creates a virtual DOM
    
- Vue creates reactive components
    
- Angular bootstraps modules
    

### **Step 3B — The app "mounts" to the root element**

The framework attaches to the `<div id="root">`.

Example for React:

`ReactDOM.createRoot(document.getElementById("root")).render(<App />);`

---

# **4. SPA Starts Rendering UI (Client Rendering)**

### **Step 4A — JS fetches data (optional)**

- SPA may call APIs before first render.
    

### **Step 4B — Framework builds virtual DOM**

- Based on JS + templates + data
    

### **Step 4C — Browser updates the real DOM**

- Text, buttons, layout appear
    
- CSS applies styling
    

📌 At this point → **User sees the actual UI**

---

# **5. App Is Now Interactive (Hydrated)**

Although “hydration” is more of an SSR term, in SPAs this phase is simply:

### **Step 5A — Event listeners attach**

- onClick
    
- onChange
    
- onSubmit
    
- router events
    

### **Step 5B — SPA router becomes active**

Navigations like:

`/dashboard /settings /profile`

happen **without reloading the page**.

### **Step 5C — Lazy chunks load as user navigates**

- If user moves to a different route
    
- SPA downloads only the JS chunk needed for that route (code splitting)
    

---

# **✔ Final Result**

The SPA is fully loaded, JavaScript is running, and:

- server just serves static files
    
- client handles rendering, routing, updates, DOM changes, interactions
    

**The client is doing all the heavy CPU work.**

---

# 📌 **Summary in Order (Very Simply)**

1. **Dev-time:** Bundler compiles → bundles → optimizes code
    
2. **Browser loads `index.html`**
    
3. **Browser downloads JS bundles**
    
4. **Browser executes JS → initializes SPA**
    
5. **Framework renders UI into `<div id="root">`**
    
6. **SPA router activates (client-side routing)**
    
7. **App is interactive + loads more chunks as needed**