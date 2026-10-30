# Complete React Interview Preparation: All Topics + Questions and Answers

We’ll build a comprehensive React interview preparation guide, covering the fundamentals, advanced concepts, practical coding, debugging, performance optimization, real-world scenarios, and production deployment.

The goal is not just to memorize interview answers. You should understand **what React does, how it works internally, why a feature exists, when to use it, and how to solve real problems with it**.

No single guide can guarantee every question an interviewer might invent, but we can systematically cover the full React ecosystem and its important subtopics without deliberately skipping sections.

---

# Part 1: Complete React Topic Roadmap

We’ll use this roadmap as our master checklist. Each section includes:
- **Theory:** Concepts explained clearly.
- **Interview questions and answers:** Beginner, intermediate, and advanced.
- **Scenario-based questions:** Real problems encountered in development.
- **Coding questions:** Programs and implementation tasks.
- **Common mistakes:** Traps interviewers often test.
- **Practical examples:** How concepts are used in actual applications.

## A. Master Checklist: React Topics From Beginner to Expert

The roadmap follows the main areas of the official [React documentation](https://react.dev/learn), supplemented with practical frontend engineering and interview topics.

| No. | Topic | What we must cover |
|:---:|---|---|
| **1** | **React Fundamentals** | What React is, features, benefits, limitations, use cases |
| **2** | **JavaScript Prerequisites** | ES6+, arrow functions, destructuring, spread/rest, modules, closures, promises, async/await |
| **3** | **React Environment Setup** | Node.js, npm, Vite, project creation, development server |
| **4** | **React Project Structure** | `src`, `public`, entry points, assets, environment variables |
| **5** | **Components** | Functional components, component composition, reusable components, component purity |
| **6** | **JSX** | Syntax, expressions, fragments, attributes, JSX transformation |
| **7** | **Props** | Passing data, default values, `children`, prop drilling, one-way data flow |
| **8** | **State** | `useState`, state updates, batching, functional updates, derived state |
| **9** | **Events** | Event handlers, event arguments, forms, propagation, preventing default behavior |
| **10** | **Conditional Rendering** | `if`, ternary operator, logical AND (`&&`), loading and error states |
| **11** | **Lists and Keys** | `map`, `filter`, stable keys, index-key problems, rendering lists |
| **12** | **Component Lifecycle** | Mounting, updating, unmounting, lifecycle concepts in functional components |
| **13** | **Hooks Fundamentals** | Rules of Hooks, built-in Hooks, custom Hooks |
| **14** | **`useEffect`** | Dependencies, cleanup, synchronization, fetching data, infinite loops |
| **15** | **`useRef`** | DOM access, mutable values, timers, previous values |
| **16** | **`useContext`** | Context creation, providers, consumers, avoiding unnecessary context updates |
| **17** | **`useReducer`** | Reducers, actions, complex state, state transitions |
| **18** | **`useMemo`** | Memoizing calculated values and expensive computations |
| **19** | **`useCallback`** | Memoizing function references and avoiding unnecessary renders |
| **20** | **`React.memo`** | Component memoization, shallow prop comparison, limitations |
| **21** | **Custom Hooks** | Reusable stateful logic, Hook design, cleanup |
| **22** | **Forms** | Controlled and uncontrolled inputs, validation, submission, reset |
| **23** | **Advanced Forms** | Dynamic fields, reusable form components, form libraries |
| **24** | **Refs and Imperative APIs** | `forwardRef` in React 18 and earlier, current ref patterns (React 19 ref-as-prop), `useImperativeHandle` |
| **25** | **Portals** | Modals, tooltips, overlays, event propagation |
| **26** | **Error Handling** | Error boundaries, fallback UI, async error handling |
| **27** | **Fragments** | Grouping elements without extra DOM nodes |
| **28** | **Strict Mode** | Development checks, repeated rendering and Effect setup |
| **29** | **Rendering Internals** | Trigger, render, commit, reconciliation, Fiber concepts |
| **30** | **Virtual DOM** | What it means, how it relates to rendering, limitations of the term |
| **31** | **React DOM** | `createRoot`, `root.render`, `root.unmount`, DOM integration |
| **32** | **React 18 Features** | Automatic batching, transitions (`useTransition`, `useDeferredValue`), concurrent rendering concepts, Suspense improvements |
| **33** | **Modern React Features (React 19)** | Actions, `useActionState`, `useOptimistic`, `useFormStatus`, `use`, and other version-dependent APIs |
| **34** | **React Compiler** | Automatic optimization, supported configuration, relationship to manual memoization |
| **35** | **Routing** | React Router, routes, nested routes, parameters, navigation |
| **36** | **Protected Routes** | Authentication checks, authorization, redirects, role-based access |
| **37** | **API Integration** | Fetch, Axios, REST APIs, HTTP methods, headers, response handling |
| **38** | **Asynchronous UI** | Loading, success, empty, error, cancellation, race conditions |
| **39** | **Data Fetching** | Effects, caching, refetching, pagination, infinite scrolling |
| **40** | **State Management** | Local state, lifted state, Context, reducers, external state stores |
| **41** | **Redux** | Store, actions, reducers, selectors, middleware, Redux Toolkit (RTK) |
| **42** | **Server State** | TanStack Query (React Query), caching, invalidation, mutations, stale data |
| **43** | **Zustand and Alternatives** | External stores, `useSyncExternalStore`, subscriptions, state selection |
| **44** | **Performance Optimization** | Profiling, render analysis, memoization, expensive calculations |
| **45** | **Code Splitting** | `React.lazy`, `Suspense`, dynamic imports, route-level splitting |
| **46** | **Lists at Scale** | Pagination, windowing/virtualization (`react-window`, TanStack Virtual), large datasets |
| **47** | **TypeScript With React** | Props, interfaces, unions, generics, events, refs, Hooks |
| **48** | **Styling** | CSS, CSS Modules, Sass, inline styles, Tailwind CSS, CSS-in-JS |
| **49** | **UI Components** | Reusable buttons, modals, tables, dropdowns, headless UI, design systems |
| **50** | **Accessibility (a11y)** | Semantic HTML, keyboard navigation, ARIA, focus management |
| **51** | **Testing** | Unit tests, integration tests, React Testing Library, Vitest, Jest |
| **52** | **End-to-End Testing** | Playwright, Cypress, user journeys, browser automation |
| **53** | **Security** | XSS, `dangerouslySetInnerHTML`, safe rendering, authentication, token handling, dependency security |
| **54** | **Authentication** | Login, logout, sessions, `HttpOnly` cookies, access tokens, refresh tokens |
| **55** | **File Operations** | Uploads, downloads, `FormData`, progress, validation, `URL.createObjectURL` previews |
| **56** | **Internationalization (i18n)** | Multiple languages, `Intl` locale formatting, translation management, RTL layouts |
| **57** | **React Architecture** | Feature-based folders, component boundaries, separation of concerns |
| **58** | **Design Patterns** | Composition, compound components, render props, higher-order components (HOCs) |
| **59** | **Server-Side Rendering (SSR)** | SSR, hydration, client rendering (CSR), rendering trade-offs |
| **60** | **Static Generation (SSG)** | SSG, prerendering, static hosting, Incremental Static Regeneration (ISR) concepts |
| **61** | **React Server Components (RSC)** | Server versus Client Components (`"use client"`), boundaries, serialization |
| **62** | **React Frameworks** | Next.js (App Router), Remix / React Router v7, framework-specific rendering features |
| **63** | **Suspense and Streaming** | Fallbacks, async boundaries, streaming HTML (`renderToPipeableStream`), selective hydration |
| **64** | **Web Performance** | Core Web Vitals (LCP, INP, CLS), bundle analysis, image optimization, lazy loading |
| **65** | **Build Tools** | Vite, Rollup/esbuild, bundling, tree shaking, source maps, production builds |
| **66** | **Environment Configuration** | Development, staging, testing, and production configuration (`import.meta.env`) |
| **67** | **Deployment** | Static hosting, CDN caching headers, reverse proxy, SPA routing fallback (`try_files`) |
| **68** | **Docker and React** | Dockerfiles, multi-stage builds, Nginx configuration, container security |
| **69** | **CI/CD** | Automated testing, linting, type-checking, builds, deployment pipelines |
| **70** | **Git and Team Workflows** | Feature branches, pull requests, code reviews, conflict resolution |
| **71** | **Debugging** | React DevTools Components & Profiler tabs, browser DevTools, network debugging |
| **72** | **Common Errors** | Invalid Hook calls, stale closures, missing keys, hydration mismatches |
| **73** | **Frontend System Design** | Component architecture, state normalization, caching, API boundaries, scalability |
| **74** | **Real-World Scenarios** | Live dashboards, e-commerce checkouts, exam portals, admin panels, multi-step forms |
| **75** | **Coding Interviews** | Debounced search, filtering, sorting, pagination, custom Hooks, interactive UI widgets |
| **76** | **JavaScript Deep Dive** | Closures, event loop, macrotasks vs microtasks, promises, immutability, structural sharing |
| **77** | **Browser Fundamentals** | DOM, CSSOM, Critical Rendering Path, layout (reflow), paint, compositing |
| **78** | **Network Fundamentals** | HTTP/2 & HTTP/3, CORS preflight, cookies (`SameSite`), caching (`ETag`), WebSockets, SSE |
| **79** | **Advanced Patterns** | Finite state machines, optimistic UI updates, compound APIs, controlled/uncontrolled hybrid components |
| **80** | **React Version Differences** | Class components -> Hooks (16.8) -> Concurrent React (18) -> Actions & Compiler (19), migrations |

> [!IMPORTANT]
> Some topics belong to React itself, while others belong to JavaScript, browser APIs, or the surrounding React ecosystem. All are relevant to professional React development and interviews.
> For version-specific features, we distinguish modern React APIs (React 18 and React 19) from older patterns rather than treating every feature as available in every version. The official [React API reference](https://react.dev/reference/react) is our primary reference for that distinction.

---

# Part 2: React Fundamentals — Interview Questions and Answers

We begin at the foundation and progress toward advanced topics.

## Topic 1: Introduction to React

### Q1. What is React?

**Answer:**

React is an open-source JavaScript library used to build user interfaces. It allows developers to create reusable components and describe how the UI should look for a particular state.

React updates the interface automatically and efficiently when relevant state or props change.

**Example:**

```jsx
function Welcome() {
  return <h1>Welcome to React!</h1>;
}

export default Welcome;
```

Here, `Welcome` is a React function component that describes a heading.

**Key interview points:**
- React is a **UI library**, not a complete opinionated application framework by itself.
- React applications are built from composable **components**.
- Components receive external data through **props**.
- Components manage internal changing data using **state**.
- React uses **declarative UI development**.
- React integrates cleanly with routing, API clients, state-management libraries, and full-stack frameworks.

*Official reference:* [React Quick Start](https://react.dev/learn)

---

### Q2. Why do developers use React?

**Answer:**

React is widely adopted because it makes complex, interactive user interfaces easier to develop, reuse, test, and maintain.

Its main advantages include:
- **Component reusability:** Create a component (e.g., `Button`, `Modal`, `DataTable`) once and reuse it across the application.
- **Declarative rendering:** Describe the desired UI for a given state instead of manually executing imperative DOM operations (`document.createElement`, `appendChild`) for every state transition.
- **State-driven interfaces:** React automatically synchronizes the DOM with application state.
- **Unidirectional (one-way) data flow:** Data flows predictably from parent components down to children via props, making debugging state changes much easier than two-way data binding.
- **Rich ecosystem:** Mature libraries exist for routing (React Router, TanStack Router), server state (TanStack Query), forms (React Hook Form), and frameworks (Next.js).
- **Flexible adoption:** React can power an entire Single-Page Application (SPA), a Server-Rendered application, or a single interactive widget embedded on an existing legacy page.
- **Developer tooling:** React Developer Tools allows inspecting component hierarchies, props, hooks, and flamegraph render timings.

**Example:**

Instead of manually querying and mutating a heading node using DOM methods, React renders it declaratively from state:

```jsx
import { useState } from "react";

function Greeting() {
  const [name, setName] = useState("Binil");

  return (
    <div>
      <h1>Hello, {name}</h1>
      <button onClick={() => setName("React Developer")}>
        Change Name
      </button>
    </div>
  );
}

export default Greeting;
```

When the button is clicked, `setName` schedules a state update, React re-evaluates `Greeting`, compares the new element tree with the previous one, and updates only the changed text in the DOM.

---

### Q3. Is React a library or a framework?

**Answer:**

React is a **JavaScript library** focused specifically on building and rendering user interfaces.

A **framework** enforces an overarching application architecture and provides built-in solutions for routing, data fetching, bundling, server rendering, and folder conventions (Inversion of Control: the framework calls your code).

React itself focuses on the view layer. Developers either assemble their own stack around React (e.g., Vite + React Router + TanStack Query) or adopt a **React-based framework** such as Next.js or Remix.

**Interview distinction:**

| React (Library) | React-Based Framework (e.g., Next.js, Remix) |
|---|---|
| Primarily provides UI components, state hooks, and rendering primitives | Provides a complete, opinionated application architecture |
| Routing is added separately (e.g., `react-router-dom`) | Includes built-in file-system or config-based routing |
| Build, bundler, and rendering setup depend on your chosen tooling | Provides built-in bundling, SSR, SSG, RSC, and API routes |
| Can be dropped into a single `<div>` on an existing webpage | Typically owns the entire application lifecycle and server entry point |

**Follow-up question:** *Is React alone enough to build an entire production application?*
You can build a complete client-side application using React alongside ecosystem libraries for routing, data fetching, and bundling. However, for SEO-critical pages, Server-Side Rendering (SSR), or React Server Components (RSC), pairing React with a framework or custom server setup is required.

---

### Q4. What is declarative programming in React?

**Answer:**

**Declarative programming** means describing *what* the UI should look like for a given state, rather than writing step-by-step instructions (*how*) to mutate the DOM from one state to another.

**Declarative (React):**

```jsx
function Status({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? "Welcome back!" : "Please log in."}
    </div>
  );
}
```

**Imperative (Vanilla DOM):**

```javascript
const container = document.getElementById("status");
if (isLoggedIn) {
  container.textContent = "Welcome back!";
} else {
  container.textContent = "Please log in.";
}
```

**Why is declarative rendering useful?**
In a complex screen (like an e-commerce checkout with promo codes, address validation, payment method toggles, and loading spinners), imperative code requires tracking every possible transition between states ($N^2$ transitions), which frequently leads to UI desynchronization bugs. Declarative React code makes the UI a predictable function of current state: $\text{UI} = f(\text{state})$.

---

### Q5. What is the difference between React and JavaScript?

**Answer:**

JavaScript (ECMAScript) is a general-purpose programming language executed by browser engines (V8, SpiderMonkey, JavaScriptCore) and runtimes like Node.js. React is a UI library written in JavaScript.

| JavaScript | React |
|---|---|
| A programming language | A UI library built in JavaScript |
| Manipulates the browser DOM imperatively (`document.querySelector`) | Provides a declarative component and state model for UI rendering |
| Provides primitives, closures, prototypes, arrays, promises, and `async/await` | Provides components, JSX elements, Hooks (`useState`, `useEffect`), and reconciliation |
| Runs independently without React | Depends entirely on JavaScript |

React does **not** replace JavaScript. Strong React engineers rely heavily on core JavaScript mechanisms: lexical closures (which power Hooks), referential equality (`Object.is`), array methods (`map`, `filter`, `reduce`), destructuring, ES modules, and the asynchronous event loop.

---

## Topic 2: React Environment and Project Setup

### Q6. What tools are commonly used to create a React application?

**Answer:**

A modern React development environment typically includes:
- **Node.js:** JavaScript runtime used outside the browser to run build tools, dev servers, and test runners.
- **Package Manager (`npm`, `pnpm`, or `yarn`):** Installs and manages packages and lockfiles.
- **Vite:** Fast frontend build tool providing instant ES module dev server startup, Hot Module Replacement (HMR), and optimized Rollup production builds.
- **`react`:** The core React library (components, Hooks, elements).
- **`react-dom`:** Renderer package that connects React to the browser DOM (`react-dom/client`).
- **Code Editor:** Visual Studio Code with ESLint, Prettier, and TypeScript extensions.
- **Browser DevTools & React Developer Tools:** Browser extensions for inspecting the component tree, hooks state, and render performance.

**Creating a new React project with Vite:**

```bash
# JavaScript template
npm create vite@latest my-react-app -- --template react

# Or TypeScript template (Recommended for production)
npm create vite@latest my-react-app -- --template react-ts

cd my-react-app
npm install
npm run dev
```

*(Note: Legacy `create-react-app` (CRA) is deprecated. For new standalone React SPAs, Vite is the standard tool; for full-stack/SSR apps, Next.js or React Router v7/Remix is standard.)*

---

### Q7. What is the role of `package.json`?

**Answer:**

`package.json` is the manifest file at the root of a JavaScript/Node.js project. It defines:
- **Project metadata:** `name`, `version`, `private`, and module type (`"type": "module"`).
- **`dependencies`:** Packages required at runtime in production (e.g., `react`, `react-dom`, `react-router-dom`).
- **`devDependencies`:** Packages required only during development, testing, and building (e.g., `vite`, `typescript`, `eslint`, `vitest`).
- **`scripts`:** CLI commands aliased for starting the dev server, building, linting, and testing.

**Example:**

```json
{
  "name": "my-react-app",
  "private": true,
  "version": "0.1.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint .",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.3.0",
    "vite": "^6.0.0"
  }
}
```

**Follow-up:** *What is the difference between `package.json` and `package-lock.json`?*
`package.json` specifies allowed version ranges (e.g., `^19.0.0` allows minor and patch updates), whereas `package-lock.json` locks the **exact version and cryptographic integrity hash** of every direct and transitive dependency so every developer and CI/CD server installs the exact same dependency tree (`npm ci`).

---

### Q8. What is the difference between `npm` and `npx`?

**Answer:**

- **`npm` (Node Package Manager):** Installs, updates, and manages dependencies and runs scripts defined in `package.json`.
- **`npx` (Node Package Execute):** Executes a CLI binary from `./node_modules/.bin` (locally installed) or temporarily downloads and runs a package generator without polluting global installations.

**Examples:**

```bash
# Install project dependencies listed in package.json
npm install

# Run a locally installed binary or one-off CLI tool
npx vite --version
npx playwright test
```

---

### Q9. What is the purpose of `src`, `public`, and `index.html` in a Vite React project?

**Answer:**

- **`index.html`:** The root HTML entry point located at the project root in Vite. It contains `<div id="root"></div>` (where React mounts) and `<script type="module" src="/src/main.jsx"></script>`.
- **`src/`:** Contains all source code processed by the bundler (React components, Hooks, TypeScript/JS modules, CSS, and imported images that get hashed and optimized during build).
- **`src/main.jsx` (or `main.tsx`):** The JavaScript entry point that calls `createRoot(document.getElementById('root')).render(<App />)`.
- **`public/`:** Contains static assets (such as `favicon.ico`, `robots.txt`, `manifest.json`) that are copied verbatim to the root of the production `dist/` folder without bundling or filename hashing.

**Typical Vite project structure:**

```text
my-react-app/
├── public/
│   └── favicon.ico
├── src/
│   ├── assets/
│   ├── components/
│   ├── hooks/
│   ├── App.jsx
│   ├── App.css
│   └── main.jsx
├── index.html
├── package.json
└── vite.config.js
```

---

### Q10. What happens when you run `npm run dev`?

**Answer:**

In a Vite React project:
1. `npm` looks up `"dev"` in `package.json` `"scripts"` and executes `vite`.
2. Vite starts an HTTP development server (commonly on `http://localhost:5173`).
3. When you open the URL in a browser, Vite serves `index.html`.
4. The browser parses `<script type="module" src="/src/main.jsx"></script>` and requests `/src/main.jsx`.
5. Vite transforms JSX/TypeScript on the fly using `esbuild` / Babel and serves native ES modules to the browser.
6. `main.jsx` executes `createRoot(document.getElementById("root")).render(<App />)`.
7. React invokes the `App` component and its children, builds the initial fiber tree, and commits the resulting DOM nodes inside `#root`.
8. When you edit and save a component file, Vite uses **Hot Module Replacement (HMR)** with **React Fast Refresh** over a WebSocket connection to swap only the modified module in the browser while preserving existing component state!

---

## Topic 3: Components

### Q11. What is a React component?

**Answer:**

A React component is a reusable, self-contained JavaScript function (or class) that accepts inputs (`props`), manages local state, and returns React elements (typically written in JSX) describing a section of the UI.

```jsx
function Header() {
  return <h1>My Application</h1>;
}

function App() {
  return (
    <div>
      <Header />
      <p>Welcome to the application.</p>
    </div>
  );
}

export default App;
```

Here, `Header` and `App` are components, and `App` composes `Header` inside its output.

---

### Q12. What is the difference between a function component and a class component?

**Answer:**

Both define React components, but they differ in syntax, state management, and mental model:

| Dimension | Function Components | Class Components |
|---|---|---|
| **Syntax** | Plain JavaScript functions | ES6 classes extending `React.Component` |
| **State Management** | `useState` / `useReducer` Hooks | `this.state` object and `this.setState()` |
| **Side Effects & Lifecycle** | `useEffect` (synchronization model) | `componentDidMount`, `componentDidUpdate`, `componentWillUnmount` |
| **`this` Binding** | No `this` keyword required | Requires careful `this` binding for event handlers |
| **Logic Reuse** | Custom Hooks (`useAuth`, `useDebounce`) | Higher-Order Components (HOCs) or Render Props |
| **Captured Values** | Captures the exact props/state of the render via closure | `this.props` mutates over time across async callbacks |
| **Exceptions** | Cannot catch render errors directly (uses wrapper) | Can implement `static getDerivedStateFromError` and `componentDidCatch` (Error Boundaries) |

**Function component:**

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount((c) => c + 1)}>
      Count: {count}
    </button>
  );
}
```

**Class component:**

```jsx
import { Component } from "react";

class Counter extends Component {
  state = { count: 0 };

  render() {
    return (
      <button
        onClick={() =>
          this.setState(({ count }) => ({ count: count + 1 }))
        }
      >
        Count: {this.state.count}
      </button>
    );
  }
}
```

**Interview tip:** Function components with Hooks are the modern standard for all new React code, while Error Boundaries are the one remaining feature whose underlying implementation still uses a class component (usually wrapped via libraries like `react-error-boundary`).

---

### Q13. Why must React component names begin with a capital letter?

**Answer:**

The JSX compiler uses the **first letter's case** to distinguish built-in HTML/SVG DOM tags from custom React component references:
- **Lowercase tags (`<div>`, `<span>`, `<profile />`):** Passed as a string literal (`"div"`, `"profile"`) to `_jsx("profile", {})`, telling React to create a built-in HTML element `<profile>` in the DOM.
- **Capitalized tags (`<Profile />`, `<UserCard />`):** Compiled as a direct JavaScript identifier reference `_jsx(Profile, {})`, telling React to invoke the `Profile` component function.

**Correct:**

```jsx
function Profile() {
  return <h2>User Profile</h2>;
}

function App() {
  return <Profile />;
}
```

**Rule:** Always use **PascalCase** (`UserProfile`, `ProductCard`, `ExamDashboard`) for React components.

---

### Q14. What is component composition?

**Answer:**

Component composition is the practice of building complex UIs by combining smaller, focused components together—passing components as props or nesting them via the special `children` prop—rather than using class inheritance.

```jsx
function Button({ children, onClick }) {
  return (
    <button className="btn" onClick={onClick}>
      {children}
    </button>
  );
}

function Card({ title, actions, children }) {
  return (
    <section className="card">
      <header className="card-header">
        <h2>{title}</h2>
        {actions}
      </header>
      <div className="card-body">{children}</div>
    </section>
  );
}

function App() {
  return (
    <Card
      title="Student Details"
      actions={<Button onClick={() => alert("Edit")}>Edit</Button>}
    >
      <p>Name: Binil</p>
      <Button onClick={() => alert("Profile")}>View Profile</Button>
    </Card>
  );
}
```

**Benefits of composition:**
1. Avoids brittle inheritance hierarchies.
2. Container components (`Card`, `Modal`, `Layout`) don't need to know their inner content ahead of time.
3. Reduces prop drilling when intermediate layout components accept `{children}` directly instead of forwarding dozens of data props.

---

### Q15. What is the difference between a Component, a React Element, and a Component Instance / Fiber?

**Answer:**

- **Component:** The blueprint—a function or class definition (`function Greeting() { return <h1>Hello</h1>; }`).
- **React Element:** A lightweight, immutable plain JavaScript object returned by JSX (`<Greeting />`) that describes *what* should be rendered:
  ```javascript
  {
    $$typeof: Symbol.for("react.element"),
    type: Greeting,
    key: null,
    props: {}
  }
  ```
- **Fiber (Internal Instance):** The stateful internal work node created and managed by React's reconciler in memory to track a specific mounted occurrence of that component, its Hooks linked list, and its associated real DOM node.

---

# Part 3: Scenario-Based Questions — Fundamentals

### Scenario 1: A button appears on the page, but clicking it does nothing. What would you check?

**Answer:**

1. **Check whether the function is passed as a reference vs invoked during render:**
   - *Bug:* `<button onClick={handleClick()}>Click Me</button>` invokes `handleClick()` immediately **during rendering** and passes its return value (`undefined`) to `onClick`. Clicking the button later does nothing.
   - *Fix:* Pass the function reference: `<button onClick={handleClick}>Click Me</button>`, or wrap it in an arrow function when passing arguments: `<button onClick={() => handleClick(10)}>Click Me</button>`.
2. **Check if it is a custom component that forgot to forward `onClick`:**
   - `<CustomButton onClick={handleClick} />` only works if `CustomButton` actually destructures `onClick` from props and attaches it to the underlying native `<button onClick={onClick}>`.
3. **Check CSS pointer traps or `disabled` attributes:**
   - Inspect in Browser DevTools whether `disabled` is truthy, `pointer-events: none` is applied, or an invisible overlay (`z-index`) is covering the button.

---

### Scenario 2: You created a component, but React displays an error saying it cannot find the component (or `Element type is invalid`). What could be wrong?

**Answer:**

Common causes include:
1. **Default vs Named Export mismatch** (causes `Element type is invalid: expected a string or a class/function but got: undefined`):
   ```jsx
   // Header.jsx (Default export)
   export default function Header() {
     return <h1>Dashboard</h1>;
   }
   // Correct import:
   import Header from "./Header";
   // WRONG import (imports undefined!):
   import { Header } from "./Header";
   ```
   Conversely, if `Header.jsx` uses a **named export** (`export function Header()`), it must be imported with braces: `import { Header } from "./Header";`.
2. **Incorrect relative path or case sensitivity** (`./components/header` vs `./components/Header`).
3. **Circular dependency** between two files importing each other synchronously.

---

### Scenario 3: A parent component renders three instances of the same child component. Do all instances share their state?

**Answer:**

**No.** State in React is isolated to each component's position in the render tree. Rendering `<Counter />` three times creates three separate Fiber nodes, each with its own independent `useState` memory slot:

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount((c) => c + 1)}>
      {count}
    </button>
  );
}

function App() {
  return (
    <>
      <Counter />
      <Counter />
      <Counter />
    </>
  );
}
```

If all three counters *must* display and update the exact same shared number, **lift the state up** into their closest common parent (`App`) and pass `count` and `onIncrement` down via props.

---

### Scenario 4: Your React component displays `[object Object]` or crashes with `Objects are not valid as a React child`. Why?

**Answer:**

1. **`Objects are not valid as a React child`:** React can render primitives (`string`, `number`), JSX elements, arrays of elements, or portals—not plain JavaScript objects (`{ name: "Binil" }`).
   - *Bug:* `<div>{user}</div>`
   - *Fix:* Render specific primitive properties (`<div>{user.name}</div>`), or format as JSON for debugging (`<pre>{JSON.stringify(user, null, 2)}</pre>`).
2. **`[object Object]` string output:** Occurs when an object is coerced to a string via template literals or string concatenation before rendering, such as `` <div>{`User: ${user}`}</div> `` or `href={urlObject}`.

---

### Scenario 5: A developer places an entire application inside one enormous 3,000-line component. What problems can arise?

**Answer:**

1. **Performance degradation (unnecessary re-renders):** Every keystroke in a single search input updates state at the top of the monolith and forces the entire 3,000-line tree to re-render.
2. **Cognitive overload & merge conflicts:** Multiple developers editing the same file constantly collide in Git.
3. **Impossible to unit test or reuse in isolation:** You cannot test the pagination control or student table without mounting the entire dashboard and all its API dependencies.
4. **Refactoring Strategy:** Decompose by **Single Responsibility and State Colocation**:
   ```text
   ExamDashboard
   ├── DashboardHeader
   ├── ExamSummary
   ├── UpcomingExams
   ├── StudentFilterBar   (colocates local filter input state)
   ├── StudentTable
   └── Pagination
   ```

---

# Part 4: Your Interview Preparation Plan

We work through the 80-topic master checklist across 13 structured phases:

| Phase | Topics Covered | Core Engineering Goal |
|:---:|---|---|
| **Phase 1** | Topics 1–7: Fundamentals, JS Prerequisites, Setup, Project Structure, Components, JSX, Props | Build a rock-solid foundation in React's component and rendering model |
| **Phase 2** | Topics 8–11: State (`useState`), Events, Conditional Rendering, Lists & Keys | Create interactive, bug-free stateful user interfaces |
| **Phase 3** | Topics 12–15, 21, 28: Component Lifecycle, Hooks Rules, `useEffect`, `useRef`, Custom Hooks, Strict Mode | Master synchronization, closures, refs, and custom stateful logic |
| **Phase 4** | Topics 22–27: Forms, Advanced Forms, `forwardRef`/`useImperativeHandle`, Portals, Error Boundaries, Fragments | Build resilient forms, modals, imperative wrappers, and fault boundaries |
| **Phase 5** | Topics 35–39: Routing, Protected Routes, API Integration, Asynchronous UI, Data Fetching | Architect complete multi-page client applications with race-condition-free networking |
| **Phase 6** | Topics 16–17, 40–43: `useContext`, `useReducer`, State Architecture, Redux Toolkit, TanStack Query, Zustand | Master local, global client, and asynchronous server state management |
| **Phase 7** | Topics 18–20, 29–34, 44–46: `useMemo`, `useCallback`, `React.memo`, Fiber Internals, Virtual DOM, React 18/19, React Compiler, Code Splitting, Virtualization | Diagnose rendering bottlenecks and understand React's Fiber engine deeply |
| **Phase 8** | Topics 47–50: TypeScript with React, Styling Architectures, UI Component Systems, Accessibility (a11y) | Build type-safe, accessible, design-system-grade interfaces |
| **Phase 9** | Topics 51–56, 71–72: Unit/Integration Testing, E2E Testing, Security (XSS/CSRF), Authentication, File Operations, i18n, Debugging, Common Errors | Ensure enterprise production quality, security, and test coverage |
| **Phase 10** | Topics 59–63: SSR, SSG/ISR, React Server Components (RSC), Next.js/Frameworks, Suspense & Streaming | Master modern full-stack rendering architectures |
| **Phase 11** | Topics 64–70: Core Web Vitals, Vite/Build Tools, Env Config, Deployment, Docker/Nginx, CI/CD, Git Workflows | Package, containerize, optimize, and deploy React apps to production |
| **Phase 12** | Topics 57–58, 73–74, 76–80: Architecture, Design Patterns, Frontend System Design, Real-World Scenarios, JS/Browser/Network Deep Dive, Version Differences | Excel in Senior/Staff Frontend System Design and architectural interviews |
| **Phase 13** | Topic 75: Practical Coding Challenges & Complete Production Implementations | Write clean, bug-free React code under live coding interview constraints |

---

# Part 5: Phase 1 Deep Dive — JavaScript Prerequisites, Component Purity, JSX & Props (Topics 2, 5, 6, 7)

## Topic 2: JavaScript Prerequisites for React Interviews

Before diving into JSX and Hooks, interviewers frequently test the exact JavaScript mechanics that React relies on.

### Q16. Which JavaScript concepts are most critical for understanding how React works?

**Answer:**

1. **Destructuring & Default Values:** Used constantly when extracting props and Hook return values:
   ```javascript
   // Object destructuring with renaming and default value
   const { title, count = 0, author: authorName } = props;

   // Array destructuring (allows naming state variables anything you want)
   const [isOpen, setIsOpen] = useState(false);
   ```
2. **Spread (`...`) & Rest (`...`) Operators for Immutability:** React relies on shallow reference comparisons (`Object.is(prevState, nextState)`) to detect state changes. Spread syntax creates shallow copies:
   ```javascript
   // Updating nested object immutably
   setUser((prev) => ({
     ...prev,
     address: { ...prev.address, city: "Bengaluru" }
   }));

   // Forwarding remaining props to a native element
   function Input({ label, ...restProps }) {
     return (
       <label>
         {label}
         <input {...restProps} />
       </label>
     );
   }
   ```
3. **Lexical Closures:** Every time a function component renders, event handlers and `useEffect` callbacks declared inside that render **close over** the `props` and `state` constants from that specific render snapshot. Misunderstanding closures is the #1 cause of **stale closure bugs** in React interviews.
4. **Short-Circuit Evaluation & Nullish Coalescing (`??`) vs Logical OR (`||`):**
   - `count || 10` evaluates to `10` when `count` is `0` (because `0` is falsy!).
   - `count ?? 10` evaluates to `0` when `count` is `0`, falling back to `10` **only** when `count` is `null` or `undefined`.
5. **Array Non-Mutating Methods:**
   - **Never mutate state arrays in place** (`push`, `pop`, `splice`, `sort`, `reverse`).
   - Always use non-mutating methods that return a new array: `map`, `filter`, `slice`, `concat`, `[...arr]`, or ES2023 immutable methods (`toSorted()`, `toReversed()`, `toSpliced()`, `with()`).

---

## Topic 5 (Continued): Component Purity

### Q17. What does it mean that a React component must be a "pure function" during rendering?

**Answer:**

In React, **rendering must be pure**:
1. **Minds its own business:** A component must not mutate any variables or objects that existed *before* it was called (such as global variables, `document.title`, `localStorage`, or input props).
2. **Same inputs, same output:** Given the exact same `props`, `state`, and `context`, a component should always return the exact same JSX output.

**Impure Component (Buggy—breaks in Strict Mode and Concurrent Rendering):**

```jsx
let guestCount = 0; // External variable outside React!

function Cup() {
  guestCount = guestCount + 1; // Side effect during render!
  return <h2>Tea cup for guest #{guestCount}</h2>;
}
```

**Pure Component (Correct):**

```jsx
function Cup({ guestNumber }) {
  return <h2>Tea cup for guest #{guestNumber}</h2>;
}
```

*Note:* Side effects (DOM mutations, timers, network calls, updating state) belong inside **Event Handlers** (when triggered by user actions) or **`useEffect`** (when triggered by synchronization after commit)—never directly in the top-level body of a component during render.

---

## Topic 6: JSX in Depth

### Q18. What is JSX, and can browsers read it directly?

**Answer:**

**JSX (JavaScript XML)** is a syntax extension for JavaScript that lets you write HTML-like markup directly inside JavaScript files.

**Browsers cannot execute JSX directly.** Build tools like Vite (using esbuild/SWC/Babel) compile JSX into standard JavaScript function calls before code reaches the browser.

**How the JSX Transformation works:**

- **Classic JSX Transform (React 16 and earlier):**
  Required `import React from "react"` at the top of every file because JSX compiled to `React.createElement(type, props, ...children)`.
- **Modern Automatic JSX Runtime (React 17+, default in React 18 & 19):**
  Automatically imports `_jsx` from `"react/jsx-runtime"` at build time—you no longer need `import React from "react"` just to write JSX!

```jsx
// What you write (JSX):
const element = <h1 className="greeting">Hello, Binil!</h1>;

// What the compiler outputs (Modern React 17+ JSX Runtime):
import { jsx as _jsx } from "react/jsx-runtime";
const element = _jsx("h1", {
  className: "greeting",
  children: "Hello, Binil!"
});
```

---

### Q19. What is the difference between a JavaScript Expression and a Statement inside JSX `{}`?

**Answer:**

Inside JSX curly braces `{ ... }`, you can only embed **JavaScript expressions** (anything that evaluates to a value), **never statements** (`if/else`, `for`, `switch`, `while`, variable declarations).

| Valid in JSX `{}` (Expressions) | Invalid in JSX `{}` (Statements) |
|---|---|
| `{user.name}` | `{if (isAdmin) { return <p>Admin</p>; }}` |
| `{2 + 2}` | `{for (let i = 0; i < 5; i++) {}}` |
| `{isLoggedIn ? <Dashboard /> : <Login />}` | `{let x = 10;}` |
| `{items.map(item => <li key={item.id}>{item.name}</li>)}` | `{switch (status) { ... }}` |

---

### Q20. Why do JSX attributes use `className`, `htmlFor`, and camelCase (`onClick`, `tabIndex`) instead of standard HTML attribute names?

**Answer:**

Because JSX compiles into JavaScript objects and maps to **DOM JavaScript properties** rather than raw HTML attribute strings, and certain HTML attribute names (`class`, `for`) are reserved keywords in JavaScript:
- HTML `class` -> JSX `className`
- HTML `for` (on `<label>`) -> JSX `htmlFor`
- HTML `tabindex` -> JSX `tabIndex`
- HTML `onclick` -> JSX `onClick`
- HTML `stroke-width` (SVG) -> JSX `strokeWidth`
- Exceptions: `aria-*` (`aria-label`, `aria-expanded`) and `data-*` (`data-testid`) keep their standard hyphenated syntax in JSX!

---

### Q21. Why must a JSX component return a single root element, and how does `<Fragment>` (`<>...</>`) solve this?

**Answer:**

Because a JSX tag compiles into a function call (`_jsx(...)`), and a JavaScript function can only return **one value** (you cannot return two separate objects from a function without wrapping them in a parent container or array).

Wrapping sibling elements in an unnecessary `<div>` can break CSS Flexbox/Grid layouts or invalid HTML structures (such as inside `<table><tr>` where a wrapper `<div>` around `<td>` cells is invalid HTML).

**`Fragment` (`<React.Fragment>` or shorthand `<>...</>`)** groups multiple elements without inserting any extra DOM node into the browser:

```jsx
import { Fragment } from "react";

// Shorthand syntax (most common):
function TableColumns() {
  return (
    <>
      <td>Column 1</td>
      <td>Column 2</td>
    </>
  );
}

// Explicit Fragment syntax (required when passing a `key` inside .map()!):
function Glossary({ items }) {
  return (
    <dl>
      {items.map((item) => (
        <Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </Fragment>
      ))}
    </dl>
  );
}
```

**Common Interview Trap:** *Can you pass a `key` prop to the shorthand `<>...</>` fragment?*
**No!** Shorthand `<>` does not accept any attributes. When rendering a list of fragments inside `.map()`, you must use `<Fragment key={item.id}>...</Fragment>`.

---

### Q22. What values render nothing in JSX, and what is the famous `0` rendering trap?

**Answer:**

- **Values that render nothing at all in JSX:** `false`, `null`, `undefined`, and `true`.
- **Values that DO render visibly as text in JSX:** `0`, `-0`, `NaN`, and `""` (empty string renders an empty text node).

**The Famous `&&` Zero Trap (Tested in 80% of React Interviews):**

```jsx
// BUG: When items.length is 0, React renders the literal number "0" on screen!
function NotificationBadge({ items }) {
  return (
    <div>
      {items.length && <span>You have {items.length} messages</span>}
    </div>
  );
}
```

**Why does this happen?**
In JavaScript, `0 && <span>...</span>` short-circuits and evaluates to `0` (because `0` is falsy). React then sees `{0}` as a child and renders `"0"` into the DOM!

**Correct Fixes:**

```jsx
// Fix 1: Explicit boolean comparison (Recommended)
{items.length > 0 && <span>You have {items.length} messages</span>}

// Fix 2: Ternary returning null
{items.length ? <span>You have {items.length} messages</span> : null}
```

---

## Topic 7: Props and One-Way Data Flow

### Q23. What are Props in React, and why are they read-only (immutable)?

**Answer:**

**Props (properties)** are the input arguments passed from a parent component down to a child component. Technically, React collects all JSX attributes and nested children into a single `props` object passed as the first parameter to the component function.

**Props are strictly read-only (immutable).** A child component must never modify its own `props` (e.g., `props.count = 10;`). This enforces **unidirectional (one-way) data flow** and component purity: if data on screen is wrong, you can trace it straight up the component tree to the single parent component that owns the state.

```jsx
// Default prop values using ES6 parameter defaults:
function Avatar({ username = "Guest", size = 48, isOnline = false }) {
  return (
    <div className="avatar" style={{ width: size, height: size }}>
      <span>{username[0].toUpperCase()}</span>
      {isOnline && <span className="status-dot" />}
    </div>
  );
}
```

---

### Q24. How can a child component communicate or send data back to its parent component?

**Answer:**

Even though data flows top-down via props, a parent can pass a **callback function as a prop** down to the child. When an event occurs inside the child, the child calls that prop function with the new data as an argument:

```jsx
import { useState } from "react";

function SearchInput({ onSearchChange }) {
  return (
    <input
      type="search"
      placeholder="Search products..."
      onChange={(e) => onSearchChange(e.target.value)}
    />
  );
}

function ProductCatalog() {
  const [query, setQuery] = useState("");

  return (
    <div>
      <SearchInput onSearchChange={setQuery} />
      <p>Searching for: {query}</p>
    </div>
  );
}
```

---

### Q25. What is Prop Drilling, and what are the best ways to solve it without overusing Context or Redux?

**Answer:**

**Prop drilling** occurs when you pass props down through multiple intermediate layers of components (`App -> Dashboard -> MainContent -> Sidebar -> UserMenu -> Avatar`) that do not use the data themselves, solely so a deeply nested child can read it.

**How to solve Prop Drilling (in order of preference):**

1. **Component Composition (`children` or slot props — Best First Choice!):**
   Instead of `Dashboard` rendering `Sidebar` which renders `UserMenu` which renders `Avatar` and passing `user` through every layer, render `<Avatar user={user} />` directly inside `App` and pass the JSX element down as `children` or a prop (`userMenu={<Avatar user={user} />}`)! Intermediate layout components never touch `user`.
2. **React Context (`useContext`):**
   Ideal for low-frequency global or subtree-scoped concerns like `Theme`, `Locale`, `CurrentAuthenticatedUser`, or Compound Component state.
3. **External State Stores (Zustand, Redux Toolkit) or Server State (TanStack Query):**
   Ideal when high-frequency state or cached API data is needed across distant branches of the tree.

---

# Part 6: Phase 2 Deep Dive — State (`useState`), Events, Conditional Rendering, Lists & Keys (Topics 8, 9, 10, 11)

## Topic 8: State (`useState`, Batching, Functional Updates, Derived State)

### Q26. What is the difference between Props and State in React?

**Answer:**

| Dimension | Props | State |
|---|---|---|
| **Ownership** | Passed **into** the component from its parent (like function parameters) | Created and managed **inside** the component via `useState` / `useReducer` (like local memory remembered across renders) |
| **Mutability** | Strictly read-only to the receiving component | Updated by the component that owns it via its state setter (`setState`) |
| **Triggering Re-renders** | Child re-renders when parent re-renders and passes new props | Calling `setState` with a new value (`!Object.is(prev, next)`) schedules a re-render |

---

### Q27. Why does calling `setCount(count + 1)` three times in the same event handler only increment `count` by `1`?

**Answer:**

Consider this classic interview trap:

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  function handleTripleIncrement() {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
    console.log(count); // Still logs 0!
  }

  return <button onClick={handleTripleIncrement}>{count}</button>;
}
```

**Why does `count` become `1` instead of `3`, and why does `console.log(count)` print `0`?**

1. **State behaves like a snapshot per render:** During the current render where `count` is `0`, the constant `count` inside `handleTripleIncrement` is fixed at `0`. Calling `setCount` does **not** mutate the `count` variable in the already-running function closure; it schedules a new render with the updated state.
2. **Replacing vs Updating:** Therefore, all three calls execute `setCount(0 + 1)`, telling React three times: *"Set the state for the next render to `1`"*.
3. **Automatic Batching:** React batches all three updates inside the event handler into a single re-render at the end of the event handler.

**The Fix: Functional State Updates (`c => c + 1`)**

When the next state depends on the previous state, pass an **updater function** to `setCount`:

```jsx
function handleTripleIncrement() {
  setCount((prevCount) => prevCount + 1); // 0 -> 1
  setCount((prevCount) => prevCount + 1); // 1 -> 2
  setCount((prevCount) => prevCount + 1); // 2 -> 3
}
```

React places the updater functions in a queue and passes the return value of each updater as the `prevCount` argument to the next updater during the next render, resulting in `3`.

---

### Q28. What is Lazy State Initialization in `useState`, and when should you use it?

**Answer:**

Compare these two calls:

```jsx
// 1. Direct expression (Runs on EVERY render, even though React only uses it on mount!)
const [todos, setTodos] = useState(
  JSON.parse(localStorage.getItem("todos") || "[]")
);

// 2. Lazy initializer function (Runs ONLY ONCE on initial mount!)
const [todos, setTodos] = useState(() => {
  return JSON.parse(localStorage.getItem("todos") || "[]");
});
```

When you pass a **function** `() => computeInitialState()` to `useState(initFn)`, React only calls `initFn` once during the initial mount and ignores it on all subsequent re-renders. Use lazy initialization whenever computing initial state involves reading `localStorage`, parsing JSON, or filtering/building a large array.

---

### Q29. What is "Derived State," and why is storing derived values in `useState` + `useEffect` an anti-pattern?

**Answer:**

**Anti-Pattern (Redundant State + Extra Re-render):**

```jsx
function CartSummary({ items }) {
  const [total, setTotal] = useState(0);

  // BAD: Causes an unnecessary double-render and temporary out-of-sync UI!
  useEffect(() => {
    setTotal(items.reduce((sum, item) => sum + item.price * item.qty, 0));
  }, [items]);

  return <div>Total: ₹{total}</div>;
}
```

**Correct Pattern (Compute Derived State Directly During Render):**

```jsx
function CartSummary({ items }) {
  // GOOD: Computed synchronously during render; always 100% in sync!
  const total = items.reduce((sum, item) => sum + item.price * item.qty, 0);

  return <div>Total: ₹{total}</div>;
}
```

*(If `items` has 50,000 elements and the calculation is CPU-expensive, wrap the calculation in `useMemo`—still never in `useState` + `useEffect`!)*

---

## Topic 9: Events and React's SyntheticEvent System

### Q30. What is a `SyntheticEvent` in React, and how does event delegation work in React 17/18/19?

**Answer:**

- **`SyntheticEvent`:** React wraps native browser events (`MouseEvent`, `KeyboardEvent`, `InputEvent`) in a cross-browser wrapper called `SyntheticEvent` that normalizes browser quirks while providing the standard W3C interface (`e.stopPropagation()`, `e.preventDefault()`, `e.target`, `e.currentTarget`, and `e.nativeEvent`).
- **Event Delegation at the Root Container:**
  React does **not** attach native DOM event listeners to every individual `<button>` or `<input>` node in the DOM. Instead:
  - In **React 16 and earlier**, React attached top-level listeners to `document`.
  - In **React 17, 18, and 19**, React attaches delegated event listeners to the **root DOM container** (`#root` passed to `createRoot(rootNode)`). This prevents conflicts when multiple React roots or third-party scripts coexist on the same page.
- **Capture vs Bubble Phase in JSX:**
  - Normal event props (`onClick`, `onKeyDown`) run during the **Bubbling phase** (bottom-up).
  - Appending `Capture` (`onClickCapture`, `onFocusCapture`) registers the handler for the **Capture phase** (top-down).

---

## Topic 10: Conditional Rendering

### Q31. What are the main ways to implement conditional rendering in React, and how do you handle multi-state async UIs cleanly?

**Answer:**

1. **Early Return (`if / else`)** — Best when an entire component output changes based on loading/error state:
   ```jsx
   function UserProfile({ isLoading, error, user }) {
     if (isLoading) return <Spinner />;
     if (error) return <ErrorBanner message={error.message} />;
     if (!user) return <EmptyState message="No user found." />;

     return <h1>{user.name}</h1>;
   }
   ```
2. **Ternary Operator (`condition ? <A /> : <B />`)** — Best for inline two-branch conditions inside JSX.
3. **Logical AND (`booleanCondition && <A />`)** — Best for showing or hiding an optional element (always ensure the left side is a true boolean `count > 0` to avoid rendering `0`).
4. **Lookup Object / Dictionary Map** — Best for replacing complex nested ternaries or `switch` statements based on status enums:
   ```jsx
   const STATUS_BADGE = {
     PENDING: <Badge color="yellow">Pending</Badge>,
     APPROVED: <Badge color="green">Approved</Badge>,
     REJECTED: <Badge color="red">Rejected</Badge>
   };

   function OrderStatus({ status }) {
     return STATUS_BADGE[status] ?? null;
   }
   ```

---

## Topic 11: Lists and Keys

### Q32. Why does React require a `key` prop when rendering lists, and why is using the array `index` as a `key` dangerous?

**Answer:**

**Why `key` exists:**
During reconciliation (diffing the previous element tree against the new element tree), `key` gives each element in an array a **stable identity across renders** so React can tell whether an item was added, removed, reordered, or updated.

**Why `key={index}` is dangerous when lists can be reordered, filtered, or have items inserted/deleted:**

Suppose you have a list of 3 `<TodoItem>` components, each containing an uncontrolled `<input>` or local `useState` for editing:
```text
Render 1 (Initial):
index=0 (key=0): "Buy Milk"    [input value typed by user: "2 liters"]
index=1 (key=1): "Pay Bills"   [input value: ""]
index=2 (key=2): "Read Book"   [input value: ""]
```
Now you **insert a new item `"Call Mom"` at the beginning** of the array (`unshift`):
```text
Render 2 (With key={index} — BUG!):
index=0 (key=0): "Call Mom"    [React reuses Fiber for key=0! Still holds input value "2 liters"!]
index=1 (key=1): "Buy Milk"    [React reuses Fiber for key=1! Input value is now ""!]
index=2 (key=2): "Pay Bills"
index=3 (key=3): "Read Book"   [React mounts key=3 as if it were the newly added item!]
```
Because `key={0}` remained at position `0`, React thinks the item with `key={0}` merely changed its text prop from `"Buy Milk"` to `"Call Mom"`, so it **keeps the existing DOM node and local component state** from `"Buy Milk"` attached to `"Call Mom"`, corrupting the UI!

**Rules for Keys:**
1. **Always use a stable, unique ID from your data:** `key={item.id}`.
2. **Never use `key={Math.random()}` or `key={crypto.randomUUID()}` generated inside render:** Generating a new random key on every render forces React to **destroy and recreate every DOM node and reset all child state** on every single render!
3. **When is `key={index}` acceptable?** Only when all three conditions hold: the list is static (never reordered, filtered, or prepended/deleted from the middle), the items have no unique IDs, and the child components are stateless.

---

### Q33. Pro Interview Trick: How can you intentionally use the `key` prop outside of lists to reset a component's state?

**Answer:**

Because React associates state with a component's `(type, key)` at its position in the UI tree, **changing a component's `key` prop forces React to unmount the old component instance (destroying all its internal `useState` and DOM state) and mount a brand-new instance from scratch!**

```jsx
// Instead of writing complex useEffect logic to reset form state when selectedUserId changes:
function ChatWindow({ selectedUserId }) {
  return <MessageComposer key={selectedUserId} recipientId={selectedUserId} />;
}
```
Whenever `selectedUserId` changes from `"user-1"` to `"user-2"`, `MessageComposer` gets a new `key`, cleanly resetting its draft text state and input focus without a single `useEffect`!

---

# Part 7: Phase 3 Deep Dive — Lifecycle, Hooks Rules, `useEffect`, `useRef`, Custom Hooks, Strict Mode (Topics 12, 13, 14, 15, 21, 28)

## Topic 12: Component Lifecycle in Functional Components

### Q34. What are the three phases of a React component's lifecycle, and how do they map to Functional Components?

**Answer:**

1. **Mounting (Birth):** The component is rendered for the first time, React creates its DOM nodes, inserts them into the browser DOM, and runs initial effects (`useEffect(() => { ... }, [])`).
2. **Updating (Growth):** Triggered when state changes (`useState` / `useReducer`), parent re-renders with new props, or consumed Context changes. React invokes the component function, reconciles differences, commits DOM updates, runs the **cleanup function** of previous effects (whose dependencies changed), and runs the new effects.
3. **Unmounting (Death):** The component is removed from the UI tree. React runs all remaining `useEffect` cleanup functions (`return () => { ... }`) before removing the DOM nodes.

> [!TIP]
> **Modern React Mental Model Shift:** Instead of thinking in class lifecycle methods (`componentDidMount` vs `componentDidUpdate`), think of `useEffect` as **synchronizing your component with an external system** (WebSocket, timer, DOM listener, non-React widget):
> - The Effect body **starts synchronization**.
> - The Effect cleanup function **stops synchronization**.

---

## Topic 13: Hooks Fundamentals & Rules of Hooks

### Q35. What are the two mandatory Rules of Hooks, and why does React break if you call a Hook inside an `if` statement?

**Answer:**

**The Two Rules of Hooks:**
1. **Only call Hooks at the top level** of your React function (never inside `if` conditions, `for` loops, nested functions, or after an early `return`).
2. **Only call Hooks from React function components or Custom Hooks** (never from regular JavaScript utility functions).

**Why does React require Hooks to be called in the exact same order on every render? (Internal Mechanism)**

Notice that when you call `useState(0)`, you do **not** pass a key or variable name to React:
```jsx
const [name, setName] = useState("Binil");   // Hook Slot 0
const [age, setAge] = useState(28);          // Hook Slot 1
const [role, setRole] = useState("Engineer");// Hook Slot 2
```
How does React know which `useState` call corresponds to `name`, `age`, or `role`?
Internally, each component's **Fiber node** stores a **singly linked list of `Hook` objects (`memoizedState`)** indexed purely by **call order** (`0 -> 1 -> 2`).

If you wrap Hook Slot 1 in a conditional:
```jsx
if (name !== "") {
  const [age, setAge] = useState(28); // Skipped when name === ""!
}
const [role, setRole] = useState("Engineer"); // Now lands in Slot 1 instead of Slot 2!
```
On the render where `name === ""`, the second `useState` is skipped, so `role` reads from **Hook Slot 1** (which holds `28` instead of `"Engineer"`), shifting the entire linked list and causing React to throw `Rendered fewer hooks than expected`!

---

## Topic 14: `useEffect` in Depth

### Q36. How do the three dependency array configurations of `useEffect` behave?

**Answer:**

```jsx
// 1. No dependency array at all:
// Runs after the initial mount AND after EVERY single re-render.
useEffect(() => {
  console.log("Runs after every render");
});

// 2. Empty dependency array []:
// Runs ONLY ONCE after initial mount (and twice in dev StrictMode to test cleanup).
useEffect(() => {
  console.log("Runs on mount");
  return () => console.log("Runs on unmount");
}, []);

// 3. With dependencies [userId, filter]:
// Runs on mount AND whenever Object.is(prevDep, nextDep) is false for any item.
useEffect(() => {
  console.log("Runs when userId or filter changes");
  return () => console.log("Cleans up previous userId/filter before next effect runs");
}, [userId, filter]);
```

---

### Q37. When exactly does the `useEffect` cleanup function run?

**Answer:**

Many candidates mistakenly answer *"Only when the component unmounts."* That is incomplete!

A `useEffect` cleanup function runs at **two** times:
1. **Before every re-execution of the effect** when one of its dependencies changes (cleaning up the previous render's subscription/timer using the **previous render's closure values** before starting the new subscription).
2. **When the component finally unmounts** from the DOM.

```text
Mount (userId = 1)     --> Effect runs for userId = 1
Update (userId = 2)    --> 1. Cleanup runs for userId = 1
                       --> 2. Effect runs for userId = 2
Unmount                --> Cleanup runs for userId = 2
```

---

### Q38. What causes an infinite re-render loop in `useEffect`, and how do you fix it?

**Answer:**

**Cause 1: Updating state unconditionally inside `useEffect` while listing that state in the dependency array (or omitting the dependency array):**

```jsx
// INFINITE LOOP:
useEffect(() => {
  setCount(count + 1);
}, [count]);
```

**Cause 2: Putting an inline object, array, or function in the dependency array that gets re-allocated on every render:**

```jsx
function UserList({ role }) {
  // New object reference allocated on every render!
  const options = { role, activeOnly: true };

  useEffect(() => {
    fetchUsers(options); // If fetchUsers triggers setState, this loops forever!
  }, [options]); // Object.is(prevOptions, nextOptions) is ALWAYS false!
}
```

**Fixes for Cause 2:**
1. Move the object inside the `useEffect` callback, or depend on the primitive properties (`[role]`) instead of the wrapper object (`[options]`).
2. Or memoize the object/function with `useMemo` / `useCallback` if it is passed from props.

---

### Q39. What is the difference between `useEffect` and `useLayoutEffect`?

**Answer:**

| Dimension | `useEffect` | `useLayoutEffect` |
|---|---|---|
| **Execution Timing** | Runs **asynchronously after** the browser has painted the DOM changes to the screen | Runs **synchronously after** React mutates the DOM, **before** the browser paints the screen |
| **Blocks Browser Paint?** | No (non-blocking, smooth UI) | Yes (blocks painting until the callback finishes) |
| **When to Use** | 99% of effects (data fetching, analytics, subscriptions, document listeners) | Only when you must **measure DOM layout** (`getBoundingClientRect()`, scroll height) and synchronously adjust position/styles before paint to prevent a visible **visual flicker** (e.g., positioning a tooltip so it doesn't flash off-screen first) |

---

## Topic 15: `useRef`

### Q40. What are the two primary use cases for `useRef`, and how does `useRef` differ from `useState`?

**Answer:**

`useRef(initialValue)` returns a persistent mutable object `{ current: initialValue }` whose reference stays identical across all renders of the component.

| `useState` | `useRef` |
|---|---|
| Calling `setState(newValue)` **triggers a re-render** | Mutating `ref.current = newValue` **never triggers a re-render** |
| State value is immutable within a render snapshot | `ref.current` is mutable and should only be read/written inside event handlers or Effects (not during render) |
| Used for data that is rendered visibly in the UI | Used for DOM nodes, timer IDs, WebSocket instances, or tracking previous values behind the scenes |

**Use Case 1: Accessing a DOM Element Directly (Focus, Scroll, Media Playback)**

```jsx
import { useRef } from "react";

function SearchBox() {
  const inputRef = useRef(null);

  function handleFocusClick() {
    inputRef.current?.focus();
  }

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Type here..." />
      <button onClick={handleFocusClick}>Focus Input</button>
    </div>
  );
}
```

**Use Case 2: Storing a Timer ID (`setInterval` / `setTimeout`) Across Renders Without Re-rendering**

```jsx
import { useState, useRef } from "react";

function Stopwatch() {
  const [seconds, setSeconds] = useState(0);
  const intervalIdRef = useRef(null);

  function start() {
    if (intervalIdRef.current !== null) return;
    intervalIdRef.current = setInterval(() => {
      setSeconds((s) => s + 1);
    }, 1000);
  }

  function stop() {
    clearInterval(intervalIdRef.current);
    intervalIdRef.current = null;
  }

  return (
    <div>
      <p>Elapsed: {seconds}s</p>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </div>
  );
}
```

---

## Topic 21: Custom Hooks

### Q41. What is a Custom Hook, why must its name start with `use`, and do two components using the same Custom Hook share state?

**Answer:**

- **What it is:** A JavaScript function whose name starts with `use` (e.g., `useDebounce`, `useWindowSize`, `useLocalStorage`, `useFetch`) that calls one or more built-in Hooks to extract and reuse **stateful logic** across multiple components.
- **Why it must start with `use`:** React's ESLint plugin (`eslint-plugin-react-hooks`) and the React Compiler rely on the `use` prefix to enforce the Rules of Hooks (verifying that no Hooks are called conditionally inside it).
- **Do two components calling the same Custom Hook share state? NO!** Custom Hooks share **stateful logic**, not **state itself**. Every time Component A and Component B call `useCounter()`, the `useState` calls inside `useCounter()` attach to Component A's Fiber and Component B's Fiber independently.

**Production Example: `useDebounce` Custom Hook**

```jsx
import { useState, useEffect } from "react";

export function useDebounce(value, delayMs = 300) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timerId = setTimeout(() => {
      setDebouncedValue(value);
    }, delayMs);

    // If value changes before delayMs expires, cancel the previous timer!
    return () => clearTimeout(timerId);
  }, [value, delayMs]);

  return debouncedValue;
}
```

---

## Topic 28: Strict Mode (`<StrictMode>`)

### Q42. Why does my component render twice and `useEffect` run twice in development when using `<StrictMode>`?

**Answer:**

In **Development mode only** (never in production builds), `<React.StrictMode>` intentionally:
1. **Invokes component functions, initializers, and reducers twice** to detect impure rendering side effects.
2. **Runs `setup -> cleanup -> setup` once on initial mount for every `useEffect`** to verify that your Effect cleanup function properly undoes whatever the setup function did!

**Example of a bug caught immediately by `<StrictMode>`:**

```jsx
useEffect(() => {
  const ws = createWebSocketConnection(roomId);
  ws.connect();
  // BUG: Forgot to return () => ws.disconnect()!
  // In StrictMode, mount -> unmount -> mount leaves TWO active connections open,
  // exposing the missing cleanup immediately during local development!
}, [roomId]);
```

**Rule:** Never "fix" double-invocation in development by removing `<StrictMode>` or hacking around it with a `hasRunRef` guard for subscriptions—instead, implement a proper cleanup function or an `AbortController`!

---

# Part 8: Phase 4 Deep Dive — Forms, Imperative Refs, Portals & Error Boundaries (Topics 22, 23, 24, 25, 26)

## Topics 22 & 23: Controlled vs Uncontrolled Forms & Advanced Form Architecture

### Q43. What is the difference between Controlled and Uncontrolled components in React forms?

**Answer:**

| Dimension | Controlled Component | Uncontrolled Component |
|---|---|---|
| **Single Source of Truth** | **React State (`useState`)** holds the current value | **The DOM node itself** (`<input>`) holds the current value |
| **How Value is Read** | Directly from the `state` variable | Via `new FormData(e.currentTarget)` on submit or `inputRef.current.value` |
| **How Value is Updated** | `value={text}` + `onChange={e => setText(e.target.value)}` | Native browser input handling (`defaultValue="initial"`) |
| **Re-renders on Every Keystroke?** | Yes (the component owning the state re-renders on each change) | No (zero React re-renders while typing!) |
| **Best Use Cases** | Instant keystroke formatting (credit card masks), live dynamic disabling of buttons, dependent fields | High-performance 50-field forms (React Hook Form), simple submit-only forms, `<input type="file">` (which is always uncontrolled for security) |

**Controlled Multi-Input Form Pattern:**

```jsx
import { useState } from "react";

function RegistrationForm() {
  const [formData, setFormData] = useState({
    email: "",
    role: "developer",
    acceptTerms: false
  });

  function handleChange(e) {
    const { name, value, type, checked } = e.target;
    setFormData((prev) => ({
      ...prev,
      [name]: type === "checkbox" ? checked : value
    }));
  }

  function handleSubmit(e) {
    e.preventDefault(); // Prevents full-page browser reload!
    console.log("Submitting:", formData);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        name="email"
        type="email"
        value={formData.email}
        onChange={handleChange}
        required
      />
      <select name="role" value={formData.role} onChange={handleChange}>
        <option value="developer">Developer</option>
        <option value="architect">Architect</option>
      </select>
      <label>
        <input
          name="acceptTerms"
          type="checkbox"
          checked={formData.acceptTerms}
          onChange={handleChange}
        />
        Accept Terms
      </label>
      <button type="submit" disabled={!formData.acceptTerms}>
        Register
      </button>
    </form>
  );
}
```

**Why is React Hook Form preferred over plain `useState` for large enterprise forms?**
In a 60-field form managed with a single `useState` at the top of the form, every single keystroke in one input re-renders all 60 inputs. **React Hook Form** uses uncontrolled refs under the hood with isolated field subscriptions (`useWatch` / `Controller`), achieving `O(1)` zero-re-render typing performance while supporting declarative schema validation via **Zod** or **Yup**.

---

## Topic 24: Refs and Imperative APIs (`forwardRef`, React 19 `ref` Prop, `useImperativeHandle`)

### Q44. How do you pass a `ref` to a custom child component in React 18 vs React 19, and what does `useImperativeHandle` do?

**Answer:**

1. **In React 18 and earlier:** `ref` is not passed inside `props` by default. To let a parent component attach a `ref` to a DOM element inside a child component, the child must be wrapped in `React.forwardRef((props, ref) => ...)`.
2. **In React 19+:** `forwardRef` is no longer needed! Function components can accept `ref` directly as a regular prop:
   ```jsx
   // React 19+: ref is a standard prop!
   function TextInput({ label, ref, ...props }) {
     return (
       <label>
         {label}
         <input ref={ref} {...props} />
       </label>
     );
   }
   ```
3. **What `useImperativeHandle` does:** Instead of exposing the entire raw `<input>` DOM node to the parent (which lets the parent mutate styles or attributes arbitrarily), `useImperativeHandle` restricts the exposed `ref.current` object to a custom imperative API (such as `{ focus(), shake(), clear() }`):
   ```jsx
   import { useRef, useImperativeHandle, forwardRef } from "react";

   const CustomInput = forwardRef(function CustomInput(props, ref) {
     const internalInputRef = useRef(null);

     useImperativeHandle(ref, () => ({
       focus() {
         internalInputRef.current?.focus();
       },
       clear() {
         if (internalInputRef.current) {
           internalInputRef.current.value = "";
         }
       }
     }), []);

     return <input ref={internalInputRef} {...props} />;
   });
   ```

---

## Topic 25: Portals (`createPortal`)

### Q45. What is a React Portal, when should you use it, and how does event bubbling work through a Portal?

**Answer:**

`createPortal(children, domNode)` (from `react-dom`) renders a component's children into a **different physical DOM node outside the parent component's DOM hierarchy** (typically `document.body` or `<div id="modal-root">`), while keeping the component logically in its original position inside the **React Component Tree**.

**Why Portals are essential for Modals, Dialogs, Tooltips, and Dropdowns:**
If a `<Modal>` is rendered physically inside a deeply nested `<Card>` that has CSS `overflow: hidden`, `z-index: 1`, or `transform: translate3d(...)` (which creates a new CSS Stacking Context), the modal will be clipped or trapped behind other UI elements! Rendering the modal into `document.body` via `createPortal` escapes all parent CSS clipping and stacking contexts.

```jsx
import { createPortal } from "react-dom";

function Modal({ isOpen, onClose, children }) {
  if (!isOpen) return null;

  return createPortal(
    <div className="modal-backdrop" onClick={onClose}>
      <div
        className="modal-content"
        onClick={(e) => e.stopPropagation()}
        role="dialog"
        aria-modal="true"
      >
        {children}
      </div>
    </div>,
    document.body
  );
}
```

**Crucial Interview Trap: How does event bubbling work with Portals?**
Even though the Portal's HTML exists physically at the end of `document.body`, **React SyntheticEvents bubble up the React Component Tree**, not the physical DOM tree!
If `<Parent onClick={handleParentClick}>` renders a `<Modal>` via `createPortal`, clicking a button inside `<Modal>` will still trigger `handleParentClick` unless stopped via `e.stopPropagation()`!

---

## Topic 26: Error Handling & Error Boundaries

### Q46. What is an Error Boundary in React, what errors does it catch, and what errors does it NOT catch?

**Answer:**

By default, if an unhandled JavaScript error is thrown during rendering inside any child component, React unmounts the **entire application tree**, leaving the user with a blank white screen ("White Screen of Death").

An **Error Boundary** is a component that catches JavaScript errors anywhere in its child component subtree during rendering, logs those errors to an observability service (Sentry/Datadog), and displays a graceful fallback UI instead of crashing the whole page.

```jsx
import { Component } from "react";

export class ErrorBoundary extends Component {
  state = { hasError: false, error: null };

  // 1. Update state during render phase so next render shows fallback UI
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  // 2. Log error side-effect during commit phase
  componentDidCatch(error, errorInfo) {
    console.error("Captured by ErrorBoundary:", error, errorInfo.componentStack);
  }

  render() {
    if (this.state.hasError) {
      return (
        <div role="alert" className="error-fallback">
          <h2>Something went wrong in this widget.</h2>
          <button onClick={() => this.setState({ hasError: false, error: null })}>
            Try Again
          </button>
        </div>
      );
    }
    return this.props.children;
  }
}
```

**What Error Boundaries DO Catch:**
- Errors thrown during **rendering**, **lifecycle methods**, and **constructors** of the tree below them (plus Promises rejected inside `use()` or Server Actions when paired with Error Boundaries).

**What Error Boundaries DO NOT Catch (Common Interview Question):**
1. **Event handlers** (`onClick`, `onSubmit`): Because event handlers do not run during rendering, React already knows what to display on screen. Handle errors inside event handlers using standard `try / catch` and local error state (or pass the caught error to `useErrorBoundary()` from `react-error-boundary`).
2. **Asynchronous callbacks** (`setTimeout`, `requestAnimationFrame`, raw `.then()` callbacks).
3. **Server-Side Rendering (SSR)** errors inside the boundary itself.
4. **Errors thrown in the Error Boundary component itself** (rather than its children).

---

# Part 9: Phase 5 Deep Dive — Routing, Protected Routes, API Integration & Race-Condition-Free Data Fetching (Topics 35, 36, 37, 38, 39)

## Topics 35 & 36: Client-Side Routing & Protected Routes (React Router)

### Q47. How does Client-Side Routing work in a Single-Page Application (SPA) without triggering a full browser page reload?

**Answer:**

When a user clicks a `<Link to="/dashboard">` in React Router:
1. React Router intercepts the native `<a>` click event and calls `event.preventDefault()`, preventing the browser from making a network request to the server for `/dashboard`.
2. It uses the browser's **HTML5 History API (`window.history.pushState`)** to update the URL in the address bar.
3. It notifies subscribed `<Routes>` / `<Outlet>` components via Context, unmounting the old route component and mounting `<Dashboard />` instantaneously on the client.

**Key React Router Concepts:**
- **`<Link>` vs `<NavLink>`:** `<NavLink>` automatically knows whether its target route is currently active (`className={({ isActive }) => isActive ? "active" : ""}` and sets `aria-current="page"`).
- **`<Outlet />`:** Renders the matched child route inside a shared parent layout (such as a persistent navbar + sidebar layout).
- **`useParams()`:** Reads dynamic URL path segments (`/users/:userId`).
- **`useSearchParams()`:** Reads and updates URL query strings (`?page=2&sort=asc`)—ideal for shareable filter and pagination state!
- **`useNavigate()`:** Programmatic navigation after form submission or login (`navigate("/dashboard", { replace: true })`).

---

### Q48. How do you implement a Protected Route with Authentication and Role-Based Access Control (RBAC)?

**Answer:**

Wait for initial authentication hydration (`isLoading`), redirect unauthenticated users to `/login` while preserving the `from` location in route `state` (so you can redirect them right back after login), and verify role permissions:

```jsx
import { Navigate, Outlet, useLocation } from "react-router-dom";
import { useAuth } from "./authContext";

export function ProtectedRoute({ allowedRoles }) {
  const { user, isLoading } = useAuth();
  const location = useLocation();

  if (isLoading) {
    return <div className="spinner">Verifying session...</div>;
  }

  // 1. Not logged in -> Redirect to /login, saving intended destination
  if (!user) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  // 2. Logged in, but lacks required role -> Redirect to /unauthorized (403)
  if (allowedRoles && !allowedRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" replace />;
  }

  // 3. Authorized -> Render nested child routes
  return <Outlet />;
}
```

> [!WARNING]
> **Security Rule:** Client-side Protected Routes are for **UX convenience only**. Any user can inspect the JavaScript bundle in Browser DevTools. Every backend API endpoint **must** independently verify authentication tokens and role authorization on the server!

---

## Topics 37, 38 & 39: API Integration, Asynchronous UI States, and Preventing Race Conditions

### Q49. Why can you NOT make the `useEffect` callback itself `async` (`useEffect(async () => ...)`), and how do you write a production-grade data fetching Effect that prevents race conditions?

**Answer:**

**Why `useEffect(async () => ...)` is forbidden:**
An `async` function **always implicitly returns a `Promise`**. However, `useEffect` expects its callback to return either **`undefined` (nothing) or a synchronous cleanup function (`() => void`)**. If `useEffect` receives a `Promise`, React cannot call it as a cleanup function and logs a warning.

**Why naive `useEffect` data fetching suffers from Race Conditions:**
Suppose a user selects `userId = 1` (slow network response: takes 3 seconds), and 200ms later clicks `userId = 2` (fast network response: takes 300ms):
- At `t = 500ms`, `userId = 2` finishes and renders User 2's profile.
- At `t = 3000ms`, the stale request for `userId = 1` finally resolves and calls `setUser(user1)`, **overwriting User 2's profile with User 1's stale data!**

**Complete Race-Condition-Safe Implementation Using `AbortController`:**

```jsx
import { useState, useEffect } from "react";

export function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [status, setStatus] = useState("idle"); // 'idle' | 'loading' | 'success' | 'error'
  const [error, setError] = useState(null);

  useEffect(() => {
    const controller = new AbortController();

    async function fetchUser() {
      setStatus("loading");
      setError(null);

      try {
        const response = await fetch(`/api/users/${userId}`, {
          signal: controller.signal
        });

        if (!response.ok) {
          throw new Error(`HTTP Error: ${response.status}`);
        }

        const data = await response.json();
        setUser(data);
        setStatus("success");
      } catch (err) {
        // Ignore AbortError when previous request was intentionally cancelled
        if (err.name === "AbortError") return;
        setError(err.message);
        setStatus("error");
      }
    }

    fetchUser();

    // Cleanup aborts the in-flight HTTP request if userId changes or component unmounts!
    return () => controller.abort();
  }, [userId]);

  if (status === "loading") return <p>Loading profile...</p>;
  if (status === "error") return <p role="alert">Error: {error}</p>;
  if (!user) return <p>No user found.</p>;

  return (
    <article>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </article>
  );
}
```

---

# Part 10: Phase 6 Deep Dive — State Management Architecture: `useContext`, `useReducer`, Redux Toolkit, TanStack Query & Zustand (Topics 16, 17, 40, 41, 42, 43)

## Topic 16: `useContext` and Avoiding Context Re-render Traps

### Q50. How does React Context work, and why is putting all application state into a single Context Provider a major performance anti-pattern?

**Answer:**

React Context lets a parent component provide data (`<ThemeContext.Provider value={...}>`) to any component in its subtree (`useContext(ThemeContext)`) without passing props manually through intermediate levels.

**The Context Performance Trap:**
Whenever the `value` passed to `<MyContext.Provider value={contextValue}>` changes by reference (`!Object.is(prevValue, nextValue)`), **every single component that calls `useContext(MyContext)` forces a re-render—even if `React.memo` wraps them, and even if the specific property they read from the context object did NOT change!**

**3 Ways to Prevent Unnecessary Context Re-renders:**

1. **Split Contexts by Update Frequency and Responsibility:**
   Never combine `currentUser`, `theme`, and `notifications` into a single `AppContext`. Split them into `AuthContext`, `ThemeContext`, and `NotificationContext`.
2. **Split State and Dispatch into Two Separate Contexts (`StateContext` + `DispatchContext`):**
   Components that only dispatch actions (e.g., an `AddTodoButton` calling `dispatch({ type: "ADD" })`) consume `TodoDispatchContext`. Because `dispatch` from `useReducer` has a permanently stable reference, `AddTodoButton` **never re-renders** when the `todos` list changes!
3. **Memoize the Provider Value Object (`useMemo`):**
   ```jsx
   function AuthProvider({ children }) {
     const [user, setUser] = useState(null);

     const login = useCallback((userData) => setUser(userData), []);
     const logout = useCallback(() => setUser(null), []);

     // Prevents creating a new object reference when parent re-renders
     const value = useMemo(() => ({ user, login, logout }), [user, login, logout]);

     return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
   }
   ```

---

## Topic 17: `useReducer`

### Q51. When should you choose `useReducer` over `useState`?

**Answer:**

Choose `useReducer` over `useState` when:
1. **Multiple state sub-values transition together** (e.g., `{ status, data, error, retryCount }` in an async workflow or checkout wizard) where independent `setLoading`, `setData`, `setError` calls could put the component into an impossible state (such as `isLoading: true` AND `error: "Failed"` simultaneously).
2. **Next state logic is complex** or involves multiple distinct action types (`ADD_ITEM`, `UPDATE_QUANTITY`, `APPLY_COUPON`, `CLEAR_CART`).
3. **Unit testing state logic in isolation:** A reducer `(state, action) => nextState` is a 100% pure JavaScript function that can be tested with zero React rendering setup!

```jsx
const initialState = { items: [], couponCode: null };

function cartReducer(state, action) {
  switch (action.type) {
    case "ADD_ITEM": {
      const existing = state.items.find((i) => i.id === action.payload.id);
      if (existing) {
        return {
          ...state,
          items: state.items.map((i) =>
            i.id === action.payload.id ? { ...i, qty: i.qty + 1 } : i
          )
        };
      }
      return {
        ...state,
        items: [...state.items, { ...action.payload, qty: 1 }]
      };
    }
    case "REMOVE_ITEM":
      return {
        ...state,
        items: state.items.filter((i) => i.id !== action.payload.id)
      };
    case "CLEAR_CART":
      return initialState;
    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
}
```

---

## Topics 40, 41, 42 & 43: Modern State Management Taxonomy (Local vs Client Global vs Server State)

### Q52. How do Senior Frontend Engineers categorize state in a modern React application, and when do you use Context vs Redux Toolkit vs Zustand vs TanStack Query?

**Answer:**

The biggest evolution in modern React architecture is separating **Server State** (asynchronous cached data owned by the backend database) from **Client UI State** (synchronous state owned by the browser session):

| Category | Examples | Best Tool | Why |
|---|---|---|---|
| **1. Local UI State** | Input text, dropdown open/closed, active tab | `useState` / `useReducer` | Simple, colocated with the component |
| **2. URL / Route State** | Search query, active filter, page number, sort order | `useSearchParams` (React Router) | Shareable via URL, survives page refresh and browser Back/Forward |
| **3. Low-Frequency Shared UI State** | Dark/light theme, current locale, auth session | `useContext` | Built-in, zero external dependencies, rarely mutates |
| **4. High-Frequency / Complex Client State** | Multi-component canvas editor, audio player, complex shopping cart, IDE layout | **Zustand** or **Redux Toolkit (RTK)** | Fine-grained selector subscriptions (`useSelector` / `useStore(s => s.count)`) re-render **only** the exact components whose selected slice changed |
| **5. Server State (Remote API Data)** | Product lists, user profiles, paginated orders, analytics data | **TanStack Query (React Query)** or **RTK Query** | Handles caching, request deduplication, background refetching, stale-while-revalidate, optimistic updates, and cache invalidation automatically |

**Why Redux Toolkit (RTK) replaced legacy Redux:**
Legacy Redux required boilerplate action constants, action creators, switch statements, and manual deep spread operators. **Redux Toolkit (`createSlice`, `configureStore`)** uses **Immer** internally so you can write "mutating" syntax (`state.items.push(newItem)`) inside reducers that Immer automatically converts into safe, structural-sharing immutable updates!

**Why TanStack Query eliminates 80% of manual Redux/Effect code:**

```jsx
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

function OrdersList() {
  const queryClient = useQueryClient();

  // Automatically caches, deduplicates concurrent requests, retries, and tracks loading/error!
  const { data: orders, isPending, isError } = useQuery({
    queryKey: ["orders", { status: "OPEN" }],
    queryFn: () => fetch("/api/orders?status=OPEN").then((r) => r.json()),
    staleTime: 60_000 // Fresh for 1 minute
  });

  const cancelMutation = useMutation({
    mutationFn: (orderId) =>
      fetch(`/api/orders/${orderId}/cancel`, { method: "POST" }),
    onSuccess: () => {
      // Automatically invalidates and refetches the orders list!
      queryClient.invalidateQueries({ queryKey: ["orders"] });
    }
  });

  if (isPending) return <p>Loading orders...</p>;
  if (isError) return <p>Failed to load orders.</p>;

  return (
    <ul>
      {orders.map((order) => (
        <li key={order.id}>
          Order #{order.id}
          <button onClick={() => cancelMutation.mutate(order.id)}>Cancel</button>
        </li>
      ))}
    </ul>
  );
}
```

---

# Part 11: Phase 7 Deep Dive — Performance Optimization, Memoization, Fiber Internals, React 18/19 & React Compiler (Topics 18–20, 29–34, 44–46)

## Topics 18, 19 & 20: `useMemo`, `useCallback`, and `React.memo`

### Q53. What is the exact difference between `React.memo`, `useMemo`, and `useCallback`, and how do they work together?

**Answer:**

| API | What It Memoizes | Syntax | Primary Purpose |
|---|---|---|---|
| **`React.memo`** | **A Component** | `const Child = React.memo(function Child(props) { ... })` | Skips re-rendering `Child` when its parent re-renders if all props are shallowly equal (`Object.is(prevProp, nextProp)`) |
| **`useMemo`** | **A Computed Value** (return value of a function) | `const sorted = useMemo(() => heavySort(list), [list])` | 1. Caches expensive CPU calculations across renders.<br>2. Preserves **referential identity** of an object/array passed as a prop to a `React.memo` child or a `useEffect` dependency array. |
| **`useCallback`** | **A Function Definition Reference** | `const handleClick = useCallback((id) => deleteItem(id), [])` | Preserves **referential identity** of a callback function across renders so passing it to a `React.memo` child or `useEffect` does not break memoization. |

*(Note: `useCallback(fn, deps)` is equivalent to `useMemo(() => fn, deps)`.)*

**Why `useCallback` alone is useless if the child component is NOT wrapped in `React.memo`:**

```jsx
// Scenario: Parent re-renders whenever `text` changes
function Parent() {
  const [text, setText] = useState("");
  const [items, setItems] = useState(["A", "B"]);

  // 1. Preserve function reference across Parent renders
  const handleDelete = useCallback((item) => {
    setItems((prev) => prev.filter((i) => i !== item));
  }, []);

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      {/* 2. Child MUST be wrapped in React.memo to skip re-rendering! */}
      <ItemList items={items} onDelete={handleDelete} />
    </div>
  );
}

const ItemList = React.memo(function ItemList({ items, onDelete }) {
  console.log("ItemList rendered");
  return (
    <ul>
      {items.map((item) => (
        <li key={item}>
          {item} <button onClick={() => onDelete(item)}>Remove</button>
        </li>
      ))}
    </ul>
  );
});
```

If `ItemList` is **not** wrapped in `React.memo`, it re-renders every time `Parent` re-renders regardless of whether `handleDelete` was wrapped in `useCallback`! Conversely, if `ItemList` **is** wrapped in `React.memo` but `handleDelete` is a plain function `function handleDelete() {}`, a new function reference is created on every `Parent` render, breaking `React.memo`'s shallow prop comparison!

---

## Topics 29, 30 & 31: Rendering Internals, Virtual DOM, Reconciliation & Fiber Architecture

### Q54. What happens internally during React's three steps of rendering (Trigger -> Render -> Commit), and what is React Fiber?

**Answer:**

Every React UI update goes through **three distinct phases**:

```text
1. TRIGGER PHASE             2. RENDER PHASE (Pure, Interruptible)         3. COMMIT PHASE (Synchronous)
- Initial createRoot.render  - Invokes component functions                 - Applies minimal DOM mutations
- setState / dispatch        - Builds new Virtual DOM Element tree         - Runs useLayoutEffect synchronously
                             - Reconciles (diffs) against current Fiber    - Browser paints screen
                               tree in memory                              - Runs useEffect asynchronously
```

1. **What is the "Virtual DOM"?**
   The Virtual DOM is not a separate browser DOM—it is the tree of lightweight JavaScript **React Elements** (`{ type, props, key }`) returned by your components in memory. Diffing plain JS objects in memory is fast; reading and mutating layout on real browser DOM nodes is slow.
2. **How does React's Reconciliation (Diffing) Algorithm achieve `O(n)` instead of `O(n^3)` complexity?**
   General tree-diffing algorithms take $O(n^3)$ time. React achieves $O(n)$ linear time using **two heuristics**:
   - **Heuristic 1 (Element Type):** Two elements of **different types** (`<div>` -> `<section>`, or `<TabA />` -> `<TabB />`) produce completely different trees. React tears down the old subtree completely (unmounting all state) and builds the new subtree from scratch.
   - **Heuristic 2 (Keys):** Developer-supplied `key` props identify which child elements remain stable across different positions in a list.
3. **What is React Fiber (introduced in React 16)?**
   Before Fiber (the old "Stack Reconciler"), once React started reconciling a deep tree, it executed recursively on the JavaScript call stack in one uninterruptible synchronous block—freezing the browser main thread and dropping frames if rendering took 100ms.
   **React Fiber** re-architected the reconciler around a singly-linked tree of **Fiber nodes** (where each Fiber has `child`, `sibling`, and `return` pointers) using **double buffering** (`current` tree currently on screen vs `workInProgress` tree being built in memory). Because each Fiber is a unit of work on a heap-allocated linked list rather than the JS call stack, React can **pause, yield to the browser for urgent user input, abort, or prioritize** rendering work in Concurrent React!

---

## Topic 32: React 18 Concurrent Features (`Automatic Batching`, `useTransition`, `useDeferredValue`, `Suspense`)

### Q55. What major improvements did React 18 introduce?

**Answer:**

1. **Automatic Batching Everywhere:**
   - In **React 17 and earlier**, React only batched multiple `setState` calls inside synchronous React event handlers. State updates inside `setTimeout`, `Promise.then` (`fetch`), or native DOM event listeners triggered **separate re-renders** for every `setState`.
   - In **React 18+ (`createRoot`)**, React automatically batches multiple `setState` calls into a **single re-render** regardless of where they originate (`fetch`, `setTimeout`, promises, native events). *(In the rare case where you need to read the DOM immediately after a state update, `flushSync` from `react-dom` opts out of batching.)*
2. **Transitions (`useTransition` & `startTransition`):**
   Distinguishes **urgent updates** (typing into a search input—must respond in `< 16ms`) from **non-urgent transition updates** (filtering and rendering a 10,000-row chart based on that input):
   ```jsx
   import { useState, useTransition } from "react";

   function SearchPage({ allItems }) {
     const [query, setQuery] = useState("");
     const [filtered, setFiltered] = useState(allItems);
     const [isPending, startTransition] = useTransition();

     function handleChange(e) {
       const nextQuery = e.target.value;
       // 1. Urgent update: keeps input typing responsive immediately
       setQuery(nextQuery);

       // 2. Non-urgent transition: interruptible if user types another key!
       startTransition(() => {
         setFiltered(allItems.filter((item) => item.includes(nextQuery)));
       });
     }

     return (
       <div>
         <input value={query} onChange={handleChange} />
         {isPending && <span>Updating list...</span>}
         <HeavyResultsList items={filtered} />
       </div>
     );
   }
   ```
3. **`useDeferredValue(value)`:**
   Similar to `useTransition`, when you receive a rapidly changing value from props (where you don't control the `setState` call), `const deferredQuery = useDeferredValue(query)` lets React render with the old value first to keep the UI responsive, then render the updated value in the background as an interruptible transition.

---

## Topics 33 & 34: Modern React 19 Features (`Actions`, `useActionState`, `useOptimistic`, `useFormStatus`, `use`) & React Compiler

### Q56. What are the key new features and Hooks introduced in React 19 and the React Compiler?

**Answer:**

1. **Actions & `<form action={fn}>`:**
   Functions that use async transitions to handle data mutations, automatically managing pending state, error handling, optimistic updates, and form resets.
2. **`useActionState(actionFn, initialState)`:**
   Takes an async form action function and returns `[state, formAction, isPending]`, eliminating manual `isLoading` / `error` boilerplate on forms:
   ```jsx
   import { useActionState } from "react";

   async function updateProfileAction(prevState, formData) {
     const name = formData.get("name");
     const res = await saveToApi(name);
     if (res.error) return { error: res.error };
     return { success: true };
   }

   function ProfileForm() {
     const [state, formAction, isPending] = useActionState(updateProfileAction, {});

     return (
       <form action={formAction}>
         <input name="name" disabled={isPending} />
         <button type="submit" disabled={isPending}>
           {isPending ? "Saving..." : "Save"}
         </button>
         {state.error && <p className="error">{state.error}</p>}
       </form>
     );
   }
   ```
3. **`useFormStatus()` (from `react-dom`):**
   Allows a deep child component inside a `<form>` (like a reusable `<SubmitButton />`) to read `{ pending, data }` of its parent `<form>` without prop drilling.
4. **`useOptimistic(state, updateFn)`:**
   Displays an immediate optimistic UI state while an async mutation is in flight, automatically reverting or settling to the canonical state once the async action completes.
5. **The `use(resource)` API:**
   Unlike Hooks, `use(promiseOrContext)` **can be called conditionally inside `if` statements and loops!** When passed a Promise, `use(promise)` integrates directly with `<Suspense>` (suspending while pending) and `<ErrorBoundary>` (throwing on rejection).
6. **React Compiler (React 19):**
   A build-time compiler (`babel-plugin-react-compiler`) that automatically memoizes values, functions, and JSX elements inside your components—reducing the need to manually litter code with `useMemo`, `useCallback`, and `React.memo`!

---

## Topics 44, 45 & 46: Code Splitting (`React.lazy` & `Suspense`) and List Virtualization at Scale

### Q57. How do you optimize a React app that has a huge 4 MB JavaScript bundle or freezes when rendering 50,000 rows?

**Answer:**

**Problem 1: Huge Initial JavaScript Bundle -> Fix with Route-Level Code Splitting (`React.lazy` + `Suspense`)**

Instead of bundling the Admin Panel, PDF Exporter, and Analytics Charting libraries into the initial login page bundle, split them into separate async chunks loaded on demand:

```jsx
import { Suspense, lazy } from "react";
import { BrowserRouter, Routes, Route } from "react-router-dom";

// Dynamically imported only when user navigates to /analytics!
const AnalyticsDashboard = lazy(() => import("./pages/AnalyticsDashboard"));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<div className="page-skeleton">Loading module...</div>}>
        <Routes>
          <Route path="/analytics" element={<AnalyticsDashboard />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

**Problem 2: Rendering 50,000 DOM Rows Freezes the Browser -> Fix with List Virtualization (Windowing)**

Rendering 50,000 `<tr>` or `<div>` nodes creates 500,000+ DOM elements, consuming gigabytes of browser memory and causing multi-second layout recalculations.
**Virtualization (Windowing)** using `@tanstack/react-virtual` or `react-window` calculates which ~20 rows are currently visible inside the scroll viewport (`scrollTop` / `clientHeight`) and **only mounts those 20 DOM nodes**, positioning them inside a tall spacer container!

---

# Part 12: Phase 8 Deep Dive — TypeScript With React, Styling, UI Component Systems & Accessibility (Topics 47, 48, 49, 50)

## Topic 47: TypeScript With React

### Q58. How do you properly type Props, Children, Events, Refs, and Discriminated Unions in React + TypeScript?

**Answer:**

```tsx
import { useState, useRef, type ReactNode, type ChangeEvent, type FormEvent } from "react";

// 1. Typing Props + Children + Native Button Attributes
interface ButtonProps extends React.ComponentPropsWithoutRef<"button"> {
  variant?: "primary" | "secondary" | "danger";
  children: ReactNode;
}

export function Button({ variant = "primary", children, ...rest }: ButtonProps) {
  return (
    <button className={`btn btn-${variant}`} {...rest}>
      {children}
    </button>
  );
}

// 2. Typing Discriminated Union State (Eliminates impossible states!)
type AsyncState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: string };

// 3. Typing Hooks, Refs, and Events
export function SearchForm() {
  const [query, setQuery] = useState<string>("");
  const inputRef = useRef<HTMLInputElement>(null);

  function handleChange(e: ChangeEvent<HTMLInputElement>) {
    setQuery(e.target.value);
  }

  function handleSubmit(e: FormEvent<HTMLFormElement>) {
    e.preventDefault();
    inputRef.current?.select();
  }

  return (
    <form onSubmit={handleSubmit}>
      <input ref={inputRef} value={query} onChange={handleChange} />
    </form>
  );
}
```

---

## Topics 48, 49 & 50: Styling Architectures, Reusable UI Components, and Web Accessibility (a11y)

### Q59. Compare CSS Modules, Tailwind CSS, and Runtime CSS-in-JS. Why did the React ecosystem move away from Runtime CSS-in-JS for Server Components?

**Answer:**

| Styling Approach | How It Works | Pros | Cons |
|---|---|---|---|
| **CSS Modules (`Button.module.css`)** | Build tool hashes class names (`_btn_x8f2a_1`) for automatic local scoping | Zero runtime JS overhead, standard CSS syntax, works with RSC | Verbose class name composition for dynamic variants |
| **Tailwind CSS (Utility-First)** | Atomic utility classes scanned at build time; unused classes purged | Tiny production CSS bundle, fast colocated styling, zero runtime JS | Long class strings in JSX (mitigated with `clsx` / `tailwind-merge` / `cva`) |
| **Runtime CSS-in-JS (`styled-components`, Emotion)** | Serializes JS styles and injects `<style>` tags dynamically at runtime | Dynamic JS prop interpolation | Adds runtime bundle size, slows down rendering serialization, and is **incompatible with React Server Components (RSC)** |

---

### Q60. What are the mandatory Accessibility (a11y) requirements when building a custom Modal Dialog or Dropdown in React?

**Answer:**

Interviewers love asking how to build an accessible `<Modal>` or `<Dropdown>`:
1. **Semantic HTML First:** Never use `<div onClick={...}>` for a button! A `<div>` is not focusable via `Tab` and does not fire on `Enter` or `Space` keys. Always use native `<button>` or `<dialog>`.
2. **ARIA Roles & Attributes:** Set `role="dialog"`, `aria-modal="true"`, and `aria-labelledby="modal-title-id"` on custom modals.
3. **Focus Management & Focus Trapping:**
   - When the modal opens, move keyboard focus into the modal's first interactive element (saving `document.activeElement` first).
   - **Trap `Tab` and `Shift+Tab`** inside the modal so keyboard users cannot tab into background page elements behind the backdrop.
   - Support closing the modal via the **`Escape` key**.
   - When the modal closes, **restore focus back to the trigger button** that opened it!

---

# Part 13: Phase 9 Deep Dive — Testing, Security, Authentication, File Operations, i18n, Debugging & Common Errors (Topics 51–56, 71–72)

## Topics 51 & 52: Unit/Integration Testing (React Testing Library + Vitest) & E2E Testing (Playwright)

### Q61. What is the core philosophy of React Testing Library (RTL), and why should you prefer `getByRole` over `getByTestId`?

**Answer:**

*"The more your tests resemble the way your software is used, the more confidence they can give you."* — Testing Library guiding principle.

- **Avoid testing implementation details:** Do not assert on internal `useState` variable names or component class methods (which break whenever you refactor). Instead, render the component, interact with it as a real user (`userEvent.click`, `userEvent.type`), and assert on the visible DOM output.
- **Query Priority Order:**
  1. **`getByRole` (Highest Priority):** Queries elements through the browser's Accessibility Tree (`screen.getByRole("button", { name: /submit/i })`). If your test can find the button via `getByRole`, screen readers can also use your UI!
  2. **`getByLabelText` / `getByPlaceholderText` / `getByText`**
  3. **`getByTestId` (Last Resort):** Only when semantic role or text queries are impossible.
- **`getBy` vs `queryBy` vs `findBy`:**
  - `getBy...`: Synchronous; throws an error immediately if not found (use for asserting presence).
  - `queryBy...`: Synchronous; returns `null` if not found (use **only** for asserting that an element is *not* in the document: `expect(screen.queryByText(/error/i)).not.toBeInTheDocument()`).
  - `findBy...`: Asynchronous (`Promise`); retries until the element appears or times out (use after async `fetch` or state transitions).

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { describe, it, expect } from "vitest";
import Counter from "./Counter";

describe("Counter", () => {
  it("increments count when button is clicked", async () => {
    const user = userEvent.setup();
    render(<Counter />);

    const button = screen.getByRole("button", { name: /count: 0/i });
    await user.click(button);

    expect(screen.getByRole("button", { name: /count: 1/i })).toBeInTheDocument();
  });
});
```

---

## Topics 53 & 54: Frontend Security (XSS, CSRF) & Authentication Architecture (JWT, `HttpOnly` Cookies, Refresh Tokens)

### Q62. How does React protect against Cross-Site Scripting (XSS) by default, and what are the security holes where XSS can still happen in React?

**Answer:**

1. **How React protects you by default:**
   Any string interpolated inside JSX curly braces (`<div>{userInput}</div>`) is **automatically HTML-escaped** by React before being inserted into the DOM (`<script>alert(1)</script>` is rendered harmlessly as literal text `&lt;script&gt;...`).
2. **3 Ways XSS Can Still Occur in a React App:**
   - **Using `dangerouslySetInnerHTML={{ __html: untrustedHtml }}` without sanitization:** Always sanitize untrusted HTML with **DOMPurify** (`DOMPurify.sanitize(dirtyHtml)`) before passing it to `dangerouslySetInnerHTML`.
   - **Unvalidated URL schemes in `<a href={userUrl}>` or `<iframe src={userUrl}>`:** If `userUrl` is `"javascript:alert(document.cookie)"`, clicking the `<a href={userUrl}>` executes arbitrary JavaScript! Always validate that user-supplied URLs start with `https://` or `http://` via `new URL(input)`.
   - **Storing sensitive tokens in `localStorage`:** Any XSS vulnerability in a third-party npm package can read `localStorage.getItem("token")` in a single line of JS.

---

### Q63. Where should JWT Access Tokens and Refresh Tokens be stored in a production React application (`localStorage` vs `HttpOnly` Cookies)?

**Answer:**

| Storage Mechanism | Vulnerable to XSS? | Vulnerable to CSRF? | Production Recommendation |
|---|---|---|---|
| **`localStorage` / `sessionStorage`** | **Yes!** Any JS on the page can steal the token | No | Avoid for high-security financial/enterprise apps |
| **`HttpOnly; Secure; SameSite=Lax/Strict` Cookie** | **No!** Cannot be read via `document.cookie` | Mitigated via `SameSite=Strict/Lax` + CSRF Token | **Best for Refresh Tokens & Session Cookies** |
| **In-Memory JS Variable (React State / Closure)** | Lost on page refresh; inaccessible to persistent storage scraping | No | **Best for short-lived Access Tokens (5–15 min)** paired with an `HttpOnly` Refresh Token cookie |

**The Gold-Standard SPA Auth Flow:**
1. On login, the backend sets a long-lived **Refresh Token** in an `HttpOnly; Secure; SameSite=Strict` cookie and returns a short-lived (10-minute) **Access Token** in the JSON response body.
2. React stores the Access Token **in memory only** (never in `localStorage`).
3. An Axios/Fetch interceptor attaches `Authorization: Bearer <accessToken>` to API requests.
4. When an API returns `401 Unauthorized` (or on page reload), the interceptor calls `/api/auth/refresh` (which automatically sends the `HttpOnly` cookie), obtains a fresh Access Token in memory, and transparently retries the failed request!

---

## Topics 55 & 56: File Operations (Uploads, Previews, Memory Cleanup) & Internationalization (i18n)

### Q64. How do you implement a file upload with image preview in React without leaking browser memory?

**Answer:**

When generating a local preview URL via `URL.createObjectURL(file)`, the browser allocates an in-memory `blob:` reference tied to the document. You **must call `URL.revokeObjectURL(previewUrl)`** in a `useEffect` cleanup function to prevent memory leaks:

```jsx
import { useState, useEffect } from "react";

export function AvatarUploader() {
  const [file, setFile] = useState(null);
  const [previewUrl, setPreviewUrl] = useState(null);

  useEffect(() => {
    if (!file) {
      setPreviewUrl(null);
      return;
    }

    const objectUrl = URL.createObjectURL(file);
    setPreviewUrl(objectUrl);

    // Crucial: Release memory when file changes or component unmounts!
    return () => URL.revokeObjectURL(objectUrl);
  }, [file]);

  async function handleUpload(e) {
    e.preventDefault();
    if (!file) return;

    const formData = new FormData();
    formData.append("avatar", file);

    // Do NOT manually set Content-Type header when sending FormData;
    // the browser automatically sets multipart/form-data with the boundary string!
    await fetch("/api/upload", { method: "POST", body: formData });
  }

  return (
    <form onSubmit={handleUpload}>
      <input
        type="file"
        accept="image/png, image/jpeg"
        onChange={(e) => setFile(e.target.files?.[0] ?? null)}
      />
      {previewUrl && <img src={previewUrl} alt="Preview" width={120} />}
      <button type="submit" disabled={!file}>Upload</button>
    </form>
  );
}
```

---

## Topics 71 & 72: Debugging with React DevTools & Resolving the Top 5 Common React Errors

### Q65. How do you diagnose why a component re-rendered using React DevTools, and what causes the 5 most common React errors?

**Answer:**

**Using React DevTools Profiler:**
1. Open Browser DevTools -> **Profiler** tab -> Click the Gear icon -> Enable **"Record why each component rendered while profiling."**
2. Click Record, perform the slow UI interaction, and stop recording.
3. Inspect the **Flamegraph** and **Ranked** charts: hovering over any yellow/green bar tells you the exact reason it rendered (*"Props changed: `onSelect`"* or *"Parent component rendered"* or *"Hooks changed: Hook 2"*).

**Top 5 Common React Runtime Errors & Fixes:**

| Error Message | Root Cause | Fix |
|---|---|---|
| `Invalid hook call. Hooks can only be called inside of the body of a function component` | 1. Calling a Hook inside an event handler, loop, or plain utility function.<br>2. Duplicate copies of `react` in `node_modules`. | Move Hook to top level of component; run `npm ls react` to check version duplicates. |
| `Too many re-renders. React limits the number of renders to prevent an infinite loop` | Calling `setState(...)` unconditionally during render (e.g., `onClick={setCount(1)}` instead of `onClick={() => setCount(1)}`). | Wrap handler in an arrow function or move state update inside an event handler. |
| `Rendered more/fewer hooks than during the previous render` | Calling a Hook after an early `if (loading) return <Spinner />;` or inside a conditional block. | Place **all** Hook calls above any conditional early `return`. |
| `Hydration failed because the initial UI does not match what was rendered on the server` | Rendering `new Date().toLocaleTimeString()`, `Math.random()`, `window.innerWidth`, or invalid HTML (`<div>` inside `<p>`) during SSR. | Defer browser-only/random values to `useEffect` after hydration, or fix invalid HTML nesting. |
| `Warning: Each child in a list should have a unique "key" prop` | Missing `key` on the outermost element returned inside `.map()`. | Put `key={item.id}` on the outermost element (or `<Fragment key={item.id}>`) inside `.map()`. |

---

# Part 14: Phase 10 Deep Dive — Modern Rendering Architectures: CSR, SSR, SSG, ISR, React Server Components (RSC) & Streaming (Topics 59–63)

## Topics 59, 60, 61, 62 & 63: Comparing Rendering Paradigms & React Server Components

### Q66. Compare CSR, SSR, SSG, ISR, and React Server Components (RSC). When should you choose each?

**Answer:**

| Rendering Strategy | How It Works | Pros | Cons | Best Use Cases |
|---|---|---|---|---|
| **CSR (Client-Side Rendering)** | Server sends an empty `<div id="root">` + JS bundle; browser downloads JS, runs React, and fetches data | Cheap static hosting, fast client-side transitions after initial load | Slow First Contentful Paint (FCP) on slow devices; poor SEO for non-JS crawlers | Authenticated B2B SaaS dashboards, internal admin portals, Web IDEs |
| **SSR (Server-Side Rendering)** | Server fetches data and renders full HTML on every request, then sends JS to **hydrate** the DOM | Fast initial HTML paint (FCP/LCP), dynamic per-request SEO | Higher server compute cost; Time to First Byte (TTFB) waits on server data (unless streaming) | Personalized e-commerce product pages, dynamic news feeds |
| **SSG (Static Site Generation)** | HTML is pre-rendered once at **build time** (`npm run build`) and served from a CDN | Instant CDN response, zero runtime server load | Build times grow with 100,000+ pages; content is stale until rebuild | Documentation sites, marketing landing pages, blogs |
| **ISR (Incremental Static Regeneration)** | Serves static CDN pages and regenerates individual pages in the background after a TTL or webhook | Combines CDN speed of SSG with fresh data of SSR | Eventual consistency (first visitor after expiry sees stale page while background rebuilds) | Large e-commerce catalogs, high-traffic content sites |
| **RSC (React Server Components)** | Components execute **exclusively on the server** (build time or request time) and **zero component JS is ever sent to the client bundle** | Direct backend DB/filesystem access, zero bundle weight for heavy libraries (markdown parsers, syntax highlighters) | Cannot use `useState`, `useEffect`, or browser DOM APIs inside Server Components | Modern Next.js App Router applications combining server data fetching with interactive Client islands |

---

### Q67. What is the exact difference between a React Server Component (RSC) and a Client Component (`"use client"`), and how can you compose them together?

**Answer:**

| Feature | Server Component (Default in Next.js App Router) | Client Component (`"use client"` at top of file) |
|---|---|---|
| **Can be `async function Page()` and `await db.query()` directly?** | **Yes!** | No (unless reading a Promise passed from server via `use(promise)`) |
| **Included in Client JS Bundle?** | **No (0 KB JS sent to browser!)** | Yes (bundled, downloaded, and hydrated in browser) |
| **Can use `useState`, `useReducer`, `useEffect`?** | **No** | **Yes** |
| **Can use `onClick`, `onChange`, `window`, `localStorage`?** | **No** | **Yes** |
| **Pre-rendered to HTML during SSR?** | Yes | **Yes!** Client Components are *also* pre-rendered to initial HTML on the server during SSR, then hydrated in the browser! |

**Important Composition Rule (Tested Frequently in Senior Interviews):**
*Can a Client Component directly `import` and render a Server Component?*
- **No** via direct `import` (importing a module into a `"use client"` file pulls it into the client bundle boundary).
- **YES via `children` composition (Slot Pattern)!** A Parent Server Component can render `<ClientModalWrapper><ServerUserProfile /></ClientModalWrapper>`, passing the already-rendered Server Component output as `children` into the Client Component!

---

# Part 15: Phase 11 Deep Dive — Web Performance (Core Web Vitals), Vite Build Tooling, Docker, Nginx & CI/CD Deployment (Topics 64–70)

## Topics 64, 65 & 66: Core Web Vitals, Vite Bundling & Environment Variables

### Q68. What are Google's three Core Web Vitals (LCP, INP, CLS), and how do you optimize each in a React application?

**Answer:**

1. **LCP (Largest Contentful Paint — Target: `< 2.5s`):** Measures loading performance (when the largest hero image or heading is painted).
   - *React Optimizations:* Avoid client-side fetch waterfalls (`App` mounts -> fetches user -> renders `Dashboard` -> fetches hero banner); use SSR/SSG or preload critical data and hero images (`<link rel="preload" fetchpriority="high">`), and code-split non-critical routes.
2. **INP (Interaction to Next Paint — Target: `< 200ms`, replaced FID in 2024):** Measures main-thread responsiveness across all clicks, taps, and keystrokes.
   - *React Optimizations:* Break up long synchronous renders using `startTransition` / `useDeferredValue`, virtualize long lists, debounce expensive search handlers, and avoid unnecessary full-tree re-renders.
3. **CLS (Cumulative Layout Shift — Target: `< 0.1`):** Measures visual stability (preventing buttons or text from jumping around as images/ads load).
   - *React Optimizations:* Always specify explicit `width` and `height` (or CSS `aspect-ratio`) on `<img>` tags, reserve fixed-height skeleton placeholders inside `<Suspense fallback={<Skeleton height={240} />}>`, and use `font-display: optional` or size-adjusted fallback fonts.

**Environment Variable Security Rule in Vite (`import.meta.env`):**
Vite only exposes environment variables prefixed with **`VITE_`** (e.g., `VITE_API_BASE_URL`) to client-side React code via `import.meta.env.VITE_API_BASE_URL`.
**Never** put private backend secrets (database passwords, OpenAI API keys, Stripe secret keys) in `VITE_*` variables—anything bundled into the frontend JavaScript is publicly visible in every user's Browser DevTools!

---

## Topics 67, 68, 69 & 70: Deploying React SPAs with Multi-Stage Docker, Nginx & CI/CD

### Q69. Why does refreshing a nested route (e.g., `https://example.com/orders/123`) in a deployed React SPA return `404 Not Found` from Nginx/S3, and how do you fix it?

**Answer:**

When you navigate to `/orders/123` inside the browser via `<Link>`, React Router updates the URL in memory without asking the server.
However, when the user **hits F5 (Refresh)** or opens `https://example.com/orders/123` directly in a new tab, the browser sends an HTTP `GET /orders/123` request to the static web server (Nginx, CloudFront, Vercel). Because the `dist/` build folder only contains `/index.html` and `/assets/...` (there is no physical `/orders/123/index.html` file on disk), the server returns `404 Not Found`!

**The Fix: Configure SPA Fallback Routing (`try_files` in Nginx)**
Configure the web server to fall back to `/index.html` whenever a requested path does not match a static file, allowing React Router to boot up and render the `/orders/123` route:

**Production Multi-Stage `Dockerfile` + `nginx.conf` for a React SPA:**

```dockerfile
# Stage 1: Build the React production bundle
FROM node:22-alpine AS builder
WORKDIR /app

# Install dependencies deterministically using lockfile
COPY package.json package-lock.json ./
RUN npm ci

# Copy source and build static assets into /app/dist
COPY . .
RUN npm run build

# Stage 2: Serve static assets with lightweight Nginx (Final image ~25MB!)
FROM nginx:1.27-alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

```nginx
# nginx.conf
server {
    listen 80;
    root /usr/share/nginx/html;
    index index.html;

    # 1. SPA Routing Fallback: serve index.html for all client routes
    location / {
        try_files $uri $uri/ /index.html;
        # Never cache index.html so new deployments take effect immediately!
        add_header Cache-Control "no-store, no-cache, must-revalidate";
    }

    # 2. Long-term immutable caching for content-hashed Vite assets (/assets/index-a8f3b2.js)
    location /assets/ {
        expires 1y;
        add_header Cache-Control "public, max-age=31536000, immutable";
    }
}
```

---

# Part 16: Phase 12 Deep Dive — Architecture, Design Patterns, Frontend System Design, Real-World Scenarios & JS/Browser/Network Internals (Topics 57–58, 73–74, 76–80)

## Topics 57 & 58: Feature-Based React Architecture & Advanced Component Design Patterns

### Q70. Why do large enterprise React codebases prefer Feature-Based (Domain-Driven) folder architecture over Type-Based folders?

**Answer:**

- **Type-Based Folder Structure (`/components`, `/hooks`, `/services`, `/types`):** Works fine for 10 components, but at 300+ components, modifying the "Checkout" feature requires jumping across 6 distant top-level directories, and unrelated features become tightly coupled.
- **Feature-Based Folder Structure (`src/features/checkout`, `src/features/exams`, `src/features/auth`):** Colocates components, hooks, API queries, tests, and types that change together inside a cohesive domain module, while keeping truly generic UI primitives (`Button`, `Modal`, `Input`) inside `src/components/ui/`:

```text
src/
├── app/                  # Providers, global router, store setup
├── components/ui/        # Pure, domain-agnostic design system components (Button, Modal)
├── features/             # Domain modules (Self-contained!)
│   ├── auth/
│   │   ├── api/
│   │   ├── components/
│   │   ├── hooks/
│   │   └── index.ts      # Public barrel API for the feature
│   ├── exams/
│   └── payments/
├── hooks/                # Global utility hooks (useDebounce, useMediaQuery)
└── lib/                  # Configured HTTP client, date/currency formatters
```

---

### Q71. Explain the Compound Component Pattern in React and why UI libraries (Radix UI, Reach UI) use it.

**Answer:**

Instead of passing 20 configuration props to a monolithic component (`<Tabs titles={["A", "B"]} contents={[<PanelA/>, <PanelB/>]} headerClass="..." />`), the **Compound Component Pattern** uses an internal React Context shared between a parent component and its semantic sub-components (`<Tabs>`, `<Tabs.List>`, `<Tabs.Trigger>`, `<Tabs.Content>`), just like native HTML `<select>` and `<option>`:

```jsx
import { createContext, useContext, useState } from "react";

const TabsContext = createContext(null);

export function Tabs({ defaultValue, children }) {
  const [activeTab, setActiveTab] = useState(defaultValue);
  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs">{children}</div>
    </TabsContext.Provider>
  );
}

Tabs.Trigger = function TabTrigger({ value, children }) {
  const { activeTab, setActiveTab } = useContext(TabsContext);
  const isSelected = activeTab === value;
  return (
    <button
      role="tab"
      aria-selected={isSelected}
      className={isSelected ? "tab-active" : "tab"}
      onClick={() => setActiveTab(value)}
    >
      {children}
    </button>
  );
};

Tabs.Content = function TabContent({ value, children }) {
  const { activeTab } = useContext(TabsContext);
  if (activeTab !== value) return null;
  return <div role="tabpanel">{children}</div>;
};
```

---

## Topics 73 & 74: Frontend System Design & Real-World Production Scenarios

### Q72. Scenario: Design a Real-Time Online Examination Portal in React (Timer Synchronization, Auto-Save, Network Resilience & Tab-Switch Detection)

**Answer:**

Interviewers often ask how you would architect a mission-critical React application like an **Online Exam Portal**:

1. **Tamper-Proof Countdown Timer:**
   - **Never rely solely on `setInterval(() => setSeconds(s => s - 1), 1000)`!** Browser background tabs throttle `setInterval` to once per minute or pause it when the laptop sleeps.
   - Instead, receive the authoritative `examEndTimeEpochMs` from the server, compute `remainingMs = examEndTimeEpochMs - (Date.now() + serverClockOffsetMs)` on every tick and on `visibilitychange`, and auto-submit when `remainingMs <= 0`.
2. **Offline-Resilient Answer Auto-Save:**
   - Persist every selected answer immediately to **`IndexedDB` / `localStorage`** so a browser crash or accidental refresh loses zero progress.
   - Batch and debounce network syncs (`PATCH /api/exams/:id/answers`) every 5 seconds or on question navigation using an idempotency sequence number, queuing failed requests when `navigator.onLine === false` and flushing on the `window` `"online"` event.
3. **State Normalization for 100 Questions:**
   - Store answers in a normalized map `{ [questionId]: selectedOptionId }` rather than an array of nested question objects so selecting an answer updates in `O(1)` without re-rendering the entire question palette.

---

### Q73. Scenario: You type into a search input, and an old `setInterval` or event listener logs `query` as `""` (empty string) even though the screen shows `"react"`. What is this bug, and how do you fix it?

**Answer:**

This is the classic **Stale Closure** bug:

```jsx
// BUG: Stale Closure
function ChatLogger() {
  const [message, setMessage] = useState("");

  useEffect(() => {
    const id = setInterval(() => {
      // Because dependencies are [], this callback closed over the FIRST render
      // where `message` was `""` forever!
      console.log("Current draft:", message);
    }, 3000);

    return () => clearInterval(id);
  }, []); // <-- Missing `message` dependency!
}
```

**How to Fix Stale Closures:**
1. **Include the dependency in the dependency array (`[message]`)** if restarting the effect is acceptable.
2. **Use a functional state updater (`setCount(c => c + 1)`)** if you are updating state based on previous state.
3. **Use the Latest Ref Pattern (`useRef` / `useEffectEvent`)** when you want an interval or listener to stay mounted without restarting, while always reading the latest value:
   ```jsx
   const latestMessageRef = useRef(message);
   useEffect(() => {
     latestMessageRef.current = message;
   });

   useEffect(() => {
     const id = setInterval(() => {
       console.log("Current draft:", latestMessageRef.current);
     }, 3000);
     return () => clearInterval(id);
   }, []);
   ```

---

## Topics 76, 77, 78, 79 & 80: JavaScript Event Loop, Browser Rendering Pipeline, CORS & React Version Evolution

### Q74. How do the JavaScript Event Loop (Microtasks vs Macrotasks), Browser Rendering Pipeline, and CORS Preflight Requests impact React applications?

**Answer:**

1. **JavaScript Event Loop (Call Stack -> Microtask Queue -> Render/Paint -> Macrotask Queue):**
   - **Microtasks (`Promise.then`, `queueMicrotask`, `MutationObserver`):** Execute **immediately after the current synchronous call stack empties**, *before* the browser yields to repaint the screen!
   - **Macrotasks (`setTimeout`, `setInterval`, `MessageChannel`):** Execute on subsequent ticks after microtasks and browser rendering opportunities.
2. **Browser Rendering Pipeline (Critical Rendering Path):**
   `DOM + CSSOM -> Render Tree -> Layout (Reflow) -> Paint -> Composite`.
   - Animating CSS `width`, `height`, `top`, or `left` triggers expensive **Layout (Reflow) + Paint** on the main thread.
   - Animating CSS **`transform` (`translate3d`, `scale`) and `opacity`** skips Layout and Paint entirely, executing on the **GPU Compositor thread** at 60–120 FPS!
3. **CORS (Cross-Origin Resource Sharing) & Preflight `OPTIONS`:**
   - CORS is enforced by **browsers**, not Postman or backend servers talking to each other.
   - When your React app at `http://localhost:5173` sends a `POST` request with `Content-Type: application/json` or an `Authorization: Bearer ...` header to `https://api.example.com`, the browser first sends an automatic **Preflight `OPTIONS` request** to verify `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, and `Access-Control-Allow-Headers`.

---

### Q75. Summarize the evolution of React across versions (React 15 -> 16 -> 16.8 -> 17 -> 18 -> 19).

**Answer:**

| Version | Landmark Features Introduced |
|---|---|
| **React 15 & earlier** | Synchronous Stack Reconciler, `React.createClass`, class components |
| **React 16.0–16.7** | **Fiber Reconciler rewrite**, Error Boundaries (`componentDidCatch`), Portals, Fragments, `React.lazy` + `Suspense` for code splitting |
| **React 16.8** | **React Hooks** (`useState`, `useEffect`, `useContext`, `useReducer`, `useRef`, `useMemo`, `useCallback`), enabling stateful function components |
| **React 17** | **New Automatic JSX Transform** (no `import React` needed), Event Delegation moved from `document` to the root DOM container (`#root`) |
| **React 18** | **Concurrent React** (`createRoot`), Automatic Batching everywhere, `useTransition`, `useDeferredValue`, `useId`, `useSyncExternalStore`, Streaming SSR `<Suspense>` |
| **React 19** | **Actions & Form Hooks** (`useActionState`, `useFormStatus`, `useOptimistic`), `use()` API for Promises & Context, `ref` as a regular prop (no `forwardRef` needed), `<Context>` as provider, native Document Metadata (`<title>`, `<meta>`), React Server Components stabilization, and the **React Compiler** |

---

# Part 17: Phase 13 Deep Dive — Practical Live Coding Interview Challenges (Topic 75)

In almost every React interview, you will be asked to code one of these interactive components live in 20–30 minutes. Study these clean, production-ready implementations carefully.

## Coding Challenge 1: Searchable, Sortable & Paginated Data Table with Debounce

```jsx
import { useState, useMemo, useEffect } from "react";

function useDebounce(value, delay = 300) {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(id);
  }, [value, delay]);
  return debounced;
}

export function EmployeeDirectory({ employees, pageSize = 5 }) {
  const [search, setSearch] = useState("");
  const [sortBy, setSortBy] = useState("name"); // 'name' | 'salary'
  const [sortDir, setSortDir] = useState("asc"); // 'asc' | 'desc'
  const [page, setPage] = useState(1);

  const debouncedSearch = useDebounce(search, 250);

  // Filter and sort with useMemo
  const processedEmployees = useMemo(() => {
    const normalizedQuery = debouncedSearch.trim().toLowerCase();

    const filtered = employees.filter(
      (emp) =>
        emp.name.toLowerCase().includes(normalizedQuery) ||
        emp.department.toLowerCase().includes(normalizedQuery)
    );

    return filtered.toSorted((a, b) => {
      const valA = a[sortBy];
      const valB = b[sortBy];
      const comparison =
        typeof valA === "string"
          ? valA.localeCompare(valB)
          : valA - valB;
      return sortDir === "asc" ? comparison : -comparison;
    });
  }, [employees, debouncedSearch, sortBy, sortDir]);

  const totalPages = Math.max(1, Math.ceil(processedEmployees.length / pageSize));
  const safePage = Math.min(page, totalPages);

  const paginatedRows = useMemo(() => {
    const start = (safePage - 1) * pageSize;
    return processedEmployees.slice(start, start + pageSize);
  }, [processedEmployees, safePage, pageSize]);

  function handleSort(column) {
    if (sortBy === column) {
      setSortDir((d) => (d === "asc" ? "desc" : "asc"));
    } else {
      setSortBy(column);
      setSortDir("asc");
    }
  }

  return (
    <section>
      <input
        type="search"
        placeholder="Search by name or department..."
        value={search}
        onChange={(e) => {
          setSearch(e.target.value);
          setPage(1); // Reset to page 1 on new search
        }}
      />

      <table>
        <thead>
          <tr>
            <th onClick={() => handleSort("name")}>
              Name {sortBy === "name" ? (sortDir === "asc" ? "▲" : "▼") : ""}
            </th>
            <th>Department</th>
            <th onClick={() => handleSort("salary")}>
              Salary {sortBy === "salary" ? (sortDir === "asc" ? "▲" : "▼") : ""}
            </th>
          </tr>
        </thead>
        <tbody>
          {paginatedRows.length === 0 ? (
            <tr>
              <td colSpan={3}>No matching employees found.</td>
            </tr>
          ) : (
            paginatedRows.map((emp) => (
              <tr key={emp.id}>
                <td>{emp.name}</td>
                <td>{emp.department}</td>
                <td>₹{emp.salary.toLocaleString("en-IN")}</td>
              </tr>
            ))
          )}
        </tbody>
      </table>

      <div className="pagination">
        <button
          disabled={safePage <= 1}
          onClick={() => setPage((p) => Math.max(1, p - 1))}
        >
          Previous
        </button>
        <span>
          Page {safePage} of {totalPages}
        </span>
        <button
          disabled={safePage >= totalPages}
          onClick={() => setPage((p) => Math.min(totalPages, p + 1))}
        >
          Next
        </button>
      </div>
    </section>
  );
}
```

---

## Coding Challenge 2: Reusable `useLocalStorage` Custom Hook with Cross-Tab Synchronization

```jsx
import { useState, useEffect, useCallback } from "react";

export function useLocalStorage(key, initialValue) {
  // 1. Lazy initializer safely reads from localStorage once on mount
  const [storedValue, setStoredValue] = useState(() => {
    if (typeof window === "undefined") return initialValue;
    try {
      const item = window.localStorage.getItem(key);
      return item !== null ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(`Error reading localStorage key "${key}":`, error);
      return initialValue;
    }
  });

  // 2. Setter supports both direct values and functional updates (prev => next)
  const setValue = useCallback(
    (value) => {
      setStoredValue((prev) => {
        const nextValue = value instanceof Function ? value(prev) : value;
        try {
          window.localStorage.setItem(key, JSON.stringify(nextValue));
        } catch (error) {
          console.error(`Error writing localStorage key "${key}":`, error);
        }
        return nextValue;
      });
    },
    [key]
  );

  // 3. Synchronize state across multiple open browser tabs!
  useEffect(() => {
    function handleStorageChange(e) {
      if (e.key === key && e.newValue !== null) {
        setStoredValue(JSON.parse(e.newValue));
      }
    }
    window.addEventListener("storage", handleStorageChange);
    return () => window.removeEventListener("storage", handleStorageChange);
  }, [key]);

  return [storedValue, setValue];
}
```

---

## Final React Interview Success Checklist

Before any React interview, review these 7 golden rules:
1. **Treat State as Immutable and Derived State as Computed:** Never mutate arrays/objects in place, and never sync state in `useEffect` when a value can be derived directly during render.
2. **Understand Closures and Render Snapshots:** Every render has its own props, state, and event handlers; use functional updates (`c => c + 1`) or refs when reading across async ticks.
3. **Use Keys for Identity, Not Just Suppressing Warnings:** Never use array `index` as a key on dynamic lists, and remember you can change a component's `key` to intentionally reset its state.
4. **Think of `useEffect` as External Synchronization, Not a Lifecycle Dump:** Always pair subscriptions with cleanup functions and `fetch` calls with `AbortController`.
5. **Separate Server State from Client UI State:** Reach for TanStack Query for API caching/invalidation and keep global client state minimal.
6. **Measure Before Memoizing:** Profile with React DevTools Profiler, use state colocation and component composition (`{children}`) first, and apply `React.memo`/`useMemo`/`useCallback` (or the React Compiler) strategically.
7. **Build Accessible, Secure Production Apps:** Use semantic HTML (`<button>`), manage keyboard focus in modals, sanitize untrusted HTML, and configure SPA fallback routing (`try_files`) in Nginx/Docker.
