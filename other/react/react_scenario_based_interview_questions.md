# React Interview Preparation: Complete Scenario-Based Questions and Answers

This guide focuses on the questions interviewers ask when they want to know whether you have actually developed React applications in the real world, not just memorized definitions. Every answer is written in **clear, natural, spoken-style sentences with minimal code**, so you can articulate your thought process, diagnostic steps, and architectural trade-offs effortlessly in an interview.

We cover three major areas across **100 complete scenario-based questions and answers**:

1. **Sentence-based scenario questions:** Questions such as *"Suppose your API returns duplicate records. How would you handle them in React?"*
2. **Project-based interview questions:** How you used React in your own projects (using **SeatSync, a smart examination seating and invigilation platform**, as a realistic template covering components, Hooks, API integration, state management, routing, performance, and error handling).
3. **Practical and advanced production scenarios:** Debugging, security boundaries, testing, deployment, and explaining why you chose one React approach over another.

---

# Part 1: React Scenario-Based Interview Questions

## Section 1: React Fundamentals and Components (Q1–Q5)

### Q1. Suppose you have to develop a large application with 20 different pages. How would you structure the React application?

**Answer:**

I would use a **feature-based (domain-driven) component architecture** rather than grouping files purely by technical type. In a 20-page application like the SeatSync examination platform, I would organize the `src` directory into:
- An `app/` folder for top-level setup such as the router, global context providers, and root application shell.
- A shared `components/` folder strictly for domain-agnostic UI building blocks like buttons, modals, data tables, and spinners.
- A `layouts/` folder for role-specific page wrappers such as `AdminLayout` and `StudentLayout` that keep navigation headers and sidebars consistent across screens.
- A `features/` directory where each business domain—such as `students`, `examinations`, `seating`, and `invigilation`—contains its own pages, domain components, custom Hooks, API service functions, and TypeScript types.
- Shared `services/`, `hooks/`, and `types/` directories for cross-cutting utilities like the configured HTTP client.

```text
src/
├── app/          (App.tsx, router.tsx, providers.tsx)
├── components/   (Button.tsx, Modal.tsx, DataTable.tsx)
├── layouts/      (AdminLayout.tsx, StudentLayout.tsx)
├── features/     (students/, examinations/, seating/, invigilation/)
├── services/     (apiClient.ts)
├── hooks/        (useDebounce.ts, useAuth.ts)
└── types/
```

**Why would I choose this structure?**
Reusable UI primitives eliminate visual and code duplication, while feature folders keep related code colocated so developers can work on the seating module without accidentally breaking the student module. Centralizing the API client ensures consistent authentication headers and error handling, and separating responsibilities makes unit testing straightforward. For a tiny two-page app I would keep the folder structure flat, but for 20+ pages, feature-based modularity scales cleanly across a team.

**Follow-up: What is the difference between a component and a page?**
A page represents a complete route or screen mapped to a URL, such as `StudentListPage`, and is responsible for coordinating data fetching and layout. A component is a smaller, reusable UI building block—such as `StudentTable`, `SearchInput`, or `Pagination`—that can be composed inside one or many pages.

---

### Q2. Suppose the same button is used on 15 pages, but its design needs to change. How would you handle this?

**Answer:**

Instead of maintaining 15 separate HTML `<button>` elements scattered across different pages, I would extract a single reusable `Button` component in the shared UI folder and use it everywhere. That component would accept configurable props such as `children` for the label, `variant` (for example `"primary"`, `"secondary"`, or `"danger"`), `disabled`, `type` (defaulting to `"button"` so it does not accidentally submit forms), and an `onClick` handler.

```tsx
<Button variant="primary" onClick={handleSave}>Save</Button>
<Button variant="danger" onClick={handleDelete}>Delete</Button>
```

When the design system changes—such as updating border radius, padding, or brand colors—I only need to update that single `Button` component and its stylesheet, and all 15 pages update automatically. However, I would avoid forcing a button with completely different behavior into the same component if it requires dozens of confusing conditional flags; component reuse should simplify maintenance, not create a brittle component.

---

### Q3. Suppose a child component needs information from its parent. How would you pass the data?

**Answer:**

I would pass the information down from the parent component to the child component using **props**. In React's one-way data flow model, the parent owns the data and passes values such as `name` and `registerNumber` as JSX attributes on the child component (`<StudentCard name="Anu" registerNumber="STU101" />`), and the child receives those values as properties on its function parameter object.

**Follow-up: Can a child directly modify its parent's props?**
No. Props in React are strictly read-only inputs, and a child component must never mutate them directly. If the child needs to update data owned by the parent, the parent passes a **callback function** down as a prop (for example, `onUpdateName`), and the child invokes that callback with the new value when a user interaction occurs.

---

### Q4. Suppose five components need access to the currently logged-in user's information. Would you pass props through every component?

**Answer:**

No, not if those five components are deeply nested across different branches of the application tree. Passing props through multiple intermediate layout components that do not actually use the data is known as **prop drilling**, which clutters component interfaces and couples unrelated components together.

Instead, for low-frequency global data like the currently authenticated user's profile and role, I would create a **React Context** (`UserContext`) with a Provider near the top of the application and a custom Hook (`useUser()`) so any of those five components can read the current user directly.

**How would I choose between props, Context, and external state management?**
- I use **props** when the parent-to-child relationship is direct and local (one or two levels deep).
- I use **component composition (`children`)** when an intermediate layout wrapper simply needs to render child elements without knowing about their data.
- I use **React Context** when many distant components need the same low-frequency value, such as authentication status, theme, or locale.
- I use an **external state library** (like Zustand or Redux Toolkit) or **TanStack Query** when state updates frequently or requires fine-grained selector subscriptions and caching.

---

### Q5. Suppose changing one small part of the page causes many components to render again. How would you investigate it?

**Answer:**

First, I would verify whether the extra re-renders are actually causing a noticeable performance lag, because fast virtual DOM re-renders are normal in React. If there is a real slowdown, I would follow a structured diagnostic process:

1. Open **React Developer Tools Profiler**, enable the option to record why each component rendered, and record the slow interaction.
2. Check which component's state update initiated the render cascade.
3. Check whether **state is placed too high** in the component tree. For example, if a search input's typing state lives in the top-level `Dashboard` component, every keystroke re-renders the entire dashboard. Moving that state down into a dedicated `SearchBar` child component (**state colocation**) isolates the re-render immediately.
4. Check whether parent components can use `children` composition so stateful wrappers do not force heavy child trees to re-execute.
5. Check whether parent components are creating brand-new object literals, arrays, or inline arrow functions on every render and passing them to children.
6. Only for genuinely expensive subtrees identified by the Profiler, wrap the child component in `React.memo` and stabilize its object or callback props with `useMemo` or `useCallback`.

---

## Section 2: JSX, Rendering, and Conditional UI (Q6–Q10)

### Q6. Suppose the API returns 100 student records, and you need to display them in a table. How would you render the list?

**Answer:**

I would iterate over the `students` array inside the table's `<tbody>` using JavaScript's `Array.prototype.map()` method, returning a `<tr>` element for each student and assigning a unique, stable `key` prop—such as `key={student.id}`—to each row.

```tsx
<tbody>
  {students.map((student) => (
    <tr key={student.id}>
      <td>{student.name}</td>
      <td>{student.registerNumber}</td>
    </tr>
  ))}
</tbody>
```

**Why is the `key` important?**
The `key` gives each element a stable identity across renders so React's reconciliation algorithm can accurately determine which rows were added, removed, or reordered without destroying and recreating unchanged DOM nodes or mixing up child state. A unique database ID is always preferred over the array index.

**What if the list contains 100,000 records?**
Rendering 100,000 DOM rows at once would freeze the browser main thread and consume massive memory. I would implement **server-side pagination** so only 20 to 50 records are fetched and rendered per page, or use **list virtualization (windowing)** with a library like TanStack Virtual so only the ~20 rows currently visible in the scroll viewport exist in the DOM at any moment.

---

### Q7. Suppose the application displays a blank screen while data is loading. How would you improve the experience?

**Answer:**

I would explicitly model all four states of an asynchronous UI—**Loading, Error, Empty, and Success**—and render clear visual feedback for each state instead of leaving the screen blank:

1. **Loading state:** When the request is in flight, render a skeleton placeholder matching the table layout or an accessible loading spinner so the user knows the system is working.
2. **Error state:** If the request fails, render an alert banner (`role="alert"`) with a user-friendly message and a "Retry" button.
3. **Empty state:** If the request succeeds but returns zero records (`students.length === 0`), render a helpful message such as *"No students found matching your filter"* with a button to clear filters or add a student.
4. **Success state:** Render the actual student table when records are present.

During subsequent background refreshes or page changes, I would keep the previous data dimmed on screen while showing a subtle progress indicator rather than replacing the whole page with a blank loader, preventing jarring layout shifts.

---

### Q8. Suppose an API returns a list of students, but you need to display only students whose attendance is below 75%. How would you do it?

**Answer:**

Before mapping the array into JSX elements, I would derive a filtered subset using JavaScript's non-mutating `filter()` method (`const lowAttendanceStudents = students.filter(s => s.attendance < 75)`) and then map over that filtered array to render the list. If the student list is large and the component re-renders frequently due to unrelated state, I would wrap the filter calculation in `useMemo`.

For very large datasets in production, I would pass the filter threshold as a query parameter to the backend API (`/api/students?maxAttendance=75`) so the database filters the records using an index rather than sending thousands of unnecessary records over the network. Importantly, if attendance below 75% disqualifies a student from generating a hall ticket, that business rule must also be enforced by the backend API, because client-side filtering is only for display.

---

### Q9. Suppose a user clicks Delete, but you want to ask for confirmation before deleting the record. How would you implement it?

**Answer:**

Instead of calling the delete API immediately when the user clicks the Delete button, I would open a **confirmation modal dialog** and store the targeted student's ID and name in component state (for example, `studentToDelete`). The modal would clearly state which student is about to be deleted and provide two buttons: **Cancel** (which closes the modal and clears `studentToDelete`) and **Confirm Delete** (which triggers the asynchronous `DELETE` request).

While the deletion request is in progress, I would disable the Confirm and Cancel buttons and display `"Deleting..."` on the button so the user cannot accidentally click twice. Once the backend confirms deletion, I would close the modal, refresh or remove the student from the table state, and show a toast notification. If the API fails, I would keep the modal open and display the error reason so the user can retry.

---

### Q10. Suppose a condition is false, but a React component unexpectedly displays the number `0`. What might be wrong?

**Answer:**

This happens when a developer writes a conditional JSX expression using a numeric count on the left side of the logical AND (`&&`) operator, such as `{count && <p>You have notifications</p>}`.

In JavaScript, the `&&` operator does not convert its left side into a boolean; if the left operand is falsy, it returns that exact left operand. When `count` is `0`, `0 && <p>...</p>` short-circuits and evaluates to the number `0`. Unlike `false`, `null`, or `undefined` (which React ignores), `0` is a valid renderable number in JSX, so React prints `"0"` directly on the page.

To fix this, always convert the left side into an explicit boolean expression (`{count > 0 && <p>You have notifications</p>}`) or use a ternary operator (`{count > 0 ? <p>...</p> : null}`).

---

## Section 3: State and Hooks (Q11–Q15)

### Q11. Suppose a counter increases by three, but the displayed value increases by only one. Why can this happen?

**Answer:**

This happens when an event handler calls `setCount(count + 1)` three times in a row using the state variable directly instead of a functional updater.

In React, state behaves like an **immutable snapshot** for each render. If `count` is `0` in the current render, the variable `count` remains `0` throughout the entire execution of the click handler. Calling `setCount(count + 1)` three times simply executes `setCount(0 + 1)` three times, telling React three times to set the next state to `1`.

To increment three times in a single event handler, you must pass a **functional updater** (`setCount(prev => prev + 1)`) to each call. React places those updater functions in a queue and passes the output of each function as the input to the next (`0 -> 1 -> 2 -> 3`), producing `3` on the next render.

---

### Q12. Suppose changing the state of a form input does not update the input displayed on the screen. What would you check?

**Answer:**

I would check how the `<input>` element's `value` and `onChange` attributes are wired up:

1. **Missing `onChange` handler or forgotten state setter:** If `<input value={name} />` is rendered without an `onChange` handler that calls `setName(e.target.value)`, the input is locked to the initial state value and becomes read-only.
2. **Reading the wrong event property:** For example, reading `e.target.value` on a checkbox instead of `e.target.checked`, or mismatches in the input's `name` attribute when updating a shared form state object (`[e.target.name]: e.target.value`).
3. **Mutating the state object in place:** If the handler mutates `formData.name = e.target.value` and calls `setFormData(formData)` with the exact same object reference, React sees `Object.is(prev, next) === true` and skips re-rendering the input.
4. **Parent resetting state on every keystroke:** Check whether a parent component is generating a new `key` prop on every render or calling `setName("")` inside an unintended `useEffect`.

**Follow-up: What is the difference between controlled and uncontrolled inputs?**
In a **controlled input**, React state is the single source of truth (`value` is driven by state and updated on every keystroke via `onChange`), which is ideal for live validation and dynamic conditional fields. In an **uncontrolled input**, the browser DOM node itself maintains the input's current value (initialized with `defaultValue`), and React reads the value only when needed via a `ref` or `FormData` on form submission.

---

### Q13. Suppose you use `useEffect` to fetch student data, but the request runs repeatedly and the page becomes slow. How would you debug it?

**Answer:**

An endlessly repeating API request inside `useEffect` is caused by an **infinite render-effect loop**. I would check three common culprits:

1. **Missing dependency array:** Writing `useEffect(() => { fetchStudents().then(setStudents); })` without a second argument causes the Effect to run after **every single render**. Since `setStudents` triggers a re-render, the Effect runs again after that render, creating an infinite loop. Adding an empty dependency array `[]` (or the specific filter dependencies) fixes this.
2. **State updated inside the Effect is also listed in the dependency array:** If the Effect depends on `[students]` and also calls `setStudents(newData)`, every fetch triggers another fetch.
3. **Unstable object, array, or function reference in the dependency array:** If a component creates a filter object `const filters = { department: "CSE" }` during render and passes `[filters]` as a dependency to `useEffect`, JavaScript allocates a brand-new object reference on every render. Because `Object.is(prevFilters, nextFilters)` is always `false`, the Effect re-runs on every render. To fix this, depend on primitive values (`[department]`) or memoize the object/function using `useMemo` or `useCallback`.

---

### Q14. Suppose a component needs to store the ID of a timer, but changing that ID should not cause a render. Which Hook would you use?

**Answer:**

I would use **`useRef`** (`const timerIdRef = useRef<number | null>(null)`).

`useRef` creates a persistent mutable container object with a `.current` property that survives across the entire lifetime of the component. When starting the interval, I store the returned timer ID in `timerIdRef.current`, and when stopping the timer or unmounting the component inside a `useEffect` cleanup function, I pass `timerIdRef.current` to `clearInterval()`.

**Why `useRef` instead of `useState`?**
Updating `useState` schedules a component re-render because React assumes state changes affect the visible UI. A timer ID is an internal bookkeeping handle that is never displayed on screen; mutating `timerIdRef.current` updates the value synchronously and immediately without triggering unnecessary re-renders.

---

### Q15. Suppose several components need to update the same shopping-cart total. How would you manage the state?

**Answer:**

First, I would ensure that the **shopping-cart total itself is never stored as a separate state variable**. Instead, the single source of truth should be the **array of cart items** (each with `id`, `price`, and `quantity`), and the cart total should be **derived** on the fly during render using `cartItems.reduce((sum, item) => sum + item.price * item.quantity, 0)`. Storing both `cartItems` and `total` as separate states invites synchronization bugs where items change but the total forgets to update.

To share `cartItems` and cart actions (`addItem`, `removeItem`, `updateQuantity`, `clearCart`) across multiple components—such as the product card, header cart badge, and checkout summary—I would manage the items using `useReducer` paired with React Context (or a lightweight store like Zustand) so any component can dispatch cart actions predictably.

---

# Part 2: API Integration and Real-World Scenarios

## Section 4: Fetching Data From an API (Q16–Q20)

### Q16. Suppose your React application needs to retrieve student details from a Node.js backend. How would you implement it?

**Answer:**

I would manage the full lifecycle of the HTTP request cleanly by tracking three pieces of state: `students` (the data array), `loading` (boolean), and `error` (error message or `null`). Inside `useEffect` (or using a server-state library like TanStack Query), I would initiate the asynchronous `fetch("/api/students")` request:

1. Before calling `fetch`, set `loading` to `true`, clear any previous `error`, and instantiate an `AbortController`.
2. Pass `controller.signal` to `fetch()` so the request can be cancelled if the user navigates away before it finishes.
3. Check `if (!response.ok)` explicitly—because `fetch()` only rejects its Promise on network failures, not on HTTP `404` or `500` status codes—and throw an error if the status is not `2xx`.
4. Parse the JSON response via `await response.json()` and update `students`.
5. In the `catch` block, ignore `AbortError` (since cancellation is intentional) and store any real error message in `error` state.
6. In the `finally` block, set `loading` to `false` provided the request was not aborted, and return `() => controller.abort()` as the Effect cleanup function.

In a larger production application, I would extract the raw `fetch` call into a dedicated `studentService.ts` module and wrap it with TanStack Query (`useQuery`) to gain automatic caching, background refetching, and request deduplication.

---

### Q17. Suppose the API request succeeds in Postman but fails in the browser. What would you investigate?

**Answer:**

When a request works in Postman but fails in a browser-based React app, the issue almost always stems from **browser security policies or environment differences** that Postman ignores:

1. **CORS (Cross-Origin Resource Sharing):** Browsers enforce the Same-Origin Policy. If the React dev server runs on `http://localhost:5173` and the Node.js API runs on `http://localhost:5000`, they have different ports and are therefore different origins. I would check the browser Console and Network tab to see if the response is missing the `Access-Control-Allow-Origin: http://localhost:5173` header or if a preflight `OPTIONS` request is being rejected by the backend.
2. **Cookies and Credentials (`withCredentials` / `credentials: "include"`):** Postman sends cookies automatically if configured, whereas browser `fetch()` does not send cross-origin cookies unless `credentials: "include"` is set on the frontend AND `Access-Control-Allow-Credentials: true` (with an explicit origin, never `*`) is set on the backend.
3. **Environment variables and URL resolution:** Check the Network tab's exact Request URL to see if an undefined `import.meta.env.VITE_API_URL` caused the browser to request `http://localhost:5173/undefined/students`.
4. **Mixed Content (`HTTPS` calling `HTTP`):** If the deployed React site runs over `https://` and calls an insecure `http://` API, modern browsers block the request automatically.
5. **Missing or malformed request headers:** Check whether `Content-Type: application/json` or the `Authorization` token is missing in the browser request compared to Postman.

---

### Q18. Suppose a user changes pages or search terms before an API request finishes. The old request then overwrites the new page's data. How would you prevent this?

**Answer:**

This is a classic **asynchronous race condition**: Request A (for `"Anu"`) starts first on a slow connection, Request B (for `"Binil"`) starts 200ms later and finishes quickly, and then Request A finishes last and overwrites the screen with stale results for `"Anu"`.

I would prevent this inside `useEffect` using **`AbortController`** (or a boolean `ignore = true` cleanup flag):
- At the start of the `useEffect` that depends on `[query]`, I create `const controller = new AbortController()` and pass `{ signal: controller.signal }` to `fetch()`.
- In the `useEffect` cleanup function (`return () => controller.abort()`), React automatically aborts the previous in-flight request the exact moment `query` changes or the user navigates to another page.
- Inside the `catch` block, if `error.name === "AbortError"`, I return early without updating state.

For a search input, I would also combine `AbortController` with **debouncing** (waiting 250–300ms after the user stops typing before firing the request). Debouncing reduces how many requests are sent to the server, while `AbortController` guarantees that an older slow response can never overwrite a newer fast response.

---

### Q19. Suppose a user clicks the Save button multiple times, and the backend creates duplicate records. How would you solve this?

**Answer:**

I would solve this with defense-in-depth on **both the frontend and the backend**:

1. **Frontend protection (Immediate UX Guard):**
   I track an `isSubmitting` boolean state (or check a synchronous `useRef` lock `isSavingRef.current` to block rapid sub-millisecond double-clicks before state re-renders). The moment the user clicks Save, I set `isSubmitting = true`, disable the Save button (`disabled={isSubmitting}`), and change its label to `"Saving..."` until the `try / finally` block completes.
2. **Backend protection (Authoritative Data Integrity):**
   Disabling a frontend button alone is never enough because network retries, multiple open tabs, or direct API calls can still send duplicate requests. On the backend, I would enforce **database uniqueness constraints** (for example, a `UNIQUE` index on `register_number` or `(exam_id, student_id)`) and support an **`Idempotency-Key` header** (a UUID generated by the React form when opened) so repeated submissions of the same form return the existing record instead of inserting duplicates.

---

### Q20. Suppose your API returns HTTP 401, 403, 404, or 500. How should the React application respond?

**Answer:**

Rather than showing a generic `"Something went wrong"` alert for every failure, the React application should tailor its response to the exact HTTP status code semantics:

- **`400 Bad Request` / `422 Unprocessable Entity`:** The user submitted invalid form data. Keep the user on the form, preserve everything they typed, highlight the specific invalid fields using the field error map returned by the API, and move keyboard focus to the first invalid input.
- **`401 Unauthorized`:** The user is unauthenticated or their access token expired. In an API client interceptor, attempt a silent token refresh once using the refresh token; if refresh fails, clear client session state and redirect the user to `/login` while saving their current URL so they can return after signing back in.
- **`403 Forbidden`:** The user *is* logged in validly, but their role (for example, `"student"`) lacks permission to perform the action. **Do not log them out or send them to `/login`** (which causes redirect loops); instead, display an access-denied message or redirect to a `/forbidden` explanation screen.
- **`404 Not Found`:** The specific student or examination ID in the URL does not exist. Display a clean "Record Not Found" empty state with a link back to the main directory.
- **`409 Conflict`:** A duplicate record (such as an existing register number) or concurrent edit conflict occurred. Explain the conflict clearly so the user can adjust their input or reload the latest data.
- **`429 Too Many Requests`:** Rate limit exceeded. Disable the action temporarily and inform the user when they can try again based on the `Retry-After` header.
- **`500 Internal Server Error` / `503 Service Unavailable`:** A backend server failure occurred. Never expose raw SQL errors or stack traces to the user; display a friendly error message with a "Retry" button and log the correlation ID to your error monitoring service.

---

## Section 5: Forms, Validation, and File Uploads (Q21–Q23)

### Q21. Suppose you are developing a student registration form with ten fields. How would you validate it?

**Answer:**

For a ten-field form (covering name, register number, email, phone, department, semester, and date of birth), I would combine **schema-based client validation** (using React Hook Form with a Zod schema, or a structured validation helper) with **server-side validation error mapping**:

1. **Timing of validation (Reward Early, Punish Late):** Avoid showing red error messages while the user is still typing into a field for the very first time. Validate a field on **`onBlur`** (when the user leaves the field) or on form submission, and once a field has an error, re-validate it on **`onChange`** so the error disappears immediately as soon as the user fixes it.
2. **Types of checks:** Check required fields (after `.trim()`), format rules (email format, 10-digit phone number, alphanumeric register number pattern), and cross-field rules.
3. **Accessibility:** Link each inline error message to its `<input>` using `aria-invalid="true"` and `aria-describedby="email-error"` so screen readers announce the validation failure.
4. **Backend validation:** Even after client-side validation passes, handle backend `400`/`409` responses (for example, *"Register number STU101 is already registered"*) and attach that error directly to the `registerNumber` field in the UI.

---

### Q22. Suppose the user uploads an Excel or CSV file containing 5,000 student records. How would you process it in React?

**Answer:**

In SeatSync, bulk-importing 5,000 students requires a staged **Select -> Validate -> Preview -> Confirm Import -> Summary** workflow so bad rows do not corrupt the database:

1. **Client-side file check:** When the user selects or drops a file, immediately verify the file extension (`.xlsx` or `.csv`) and maximum file size (for example, under 10 MB) before doing heavy work.
2. **Non-blocking parsing and preview:** If parsing on the client to show an instant preview, run the spreadsheet parser inside a **Web Worker** (or upload the file to a backend staging endpoint) so parsing 5,000 rows does not freeze the main UI thread.
3. **Validation summary before commit:** Display a summary card showing total rows (`5,000`), valid rows (`4,920`), invalid rows (`60` missing required columns or invalid emails), and duplicate register numbers (`20`), along with a filterable preview table of the problematic rows so the admin can fix or skip them before committing.
4. **Chunked or asynchronous backend import:** Send the validated file (`FormData`) to the backend, show an upload/import progress bar, and display a downloadable error report for any rows rejected by the server.

---

### Q23. Suppose the user navigates away while a large file upload is in progress. What should happen?

**Answer:**

I would choose the behavior intentionally based on product expectations:

- **Option 1: Warn before accidental navigation and cancel on unmount (Standard Page-Scoped Upload):**
  Attach a `beforeunload` listener while `isUploading` is true (and use React Router's `useBlocker`) to warn the user: *"An upload is currently in progress. Leaving this page will cancel the upload."* If the user confirms navigation, the component's `useEffect` cleanup aborts the request via `AbortController`, and the backend discards the incomplete multipart stream.
- **Option 2: Global Background Upload Manager (Google Drive style UX):**
  If users are expected to browse other pages while large imports run, lift the upload state and `XMLHttpRequest` / `fetch` call out of the local page component into a **global `UploadProvider` mounted at the root layout**. When the user navigates to another route, the upload continues uninterrupted in the background and displays a floating progress widget in the bottom-right corner of the screen!

---

# Part 3: Routing, Authentication, and Authorization (Q24–Q26)

### Q24. Suppose you have separate Admin, Invigilator, and Student dashboards. How would you implement navigation?

**Answer:**

I would use **React Router** with nested layout routes organized by role (`/admin/*`, `/invigilator/*`, and `/student/*`):

- Each role section wraps its child routes in a role-specific layout (`AdminLayout`, `InvigilatorLayout`, `StudentLayout`) containing `<Outlet />` and a `<ProtectedRoute allowedRoles={[...]}>` guard.
- When a user logs in, the application checks `user.role` from the authentication response and redirects them to their respective home dashboard using `navigate("/admin", { replace: true })`.
- Furthermore, I would lazily load each dashboard bundle using `React.lazy(() => import("./features/admin/AdminDashboard"))` wrapped in `<Suspense>` so a student logging in on a mobile phone never downloads the heavy JavaScript code for the Admin seating-allocation engine.

---

### Q25. Suppose a student manually types `/admin` into the browser's address bar. How would you prevent unauthorized access?

**Answer:**

I would enforce authorization at **two mandatory layers**:

1. **Frontend Route Guard (`RequireRole` / `ProtectedRoute`):**
   Every `/admin/*` route is wrapped in a guard component that reads the current authenticated user's session and role. If the user is not logged in at all, it redirects to `/login` (`<Navigate to="/login" replace />`). If the user is logged in as a `"student"` rather than an `"admin"`, it immediately blocks rendering of the admin component and redirects to a `/forbidden` (`403 Access Denied`) screen or back to `/student`.
2. **Backend API Authorization Middleware (The True Security Boundary):**
   Frontend route protection only hides UI screens; a technical student could still inspect the JavaScript bundle and send HTTP requests directly to `GET /api/admin/exams` using `curl` or Postman. Therefore, every backend API endpoint must independently verify the user's cryptographically signed session/JWT token and confirm `user.role === "admin"` before returning any data or executing any mutation.

---

### Q26. Suppose a user logs in successfully, refreshes the browser (F5), and is unexpectedly logged out. How would you investigate?

**Answer:**

Refreshing the browser wipes out all in-memory JavaScript variables (including React `useState`, Context, and Redux/Zustand memory). I would investigate two common causes of this bug:

1. **Race condition during initial Auth Provider hydration (Most Common React Bug!):**
   On page reload, `AuthProvider` initializes with `user = null` and starts an asynchronous request (`GET /api/auth/me` or silent token refresh) to restore the session. If `ProtectedRoute` checks `if (!user) return <Navigate to="/login" />` **before** waiting for `isInitializingAuth` to become `false`, it immediately kicks the user out to `/login` while the session check is still in flight! Fixing `ProtectedRoute` to render a loading spinner while `isInitializingAuth === true` solves this immediately.
2. **Session persistence / Cookie transmission issue:**
   If `GET /api/auth/me` itself returns `401` on refresh, I would inspect the browser Network and Application tabs to check whether the `HttpOnly` session/refresh cookie was blocked due to missing `credentials: "include"` on the `fetch` call, a mismatched cookie `Domain`/`Path`, `SameSite=Strict` cross-origin restrictions, or `Secure` cookie flags on an `http://` staging environment.

---

# Part 4: State Management and Application Data (Q27–Q29)

### Q27. Suppose the same student information is displayed on the dashboard, profile page, and attendance page. How would you avoid inconsistent data?

**Answer:**

If each page fetches and stores its own isolated copy of student data inside local `useState`, updating a student's name on the Profile page leaves stale data on the Dashboard and Attendance pages until a full page reload.

To guarantee consistency, I would establish a **single shared server-state cache** using **TanStack Query (React Query)** keyed by a structured query key such as `["students"]` and `["students", studentId]`. All three pages read from that centralized cache. Whenever an administrator edits a student's profile or attendance via a mutation (`useMutation`), the `onSuccess` callback invalidates `queryClient.invalidateQueries({ queryKey: ["students"] })` (or updates the cache entry directly with `setQueryData`), automatically synchronizing every mounted and subsequently visited page across the application.

---

### Q28. Suppose an API provides 10,000 records, but the user needs only the first 20. Would you fetch all records and filter them in React?

**Answer:**

No. Fetching all 10,000 records upfront just to display 20 rows wastes megabytes of mobile bandwidth, increases API response latency, consumes excessive browser memory, and slows down JSON parsing on the main thread.

Instead, I would use **server-side pagination, sorting, and filtering**:
- The React component passes the current page, page size, search term, and sort column as URL query parameters (`GET /api/students?page=1&limit=20&search=anu&sortBy=name`).
- The backend queries only those 20 rows using database indexes (`LIMIT 20 OFFSET 0` or cursor pagination) and returns `{ items, totalCount, page, totalPages }`.
- I would also synchronize `page` and `search` with the browser URL (`useSearchParams`) so bookmarking or refreshing the page keeps the user on the exact same page of results.
- Client-side filtering is only appropriate when the dataset is small (for example, under a few hundred records) and already needed in full on the client.

---

### Q29. Suppose the user updates a record (such as toggling a student's attendance checkbox) and expects the change to appear immediately without waiting for the server. What technique could you use?

**Answer:**

I would implement an **Optimistic UI Update**:

1. **Snapshot current state:** Save a reference to the previous attendance array (`const previousStudents = students`) before modifying anything.
2. **Update UI immediately:** Update the React state (or TanStack Query cache via `onMutate`) right away so the checkbox flips instantaneously with zero perceived network lag.
3. **Send request in background:** Fire the `PATCH /api/attendance/:id` request to the backend asynchronously.
4. **Rollback on failure:** If the server returns an error or network failure, catch the error, restore `setStudents(previousStudents)` to roll back the checkbox to its accurate state, and display an error toast explaining that the attendance update failed to save.
5. **Avoid optimistic updates for high-stakes operations:** While optimistic updates are great for attendance toggles, bookmarks, or likes, I would **never** use optimistic updates for financial payments, seat allocation commits, or final exam submissions where authoritative server confirmation is mandatory before showing success.

---

# Part 5: How React Was Used in a Real Project (SeatSync Template — Q30–Q37)

### Q30. Explain your project and describe how React was used in it.

**Answer:**

"I worked on **SeatSync**, an examination seating and invigilation management platform built to automate and streamline university examination workflows. The platform manages student directories, exam schedules, classroom capacities, automated seating allocation, and invigilator duty rosters across Admin, Invigilator, and Student roles.

On the frontend, I used **React with TypeScript** to build a modular, single-page application. Instead of building monolithic screens, I organized the UI into reusable layouts, generic data tables, filter bars, modal dialogs, and role-based dashboard views. React managed all interactive client state—such as search filters, multi-step bulk student import previews, pagination controls, and modal workflows—while communicating with our backend REST APIs for persistent data and business logic.

We used React Router with role-based route guards to separate the Admin, Invigilator, and Student portals, centralized our HTTP calls inside typed API service modules with `AbortController` cancellation, and ensured every asynchronous screen explicitly handled loading skeletons, empty states, validation errors, and user notifications."

---

### Q31. How would you divide an examination management application into React components?

**Answer:**

I would decompose the interface by **Single Responsibility and State Colocation** so each component has one clear job:
- **Layout Layer (`AppLayout`, `Sidebar`, `Header`):** Provides persistent navigation and user profile controls based on the logged-in role.
- **Dashboard Overview (`DashboardSummary`, `SummaryCard`, `RecentActivity`):** Displays high-level metrics such as upcoming exams, total students, and unassigned rooms.
- **Feature Containers (`ExaminationSection`, `SeatingAllocationPage`):** Coordinates data fetching and connects filters, tables, and modals for a specific domain.
- **Interactive Controls (`ExamFilters`, `SearchInput`, `Pagination`):** Collects user filter criteria and page changes without knowing how the table renders rows.
- **Presentation Components (`ExamTable`, `ExamRow`, `StatusBadge`, `ExamDetailsModal`):** Purely receives typed data and callbacks via props to render rows, status pills (`Scheduled`, `Allocated`, `Completed`), and detail overlays.

Splitting components at these natural boundaries makes the code easy to test and prevents filter input state from re-rendering unrelated parts of the layout.

---

### Q32. How would you implement a reusable table for students, rooms, and examinations?

**Answer:**

Rather than writing three separate HTML table components for Students, Rooms, and Examinations, I would build a generic, TypeScript-typed `<DataTable<T>>` component driven by a **column configuration array**:

- The `DataTable<T>` component accepts `data: T[]`, `columns: Column<T>[]`, `isLoading`, `emptyMessage`, and an optional `onRowClick` callback.
- Each `Column<T>` definition specifies a `key` (or unique `id`), a header `title`, and an optional custom `render(row: T)` function.
- If a column only needs to display plain text (such as `student.registerNumber` or `room.capacity`), the table renders `row[column.key]`. If a column needs custom UI—such as a colored `<StatusBadge>` or action buttons (`Edit`, `Delete`)—the caller provides a `render` function for that column.

This pattern lets all three modules share consistent styling, loading skeletons, empty states, keyboard accessibility, and sorting headers while still supporting custom cell content per feature.

---

### Q33. Suppose the Admin, Invigilator, and Student dashboards have different navigation menus. How would you implement them?

**Answer:**

I would define a declarative **navigation configuration map keyed by role** (`navigationByRole: Record<Role, NavItem[]>`) and pass the authenticated user's role into a shared `<Sidebar>` component.

For example, the `"admin"` key maps to Dashboard, Students, Rooms, Examinations, and Seating Allocation; `"invigilator"` maps to Assigned Duties and Room Attendance; and `"student"` maps to My Exam Schedule and My Seat Lookup. The `<Sidebar>` component simply looks up `navigationByRole[user.role]` and renders each item using React Router's `<NavLink>` component, which automatically applies active styling and `aria-current="page"` to the currently selected route.

Importantly, hiding links in the sidebar is strictly for user experience; every route is still protected by `<ProtectedRoute>`, and every backend endpoint verifies the user's role independently.

---

### Q34. How does data flow from the database to the React component?

**Answer:**

In a full-stack React and Node.js architecture, React **never** connects directly to the database. Instead, data flows through a clean, layered pipeline:

1. **UI Trigger:** The user navigates to the Students page, mounting the React component.
2. **Frontend Request:** The React component (via a custom Hook or API service) sends an authenticated HTTP `GET /api/students?page=1` request to the Node.js backend.
3. **Backend Middleware & Validation:** The Node.js server verifies the user's authentication token, checks role permissions, and validates query parameters.
4. **Database Query:** The backend service/repository layer queries the database using parameterized queries or an ORM.
5. **Serialization & Response:** The database returns the rows to Node.js, which formats them into a safe DTO JSON payload (stripping out sensitive fields) and sends an HTTP `200 OK` response.
6. **React State & Reconciliation:** The frontend parses the JSON, updates its React state or query cache, and triggers a re-render to display the student rows in the DOM.

---

### Q35. Why would you create an API service instead of calling `fetch()` directly in every component?

**Answer:**

Calling raw `fetch("/api/...")` directly inside dozens of React components duplicates base URLs, headers, error parsing, and token-refresh logic across the entire codebase. If an API endpoint path or authentication header format changes, you would have to hunt down and edit every component.

By extracting network calls into a centralized `apiClient` and domain service modules (like `studentService.ts` and `examService.ts`):
1. **Base URLs, timeouts, and auth headers** are configured in one place.
2. **Global error handling** (such as intercepting `401 Unauthorized` responses to refresh tokens or formatting backend error payloads consistently) happens automatically.
3. **Components remain clean and focused on UI state** rather than low-level HTTP status checks.
4. **Unit testing is much easier** because tests can mock the service function or intercept the centralized client cleanly.

---

### Q36. How would you implement a student search feature?

**Answer:**

My implementation depends on the size of the student dataset:

- **For a small dataset already loaded in the browser (e.g., 100 students in a single room):**
  I store the `query` string in `useState` bound to a controlled search `<input>`, normalize the query with `query.trim().toLowerCase()`, and compute `filteredStudents` using `useMemo` to check whether each student's `name` or `registerNumber` includes the query string.
- **For a large institution-wide dataset (e.g., 10,000+ students):**
  I store the immediate input text in `searchInput` state so typing feels instantaneous, pass `searchInput` through a custom **`useDebounce(searchInput, 300)`** Hook so an API request only fires 300ms after the user stops typing, reset pagination to `page = 1` whenever the search term changes, and use **`AbortController`** inside the fetch Effect (or TanStack Query) so any slower previous search request is cancelled immediately if the user types again.

---

### Q37. How would you handle an examination seating allocation operation that takes several seconds?

**Answer:**

Because assigning 2,000 students across 50 exam halls while respecting anti-cheating subject constraints can take several seconds, simply showing a generic spinner on a synchronous HTTP request risks browser timeouts, accidental double-clicks, or lost state if the admin refreshes the page.

I would design the React UI around an **asynchronous job status pattern**:
1. When the admin clicks **"Generate Seating Plan"**, immediately disable the button and send `POST /api/seating/allocate`.
2. If the operation takes more than a second or two, the backend creates an allocation job and immediately returns `202 Accepted` with a `jobId` and initial status `"PROCESSING"`.
3. The React UI transitions to a clear progress card showing the job status (`Validating student counts -> Assigning rooms -> Finalizing seat matrix`) and polls `GET /api/seating/jobs/:jobId` every 2 seconds (or listens via Server-Sent Events) until the status becomes `"COMPLETED"` or `"FAILED"`.
4. Once `"COMPLETED"`, React automatically fetches and displays the generated room-by-room seating preview. If `"FAILED"` (for example, *"Total student count 600 exceeds available room capacity 520"*), React displays the exact actionable capacity deficit so the admin can add another room and retry.

---

# Part 6: Performance, Debugging, and Production Problems (Q38–Q42)

### Q38. Suppose your React dashboard becomes slow after adding thousands of student records. What would you do?

**Answer:**

Rather than guessing or blindly adding `useMemo` everywhere, I would first **isolate which layer is causing the slowdown**—Network, JavaScript CPU computation, React re-renders, or DOM node count:

1. **Inspect the Network Tab:** Check if the API is sending a massive multi-megabyte JSON payload containing all 10,000 students at once. If so, switch to **server-side pagination** (`?page=1&limit=25`).
2. **Inspect DOM Node Count:** If all 5,000 rows must remain scrollable on a single screen without pagination, rendering 5,000 `<tr>` elements creates tens of thousands of DOM nodes that choke browser layout and paint. Implement **list virtualization** (`@tanstack/react-virtual`) so only the ~25 visible rows are mounted in the DOM.
3. **Inspect React Profiler:** Check if typing into a filter input or ticking a single row checkbox is forcing all 5,000 `<StudentRow>` components to re-render. Colocate input state, debounce the filter, and wrap `<StudentRow>` in `React.memo` with stable callbacks.
4. **Check Expensive JS Calculations:** Ensure array sorting or filtering is not mutating arrays or running `O(n^2)` nested loops on every render; memoize expensive derived data with `useMemo`.

---

### Q39. Suppose React reports a console warning that a list contains duplicate keys. What would you investigate?

**Answer:**

The warning `"Encountered two children with the same key"` means the expression passed to `key={...}` inside `.map()` is not unique among sibling elements.

1. **Check which property is being used as the `key`:** Developers sometimes write `key={student.name}` or `key={student.department}`, which collides as soon as two students share the same name or department. Switch to a guaranteed unique identifier such as `key={student.id}` or `key={student.registerNumber}`.
2. **Check for duplicate records returned by the API or pagination:** In infinite-scroll lists or improperly joined SQL queries, the backend might return the same student record twice across pages. Deduplicate the array by `id` (or fix the backend query) before rendering.
3. **Never fix duplicate keys by using `key={Math.random()}`:** Generating a random key during render silences the warning but destroys performance and resets all child state and DOM focus on every single render!

---

### Q40. Suppose changing one student's attendance accidentally changes another student's attendance. What could be the cause?

**Answer:**

There are three common causes for this bug in React:

1. **Non-unique or index-based `key` props combined with sorting/filtering:** If `<StudentRow key={index} />` holds local checkbox state and the list is filtered or sorted, index `0` now points to a different student while retaining the previous student's checkbox state.
2. **Flawed update condition in `.map()`:** When updating state via `students.map(s => s.department === targetDept ? { ...s, present: true } : s)`, matching on a non-unique field (or an `undefined` ID where `undefined === undefined` matches every student!) updates multiple records instead of the single `student.id === targetId`.
3. **Shallow mutation of shared nested objects:** If multiple student rows accidentally share a reference to the same default attendance object in memory and the handler mutates `student.attendance.present = true` directly instead of returning a new object copy (`{ ...student, present: !student.present }`), mutating one mutates all rows sharing that reference.

---

### Q41. Suppose a component keeps displaying old data after the user changes the selected examination. How would you debug it?

**Answer:**

When selecting a new examination from a dropdown (`examId` changes from `"exam-1"` to `"exam-2"`) still shows `"exam-1"`'s data, I would check:

1. **Missing `examId` in the `useEffect` dependency array or TanStack Query key:** If the data-fetching Effect has `[]` as its dependency array instead of `[examId]`, or if `useQuery({ queryKey: ["exam-details"] })` omits `examId` from the query key (`["exam-details", examId]`), React never refetches when `examId` changes.
2. **Child component copying props into local `useState` on mount:** If a child component initializes `const [details, setDetails] = useState(initialExamProps)` and stays mounted when `examId` changes, `useState`'s initial value is ignored on subsequent renders! Either read the prop directly without copying it into `useState`, or pass `key={examId}` to the child component so React mounts a fresh instance when the exam changes.
3. **Stale closure or race condition:** Check whether an earlier slow request for `"exam-1"` finished after `"exam-2"` and overwrote the state due to a missing `AbortController`.

---

### Q42. Suppose an application works locally (`npm run dev`) but fails after production deployment. What would you check?

**Answer:**

I would compare the local development environment against the production build and hosting environment systematically:

1. **SPA Direct URL Refresh returning `404 Not Found`:** Navigating via `<Link>` works, but refreshing `/admin/students` returns a `404` from Nginx, S3, or Apache because the server looks for a physical `/admin/students/index.html` file. Fix this by configuring the web server's SPA fallback (`try_files $uri $uri/ /index.html;` in Nginx).
2. **Missing or misconfigured `VITE_*` environment variables:** In Vite, environment variables are statically embedded at **build time** (`npm run build`), not read dynamically from the OS at runtime. Check the Network tab to see if API calls are hitting `localhost` or `undefined`.
3. **Case-sensitive Linux file imports:** Windows and macOS filesystems are case-insensitive (`import Button from "./button"` works locally even if the file is `Button.tsx`), whereas Linux Docker/CI build servers are strictly case-sensitive.
4. **CORS, HTTPS Mixed Content, and Secure Cookies:** Check Browser DevTools Console and Network tabs for blocked cross-origin requests, `https://` pages calling `http://` APIs, or cookies rejected due to `SameSite` / `Secure` attributes.

---

# Part 7: Advanced React, Internals, Security & Testing Scenarios (Q43–Q56)

### Q43. Suppose you call `useState` inside an `if` statement. Why can React produce an error?

**Answer:**

React tracks Hook state inside each component's internal **Fiber node** using an ordered linked list based purely on the **sequence in which Hooks are called** during render (`Hook #1 -> Hook #2 -> Hook #3`), because `useState` is not given a string key or variable name.

If you place `useState` inside an `if (isAdmin)` block, then on a render where `isAdmin` is `true`, React executes Hook #1, Hook #2, and Hook #3. If `isAdmin` becomes `false` on the next render, Hook #1 is skipped, so the second Hook in your code suddenly reads the state slot of Hook #1, corrupting state alignment and throwing `Rendered fewer hooks than expected`.

To fix this, always call all Hooks unconditionally at the **very top level** of your component function, before any `if` statements or early `return`s.

**Follow-up: Can a custom Hook call another custom Hook?**
Yes! Custom Hooks are regular functions that compose built-in Hooks or other custom Hooks, as long as they are also called unconditionally at the top level of a component or another custom Hook.

---

### Q44. Suppose an Effect uses a state variable (`count`) inside `setInterval` but does not include `count` in its dependency array `[]`. What could happen?

**Answer:**

The interval callback will suffer from a **stale closure** and log or use the initial value of `count` (`0`) forever, even as `count` changes on screen!

Because the Effect has an empty dependency array `[]`, its setup function runs only once during the first render when `count` is `0`. The arrow function passed to `setInterval` closes over that first render's lexical environment where `count === 0`.

To fix this:
- If the timer only needs to increment `count`, use a **functional state update** (`setCount(c => c + 1)`), which reads the latest pending state without needing `count` in the closure.
- If the timer needs to read `count` for logging or side effects without resetting the interval timer on every tick, mirror `count` into a `useRef` (`latestCountRef.current = count`) or use `useEffectEvent` so the interval always reads the freshest value.

---

### Q45. Suppose a component renders twice and `useEffect` runs twice when you run the application in development. Is React broken?

**Answer:**

No, React is not broken. That behavior is intentionally caused by **`<React.StrictMode>`** in development builds (and never happens in production builds).

React Strict Mode intentionally invokes component functions twice to verify that your rendering logic is a **pure function** with no side effects, and it intentionally runs `setup -> cleanup -> setup` once on initial mount for every `useEffect` to verify that your cleanup function properly undoes whatever your setup function started (such as disconnecting a WebSocket, removing a window event listener, or aborting a duplicate `fetch`).

Instead of removing `<StrictMode>` to hide the double execution, you should ensure your component renders purely and your Effects return a proper cleanup function.

---

### Q46. Suppose a parent component re-renders, and a child component performs an expensive calculation (like sorting 10,000 student records). How would you optimize it?

**Answer:**

First, I would measure the calculation in the React DevTools Profiler to confirm it is taking significant time. Then I would wrap the sorting calculation inside **`useMemo`** with `[students, sortBy]` as its dependencies so React reuses the cached sorted array whenever the parent re-renders for unrelated reasons:

```tsx
const sortedStudents = useMemo(
  () => [...students].sort((a, b) => a.name.localeCompare(b.name)),
  [students]
);
```

Notice that I spread `[...students]` into a shallow copy before calling `.sort()`, because JavaScript's `.sort()` mutates the array in place (or I can use ES2023's non-mutating `students.toSorted(...)`). If the dataset grows even larger, I would move the sorting to the backend database or a Web Worker.

---

### Q47. Suppose a child component is wrapped in `React.memo`, but it still re-renders every time the parent renders because it receives a function prop (`onSave`). Why?

**Answer:**

`React.memo` compares each prop from the previous render against the new render using shallow reference equality (`Object.is(prevProp, nextProp)`).

In JavaScript, declaring a function or inline arrow function inside the parent component (`<Child onSave={() => saveStudent(id)} />`) allocates a **brand-new function object in memory on every single parent render**. Because `prevOnSave !== nextOnSave`, `React.memo` sees that the `onSave` prop changed and re-renders the child anyway.

To make `React.memo` effective, wrap the function in **`useCallback`** inside the parent (`const handleSave = useCallback(() => saveStudent(id), [id])`) so the function reference stays identical across renders unless `id` changes.

---

### Q48. Suppose React updates a component's state. What happens internally?

**Answer:**

Internally, React processes a state update in **three phases**:

1. **Trigger Phase:** Calling `setState` enqueues an update on the component's Fiber node and schedules a render with the React scheduler (batching multiple state updates from the same event together).
2. **Render & Reconciliation Phase (Pure, In-Memory):** React calls the component function with the new state, receives the new tree of lightweight React Element objects (Virtual DOM), and **reconciles (diffs)** this new work-in-progress Fiber tree against the current Fiber tree in memory to compute the minimal set of DOM changes needed.
3. **Commit Phase (Synchronous DOM Update):** React applies only the calculated minimal mutations to the real browser DOM nodes, runs `useLayoutEffect` synchronously before browser paint, lets the browser paint the updated screen, and then runs `useEffect` callbacks asynchronously.

Crucially, **re-rendering a component in React does not mean tearing down and recreating its real DOM nodes**; React only mutates the specific DOM attributes or text nodes that actually changed.

---

### Q49. What is the difference between rendering and committing in React?

**Answer:**

- **Rendering** is the calculation phase where React calls your component functions in memory to figure out what the UI *should* look like next and diffs the returned elements against the previous render. A component can render without producing any visible change in the DOM (if its output is identical), and in Concurrent React, low-priority transition renders can even be paused or discarded before ever reaching the screen.
- **Committing** is the phase where React takes the final diffed changes from the render phase and actually mutates the real browser DOM (`appendChild`, updating text or attributes).

This distinction is why rendering must always be pure and free of side effects, while DOM mutations and external synchronization belong in the commit phase (`useEffect`).

---

### Q50. Suppose a component is removed from the page while a `window` event listener (such as `resize` or `keydown`) is still attached. What problem could occur?

**Answer:**

If a component registers `window.addEventListener("resize", handleResize)` inside `useEffect` and unmounts without removing that listener, the global `window` object retains a reference to `handleResize` and every variable closed over by that function.

This causes two production problems:
1. **Memory leak:** The unmounted component's closure cannot be garbage-collected, and every time the user visits and leaves that page, another duplicate listener piles up in memory.
2. **Unintended background execution and errors:** The listener continues firing on every window resize or keypress even when the user is on a completely different page.

To prevent this, always return a cleanup function from `useEffect` that removes the exact same listener reference: `return () => window.removeEventListener("resize", handleResize);`.

---

### Q51. Suppose an API returns a student name containing HTML markup or a `<script>` tag. How would you display it safely?

**Answer:**

I would render it using standard JSX curly braces (`<p>{student.name}</p>`). By default, React automatically **escapes all strings rendered inside JSX `{}`** before inserting them into the DOM, converting characters like `<` and `>` into safe text entities so a malicious `<script>` or `<img onerror=...>` tag is displayed harmlessly as literal text rather than executed as HTML.

I would never pass untrusted user data into `dangerouslySetInnerHTML`. In the rare case where rich HTML formatting from an external source must be rendered, I would sanitize the HTML string first using a battle-tested library like **DOMPurify** (`DOMPurify.sanitize(rawHtml)`), and I would also validate any user-supplied `<a href={url}>` links to ensure they start with `https://` or `http://` rather than `javascript:`.

---

### Q52. Suppose the frontend hides the Delete button from students, but a student sends a `DELETE` request directly to the backend. How would you prevent the operation?

**Answer:**

Hiding a button in React (`{user.role === "admin" && <DeleteButton />}`) is purely a **user-interface convenience**, never a security control. Any user can open Browser DevTools, copy their session token, and run a `fetch(..., { method: "DELETE" })` or `curl` command directly against the backend URL.

To prevent unauthorized deletion, the **backend API must enforce authentication and role-based authorization on every single request**:
1. Verify the caller's session cookie or JWT signature.
2. Look up the authenticated caller's role and permissions on the server.
3. Verify that `user.role === "admin"` (and check resource ownership if applicable).
4. Reject any student request immediately with `403 Forbidden` before touching the database.

---

### Q53. Suppose a user's session expires while they are filling out a screen in the application. How should React handle it?

**Answer:**

I would handle session expiration centrally inside the **HTTP client response interceptor** rather than scattering `401` checks across individual components:

1. **Silent Token Refresh First:** When an API call returns `401 Unauthorized`, the interceptor pauses the failed request and calls the `/api/auth/refresh` endpoint once (queuing any concurrent requests that also failed at the same moment so only one refresh call is made). If the refresh succeeds, the interceptor automatically retries the original request transparently without interrupting the user!
2. **Graceful Redirect if Refresh Fails:** If the refresh token itself is expired or revoked, the application saves any unsaved form draft to `sessionStorage` if appropriate, clears sensitive in-memory auth state, displays a clear toast notification (*"Your session has expired. Please sign in again."*), and redirects to `/login?redirect=/current-path` so the user can resume their work right after logging back in.

---

### Q54. Suppose the frontend sends an API request twice after a network timeout. How would you prevent duplicate operations?

**Answer:**

A client-side network timeout is **ambiguous**: the browser did not receive a response within the deadline, which could mean the request never reached the server, OR the server processed the request and saved the record, but the response was lost on the way back.

To safely retry without creating duplicate records or double allocations:
1. **Generate a client-side `Idempotency-Key` (UUID):** When the user initiates the logical action (such as opening or submitting the form), generate a unique UUID and send it in the `Idempotency-Key` HTTP header.
2. **Reuse the exact same `Idempotency-Key` on retry:** If the request times out and the user (or automatic retry logic) resends the request, it sends the **exact same `Idempotency-Key`**.
3. **Backend deduplication:** The backend stores the idempotency key alongside the operation result in a unique database table; when it sees the repeated key, it returns the already-completed result instead of executing the insert a second time.

---

### Q55. Suppose you need to test whether a React login form works correctly. What would you test?

**Answer:**

Using **React Testing Library** with **Vitest** and `@testing-library/user-event`, I would write behavior-driven tests that interact with the form the same way a real user does (querying elements by accessible role and label via `screen.getByRole` and `screen.getByLabelText`):

1. **Initial render:** Verify that the Email input, Password input, and Sign In button render and are accessible by label/role.
2. **Client-side validation:** Click Sign In with empty fields or an invalid email and assert that accessible validation error messages appear and the API was *not* called.
3. **Successful login flow:** Type a valid email and password using `await user.type(...)`, click Sign In, assert that the button disables and shows `"Signing in..."` while in flight, and verify that on a mocked `200 OK` response (via MSW — Mock Service Worker) the user is redirected to the dashboard.
4. **Invalid credentials (`401` error):** Mock a `401` response from the server and verify that an alert banner (`role="alert"`) displays `"Invalid email or password"` while keeping the email input populated so the user does not have to retype it.

---

### Q56. Suppose a component works in the browser but fails in an automated test. How would you debug it?

**Answer:**

When a component works in the browser but fails in Vitest/Jest + React Testing Library, the difference almost always lies in the **simulated test environment (`jsdom`) or asynchronous test timing**:

1. **Synchronous assertion running before async state updates finish:** If the component fetches data or updates state asynchronously, using synchronous `screen.getByText(...)` immediately after `render()` fails because the data hasn't arrived yet. Replace `getBy...` with `await screen.findByText(...)` or `await waitFor(...)`.
2. **Missing Context Providers in test render:** In the real app, the component is wrapped in `<BrowserRouter>`, `<AuthProvider>`, and `<QueryClientProvider>`. In a unit test, rendering the component bare without a custom `renderWithProviders()` wrapper causes it to fail when calling `useNavigate()` or `useAuth()`.
3. **Missing browser APIs in `jsdom`:** `jsdom` does not implement browser layout or certain web APIs like `window.matchMedia`, `IntersectionObserver`, `ResizeObserver`, or `window.scrollTo`. These must be stubbed in `setupTests.ts`.
4. **Call `screen.debug()`:** Run `screen.debug()` inside the failing test to print the exact HTML currently rendered in `jsdom` at the moment of failure.

---

# Part 8: Deep Project-Based Interview Questions (Q57–Q63)

## Section 8.1: Your Personal Contribution

### Q57. What was your role in the project?

**Answer:**

"In the SeatSync project, my primary responsibility was designing and developing the React frontend modules and integrating them end-to-end with our backend REST APIs. Specifically, I built reusable UI components such as the generic `DataTable`, filter bars, and confirmation modals, and I implemented the student directory, examination schedule management, and room seating allocation views.

I handled the complete data lifecycle on the frontend—including controlled forms with validation, debounced search, pagination, role-based route protection across Admin, Invigilator, and Student portals, and comprehensive loading, empty, and error states. Whenever issues came up during integration, I debugged them using React DevTools, browser network inspection, and backend logs to ensure smooth communication between the UI and the database."

---

### Q58. Which React concepts did you use in your project, and why?

**Answer:**

"I chose React features selectively based on the specific problem each screen needed to solve:
- **Functional Components and Props:** Used to break complex screens like the Seating Dashboard into small, focused, reusable pieces such as summary cards, filter controls, and data tables.
- **`useState` and `useReducer`:** Used `useState` for local component states like search inputs, modal visibility, and current page numbers, and `useReducer` when managing multi-step workflows where several state values needed to transition together predictably.
- **`useEffect` with Cleanup (`AbortController`):** Used to synchronize components with our backend APIs when route parameters or filter dependencies changed, while aborting stale in-flight requests to prevent race conditions.
- **Custom Hooks (`useDebounce`, `useAuth`):** Extracted shared stateful logic so multiple tables could debounce search inputs and read authentication context cleanly without duplicating code.
- **React Context:** Used for low-frequency global state like the authenticated user's session and role so we didn't have to prop-drill user data through every layout wrapper.
- **`useMemo` and `useCallback`:** Applied selectively where filtering or sorting large student lists in memory needed to avoid redundant recalculations."

**Follow-up: Did you use every Hook in React?**
"No. I only used Hooks that solved a concrete requirement in our application. Using unnecessary Hooks—like wrapping every trivial variable in `useMemo`—makes code harder to read without improving real performance."

---

## Section 8.2: Component Communication & Backend Integration

### Q59. How would you implement a parent component that passes student data to a child table, while the child communicates row selection back to the parent?

**Answer:**

I would follow React's **unidirectional data flow** pattern ("data flows down, events flow up"):
- The parent component (`StudentPage`) owns both the `students` array and the `selectedStudent` state (`useState<Student | null>(null)`).
- The parent passes the `students` array down to `<StudentTable>` as a data prop, and also passes the state setter (or a handler function `handleSelectStudent`) down as an `onSelect` callback prop.
- Inside `<StudentTable>`, when the user clicks a student row or its "Select" button, the child invokes `onSelect(student)` with that row's student object.
- That callback updates `selectedStudent` in the parent, causing the parent to re-render and display the selected student's details in a side panel or modal.

---

### Q60. How would you share a selected examination between unrelated components?

**Answer:**

I would choose the sharing mechanism based on the scope and persistence requirements of that selection:

1. **URL State (`useParams` or `useSearchParams` — Often the Best Choice!):**
   If selecting an examination should survive a browser page refresh (F5) and allow an admin to copy and share the URL with a colleague (for example, `/admin/seating?examId=EX205`), storing the selected `examId` in the URL query string or route parameter lets any component on the page read `useSearchParams()` directly!
2. **Lifting State to a Common Page Component:**
   If the two components live on the same screen (such as an `ExamSelectorDropdown` and an `ExamRoomAllocationTable`), lift `selectedExamId` into their parent page component and pass it down via props.
3. **React Context or Zustand Store:**
   If the selected examination acts as a global workspace selector across multiple separate pages in the application, store `selectedExamId` in an `ExamWorkspaceContext` (or Zustand store) so any page can access it without prop drilling.

---

### Q61. How would you explain the relationship between React, Node.js, and a database in your project?

**Answer:**

Each layer in the stack has a distinct responsibility and security boundary:
- **React (Presentation & Interaction Layer):** Runs in the user's browser. It renders the interface, captures user input, performs immediate UX validation, manages client-side navigation and view state, and sends HTTP requests. React is an untrusted client environment and never holds database credentials.
- **Node.js / Express (Application & Business Logic Layer):** Runs on the server. It exposes REST API endpoints, verifies authentication tokens, enforces role-based access control, validates all incoming payloads against business rules (such as verifying room capacity before allocating seats), and coordinates transactions.
- **Database Layer (Persistence & Integrity Layer):** Stores the authoritative records for students, exams, and seat assignments, enforces relational integrity constraints (foreign keys, unique constraints, ACID transactions), and uses indexes for fast retrieval.

---

### Q62. What happens if React sends an invalid examination ID to the backend?

**Answer:**

If the React frontend sends a malformed or non-existent `examId` (for instance, because a user edited the URL manually or another admin deleted that examination a moment earlier):
1. The backend validates the ID format first (returning `400 Bad Request` if malformed) and queries the database (returning `404 Not Found` if the examination no longer exists).
2. The React API service checks `response.ok`, detects the `400` or `404` status, and throws a structured error containing the status code and message.
3. The React component catches that error, clears any loading spinner, avoids crashing or rendering broken state, and displays a clear recovery UI—such as *"This examination could not be found or may have been deleted"* with a button to return to the Examinations list.

---

### Q63. How would you refresh the examination list after creating a new examination?

**Answer:**

There are three clean ways to update the displayed examination list after a `POST /api/exams` request succeeds, depending on how data fetching is architected:

1. **Cache Invalidation with TanStack Query (Preferred in production):**
   In the `onSuccess` callback of `useMutation`, call `queryClient.invalidateQueries({ queryKey: ["exams"] })`. TanStack Query automatically marks the exam list stale and refetches the fresh, server-sorted list in the background.
2. **Refetching via a shared fetch function:**
   If using plain `useEffect` and `useState`, extract `loadExams()` into a reusable function (`useCallback`) and call `await loadExams()` right after the `POST` request succeeds. This guarantees the UI reflects server-side sorting, pagination, and generated fields.
3. **Appending the returned record to local state:**
   If the list is small and unpaginated and the `POST` endpoint returns the newly created `Exam` object (including its database-generated `id`), call `setExams(prev => [createdExam, ...prev])` to update the UI immediately without an extra network round-trip.

---

# Part 9: Complete Answers to Scenario Questions Q64–Q100 (Sentence-Based Explanations)

## Section A: State and Rendering Scenarios (Q64–Q70)

### Q64. A component's state changes, but the UI does not update as expected. How would you debug it?

**Answer:**

First, I would check whether the state was **mutated in place** rather than replaced with a new object or array reference. React compares the previous state and next state using `Object.is(prevState, nextState)`; if you push an item into an existing array (`items.push(newItem)`) or mutate a property on an existing object (`user.name = "Anu"`) and pass that same reference back to `setState(items)`, React sees that the memory reference did not change and completely skips re-rendering! Creating a new reference via spread syntax (`setItems(prev => [...prev, newItem])`) or `.map()` fixes this.

Second, I would check whether a child component copied a prop into its own local `useState(propValue)` on mount and is ignoring subsequent prop updates from the parent. Third, I would verify in React DevTools whether the component calling `setState` is actually the same instance rendered on screen.

---

### Q65. Two state variables must always change together. How would you manage them?

**Answer:**

When two or more state values must always update together to maintain a valid UI invariant—for example, `selectedDepartment` and `selectedCourse` (where changing the department must immediately reset the course), or `loading`, `data`, and `error`—keeping them in separate `useState` hooks makes it easy for a developer to update one and forget to reset the other.

Instead, I would either **combine them into a single state object** (`useState({ department: "CSE", course: "" })`) or manage them with **`useReducer`**, where a single dispatched action (`{ type: "SELECT_DEPARTMENT", department: "ECE" }`) updates both fields atomically inside one pure reducer transition. And if one value can be calculated directly from the other during render, I would make it a **derived constant** rather than storing a second state variable at all.

---

### Q66. A user changes a filter, but the old filter is still applied to the results. What could be wrong?

**Answer:**

This usually happens for one of three reasons:
1. **Stale state read in the same event handler:** The developer called `setFilter(newValue)` and on the very next line called `fetchResults(filter)` using the state variable `filter` (which still holds the old render snapshot!) instead of passing `fetchResults(newValue)` or triggering the fetch via `useEffect` / `useQuery` watching `[filter]`.
2. **Missing dependency in `useMemo` or `useEffect`:** The filtering calculation inside `useMemo` or the API call inside `useEffect` forgot to include `filter` in its dependency array, so it continues returning results computed with the initial filter closure.
3. **Uncontrolled vs Controlled mismatch:** The filter dropdown updates its visual DOM state, but its `onChange` handler is not wired to update the state variable that actually drives the query.

---

### Q67. A modal closes and reopens with its old form values still inside. How would you reset it?

**Answer:**

This happens when a `<Modal isOpen={isOpen}>` component stays mounted in the React tree when hidden (for example, hiding itself via CSS `display: none` or keeping the parent form state alive while `isOpen` toggles). Because the component never unmounted, its internal `useState` form fields retain whatever the user typed previously.

There are three clean ways to fix this:
1. **Conditional mounting:** Render `{isOpen && <StudentModal onClose={...} />}` so unmounting the modal when `isOpen` becomes `false` automatically destroys its internal state, and reopening mounts a fresh state.
2. **Pass a `key` prop:** Pass `<StudentModal key={selectedStudentId ?? "new"} isOpen={isOpen} />` or increment a reset key when opening so React creates a brand-new component instance.
3. **Explicit reset on close/cancel:** Call `resetForm()` inside the modal's close handler before invoking `onClose()`.

---

### Q68. You want a component to preserve its state (like a half-typed form or scroll position) when switching between tabs. How would you design it?

**Answer:**

When you switch tabs using standard conditional rendering (`{activeTab === "profile" ? <ProfileTab /> : <SettingsTab />}`), React unmounts `<ProfileTab />` when the user switches to `"settings"`, wiping out its internal state.

To preserve state across tab switches, you have three options:
1. **Lift the form state up** into the parent `TabsContainer` component (or a Context/Zustand store) so the state lives above the conditional tab unmount boundary and is passed back down via props when the user returns to the tab.
2. **Keep both tab panels mounted and hide inactive tabs with CSS (`hidden` attribute or `display: none`):** This keeps the React Fiber and DOM nodes alive in memory (ideal when there are only a few lightweight tabs).
3. **Persist draft state in `sessionStorage` or URL state:** Sync form drafts to `sessionStorage` via a custom Hook so the state survives both tab switches and full browser refreshes.

---

### Q69. A child component resets its state unexpectedly when the list order changes or when the parent re-renders. Why?

**Answer:**

A child component loses its state during a parent re-render for one of two major reasons:
1. **Defining a component function inside another component function (Critical Anti-Pattern!):** If a developer writes `function Row() { ... }` *inside* the body of `function StudentTable()`, every time `StudentTable` re-renders, JavaScript creates a brand-new `Row` function reference in memory. During reconciliation, React sees `prevType !== nextType`, concludes it is a completely different component type, and **unmounts and remounts `Row` from scratch on every single render**, destroying its state and input focus! Always define components at the module top level.
2. **Unstable `key` prop:** Passing `key={Math.random()}` or `key={index}` when sorting causes React to mismatch or recreate component instances whenever the list order changes.

---

### Q70. Multiple state updates depend on the previous state. How should you implement them?

**Answer:**

Whenever the next state value is calculated from the previous state value—such as incrementing a counter, toggling a boolean (`setIsOpen(prev => !prev)`), or appending/filtering an array (`setItems(prev => [...prev, newItem])`)—you should always pass a **functional updater (`prev => next`)** to the state setter, or consolidate complex multi-step transitions into **`useReducer`**. Functional updates guarantee that even if multiple updates are batched together in the same event loop tick or called inside an asynchronous callback, each update receives the most recent pending state rather than a stale render snapshot.

---

## Section B: Hooks and Effects Scenarios (Q71–Q76)

### Q71. An Effect causes an infinite render loop. How would you investigate it?

**Answer:**

I would look at two things inside the `useEffect` block: **what state it sets** and **what values are in its dependency array**.
- First, check if the dependency array was accidentally omitted (`useEffect(() => { ... })`), which makes it run after every render.
- Second, check if the Effect calls `setState` unconditionally on a state variable that is also inside its dependency array.
- Third—and most commonly in real projects—check if the dependency array contains an **object, array, or function created inline during render** (such as `const config = { page, limit }` or `function fetchData() {}`). Because inline objects and functions get a new memory address on every render, React sees the dependency as changed on every render, runs the Effect, updates state, triggers a render, creates another new object reference, and loops infinitely. Moving the object/function inside the Effect or stabilizing it with `useMemo`/`useCallback` breaks the loop immediately.

---

### Q72. A timer or WebSocket subscription continues running after the user leaves the page. How would you fix it?

**Answer:**

This happens when a `setInterval`, `setTimeout`, or WebSocket subscription is started inside a component without returning a **cleanup function** from `useEffect`. When the user navigates to another route, the component unmounts from the screen, but the browser timer or socket remains active in the background.

To fix this, always store the timer ID or subscription instance inside the `useEffect` (or a `useRef`) and return a cleanup function (`return () => { clearInterval(timerId); socket.close(); }`) from the `useEffect`. React guarantees that this cleanup function executes automatically the moment the component unmounts.

---

### Q73. A custom Hook is used in five components. What should its responsibilities be, and how should you design its API?

**Answer:**

A well-designed custom Hook should follow the **Single Responsibility Principle**: it should encapsulate **one cohesive piece of stateful logic or external synchronization** (such as `useDebounce`, `useWindowSize`, `useLocalStorage`, or `usePagination`) without baking in component-specific JSX or unrelated business side effects.

When designing its API for five consumers:
- Keep its inputs simple (primitives or an options object) and handle edge cases internally (including cleanup on unmount).
- Return either a **tuple `[value, setValue]`** when the Hook exposes two primary items that callers often rename (like `useState`), or return a **named object `{ data, isLoading, error, refetch }`** when exposing three or more properties so callers can destructure only the fields they need.
- Remember that each of the five components gets its **own isolated state instance**; if all five components need to share the *exact same live value*, the custom Hook should read from a shared Context or external store.

---

### Q74. An Effect fetches the wrong record after a route parameter (like `/students/:studentId`) changes rapidly. Why?

**Answer:**

This occurs either because `studentId` was omitted from the Effect's dependency array (so navigating from `/students/101` to `/students/102` keeps showing Student 101 because the component stays mounted and reuses the same route layout), OR because of an **out-of-order network race condition** where the request for Student 101 was slow, the user clicked Student 102 which loaded fast, and then the slow response for Student 101 arrived last and overwrote the state.

Including `[studentId]` in the `useEffect` dependency array and aborting the previous request in the Effect cleanup function using `AbortController` (`return () => controller.abort()`) solves both issues completely.

---

### Q75. You need to measure a DOM element's height or position (such as a tooltip or popover) before the browser paints so it does not visibly jump on screen. Which Hook is appropriate?

**Answer:**

I would use **`useLayoutEffect`** combined with a **`useRef`** attached to the DOM element.

If you measure a tooltip's DOM dimensions using `getBoundingClientRect()` inside a standard `useEffect` and then call `setPosition(...)` to flip it above the button so it doesn't overflow the screen edge, the user will see a brief **visual flicker**: the browser paints the tooltip in the wrong clipped position first, and then `useEffect` runs and jumps it to the new position.

`useLayoutEffect` runs **synchronously right after React commits the DOM node, before the browser paints the screen**. Measuring the ref and updating position state inside `useLayoutEffect` lets React process the second render synchronously before the user ever sees the first frame, eliminating the visual jump completely.

---

### Q76. A child component needs to expose a limited imperative method (like `focusInput()` or `resetScroll()`) to its parent without exposing the entire raw DOM node. How could you do this?

**Answer:**

I would use **`useImperativeHandle`** inside the child component paired with a `ref` passed from the parent (via `forwardRef` in React 18 or a direct `ref` prop in React 19).

Inside the child component, I attach an internal `useRef` to the actual `<input>` or scrollable `<div>`, and then call `useImperativeHandle(parentRef, () => ({ focusInput: () => internalRef.current?.focus(), resetScroll: () => { internalRef.current.scrollTop = 0; } }), [])`. This keeps the child's internal DOM structure encapsulated while giving the parent a clean, type-safe imperative handle for the few actions that cannot be expressed declaratively through props.

---

## Section C: Data and API Scenarios (Q77–Q83)

### Q77. An API returns duplicate records in a list. How would you handle them in React?

**Answer:**

First, I would report the bug to the backend team because duplicate records in an API response usually indicate an un-deduplicated SQL `JOIN` or unstable `OFFSET` pagination when rows are inserted concurrently.

On the React side, to prevent duplicate `key` warnings and avoid displaying duplicate rows to the user, I would **deduplicate the array by unique ID inside `useMemo` before rendering**—for example, using a `Map` keyed by `record.id` (`Array.from(new Map(records.map(r => [r.id, r])).values())`) or tracking seen IDs in a `Set` via `.filter()`. This preserves insertion order in `O(n)` linear time and guarantees every rendered item has a unique `key`.

---

### Q78. A search request takes too long and feels sluggish as the user types. What could you optimize?

**Answer:**

I would optimize both the **frontend request behavior** and the **backend query**:
1. **Decouple input state from search query via Debouncing (`useDebounce`, 250–300ms):** Keep the `<input>` controlled by an immediate state variable so typing never lags, and only trigger the API call after the user pauses typing for 300ms.
2. **Cancel obsolete in-flight requests (`AbortController`):** Immediately abort the previous search request when a new one starts so slow queries don't waste browser connections or overwrite newer results.
3. **Cache recent searches (TanStack Query):** If the user types `"Anu"`, adds a letter `"Anup"`, and hits Backspace back to `"Anu"`, serve the cached results for `"Anu"` instantly!
4. **Enforce a minimum character threshold & pagination:** Only search once the user types 2+ characters, return only the top 20 matches (`LIMIT 20`), and ensure the backend database has a proper trigram/full-text or prefix index on the searched columns.

---

### Q79. A user submits a long form while the server or network is temporarily unavailable. What should happen?

**Answer:**

The golden rule of form error handling is **never lose the user's typed work**.
1. When the `POST` request fails due to a network error or `503 Service Unavailable`, catch the error, keep the form mounted with **all field values intact**, re-enable the Submit button, and display a clear error banner with a **"Try Again"** button.
2. For long, multi-minute forms (like creating an exam schedule or entering 50 student marks), continuously **auto-save form drafts to `localStorage` or `IndexedDB`** as the user types. That way, even if their Wi-Fi drops and they accidentally close or refresh the browser tab, reopening the page restores their draft automatically.
3. You can also listen to `window.addEventListener("online", ...)` to notify the user as soon as their internet connection is restored.

---

### Q80. The backend API response structure changes (for example, `student_name` is renamed to `fullName`). How would you minimize frontend breakage?

**Answer:**

If raw API response objects are passed directly into 30 different React components, renaming a single backend JSON field forces you to edit all 30 components.

To prevent this, I use an **Adapter / Data Mapping Layer (Anti-Corruption Layer)** inside the API service module (`studentService.ts`) combined with runtime schema validation (such as **Zod**):
- The API service receives the raw backend DTO, validates it with Zod (so contract mismatches fail loudly and clearly in tests/staging rather than causing mysterious `undefined` crashes deep in the UI), and maps it into the frontend's clean domain type (`Student`).
- If the backend renames `student_name` to `fullName`, I only update **one line in the mapper function** inside `studentService.ts`, and all 30 React components continue reading `student.name` without a single change!

---

### Q81. A mutation (such as editing a student's room assignment) succeeds on the backend, but the displayed list on screen remains stale. How would you fix it?

**Answer:**

This happens when the mutation request completes on the server, but the frontend state or query cache holding the list is never notified to update.

- If using **TanStack Query**, call `queryClient.invalidateQueries({ queryKey: ["students"] })` inside the mutation's `onSuccess` handler so the list automatically refetches fresh data, or update the cached item directly via `queryClient.setQueryData` using the updated student object returned by the `PUT`/`PATCH` response.
- If using **local `useState`**, either replace the modified item immutably inside the state array (`setStudents(prev => prev.map(s => s.id === updated.id ? updated : s))`) or re-invoke the `fetchStudents()` function after `await updateStudent()` resolves.

---

### Q82. Several independent API requests (such as fetching exam details, room capacities, and available invigilators) must complete before a page can display its allocation form. How would you coordinate them?

**Answer:**

Avoid firing them sequentially one after another (`await getExam(); await getRooms(); await getInvigilators();`), which creates a **network waterfall** where total wait time is the *sum* of all three requests!

Instead, fire all three requests **in parallel** using **`Promise.all([getExam(signal), getRooms(signal), getInvigilators(signal)])`** inside `useEffect` (or `useSuspenseQueries` in TanStack Query). With parallel fetching, all three requests travel over the network simultaneously, so the page loads in the time of the single slowest request rather than the sum of all three.

---

### Q83. One secondary request (such as "Recent Activity Logs") fails on a dashboard while the critical requests ("Upcoming Exams" and "Summary Stats") succeed. Should the whole page fail?

**Answer:**

No! A failure in a non-critical secondary widget should **never** take down the entire dashboard (**Graceful Degradation / Partial Rendering**).

Instead of using `Promise.all` (which rejects immediately if *any* single promise fails), I would either:
1. Fetch each dashboard section independently inside its own isolated component wrapped in its own local `<ErrorBoundary>` and `<Suspense>` boundary, OR
2. Coordinate them using **`Promise.allSettled([getStats(), getExams(), getActivityLogs()])`**, which waits for all promises to finish and returns `{ status: "fulfilled", value }` or `{ status: "rejected", reason }` for each one individually. That way, the main stats and exams render normally while the Recent Activity card alone shows a small local *"Unable to load activity logs — Retry"* message.

---

## Section D: Performance Scenarios (Q84–Q89)

### Q84. A large table freezes for 2 seconds whenever the user clicks a column header to sort. What would you investigate?

**Answer:**

When clicking a sort header freezes the UI for 2 seconds, the bottleneck is almost always a combination of **DOM thrashing** (re-ordering and re-rendering thousands of heavy DOM nodes at once) and **synchronous main-thread computation**:
1. Check how many rows are mounted in the DOM. If 5,000+ rows are rendered without pagination or virtualization, sorting forces React to reconcile and reorder 5,000 complex row subtrees and forces the browser to recalculate layout for tens of thousands of `<td>` cells. Adding **pagination (25–50 rows)** or **list virtualization** eliminates 95% of this work immediately.
2. Wrap the state update that changes the sort order inside React 18's **`startTransition(() => setSortBy(column))`** so the header click state updates immediately without freezing the browser if the user clicks again.
3. If client-side sorting itself is heavy (e.g., complex multi-column locale string comparisons across 50,000 items), move the sorting to the backend database (`ORDER BY`) or a background Web Worker.

---

### Q85. The initial application load is very slow on mobile networks. What could improve it?

**Answer:**

I would inspect the production build output using `rollup-plugin-visualizer` (Vite bundle analyzer) and Chrome Lighthouse, then apply four high-impact optimizations:
1. **Route-level and Heavy-Component Code Splitting (`React.lazy` + `Suspense`):** Split Admin routes, heavy chart libraries (`recharts`/`chart.js`), and Excel/PDF exporters into separate async chunks so the initial login/dashboard bundle is under ~100–150 KB gzipped.
2. **Tree-shaking & Dependency Audit:** Replace heavy non-tree-shakeable libraries (like full `moment.js` or `lodash`) with lightweight modern alternatives (`date-fns`/`Intl` or native JS methods).
3. **Asset & Image Optimization:** Convert large PNG/JPEG banners to **WebP/AVIF**, specify `width` and `height`, and add `loading="lazy"` to below-the-fold images.
4. **Compression & CDN Caching:** Enable **Brotli (`br`) or Gzip** compression on Nginx/CDN and serve hashed static assets with `Cache-Control: public, max-age=31536000, immutable`.

---

### Q86. A component renders frequently in React DevTools, but takes only 0.2ms per render and feels completely smooth. Should you optimize it with `React.memo` and `useCallback`?

**Answer:**

**No.** Re-rendering a fast, lightweight component in React is normal and extremely cheap because Virtual DOM diffing of a few elements takes microseconds.

Wrapping every trivial component in `React.memo` and every prop in `useMemo` and `useCallback` is **premature optimization**: it adds memory overhead to store dependency arrays, runs prop comparison checks on every render anyway, and clutters the codebase with boilerplate that makes it harder to read and maintain. You should only apply memoization when profiling shows a measurable render cost or when stabilizing a reference passed to an Effect dependency array or an actually expensive subtree.

---

### Q87. A large enterprise form has 200 input fields, and typing into any single field feels laggy. How would you improve responsiveness?

**Answer:**

The lag happens because storing all 200 field values in a single top-level `useState` object inside the parent `<Form>` component forces **all 200 `<Input>` components to re-render on every single keystroke**.

To make typing instantaneous (`< 16ms` per frame):
1. **Use React Hook Form (Uncontrolled Subscription Architecture):** React Hook Form registers inputs via refs and isolates state subscriptions so typing into Field #1 triggers **zero re-renders** in Fields #2 through #200!
2. **Or Isolate State / Memoize Field Sub-Components:** If using controlled state, break the 200 fields into logical field-group sections that colocate their own state, or wrap individual `<FormField>` components in `React.memo` with a stable `useCallback` change handler so only the single field currently being edited re-renders on each keystroke.

---

### Q88. Images on a student directory page make the page load slowly and jump around as they pop in. How would you improve performance and visual stability?

**Answer:**

1. **Eliminate Layout Shifts (CLS):** Always include explicit `width` and `height` attributes (or CSS `aspect-ratio: 1 / 1`) on every `<img>` tag so the browser reserves the exact pixel box before the image downloads, preventing surrounding table rows from jumping.
2. **Lazy Load Below-the-Fold Images:** Add `loading="lazy"` and `decoding="async"` to student avatars so the browser only downloads images as they scroll near the viewport, while keeping `fetchpriority="high"` (and no lazy loading) only on the top visible hero image.
3. **Serve Appropriately Sized Thumbnails:** Never serve a 4000×4000px 5 MB camera upload inside a 48×48px avatar circle! Resize and compress avatars to 96×96px WebP thumbnails on the backend/CDN and use responsive `srcset` attributes.

---

### Q89. A memoized child component receives a configuration object prop (`options={{ showHeader: true, compact: false }}`) and still re-renders on every parent render. What would you consider?

**Answer:**

Because `{ showHeader: true, compact: false }` is an inline object literal written inside the parent's JSX, JavaScript allocates a **new object reference on the heap on every parent render**. When `React.memo` checks `Object.is(prevProps.options, nextProps.options)`, it evaluates to `false`, defeating `React.memo` completely!

I would consider three solutions in order of simplicity:
1. **Pass primitive props instead of an object:** Change `<Child options={{ showHeader: true, compact: false }} />` to `<Child showHeader={true} compact={false} />`. Primitives (`true === true`) compare equal by value automatically without needing `useMemo`!
2. **Hoist static objects outside the component:** If `options` never depends on props or state, declare `const DEFAULT_OPTIONS = { showHeader: true, compact: false };` at the module top level outside the component function so its reference never changes.
3. **Wrap dynamic objects in `useMemo`:** If the object depends on dynamic state, wrap it in `const options = useMemo(() => ({ showHeader, compact }), [showHeader, compact])`.

---

## Section E: Routing and Authentication Scenarios (Q90–Q94)

### Q90. The user logs in, is redirected to `/dashboard`, presses the browser Back button, and lands back on the `/login` page even though they are already logged in. How would you fix this?

**Answer:**

This happens for two reasons that should both be fixed:
1. **Using `push` instead of `replace` after login:** When redirecting the user after a successful login, call `navigate("/dashboard", { replace: true })` (or `<Navigate to="/dashboard" replace />`). Using `replace: true` replaces the `/login` entry in the browser's history stack instead of pushing on top of it, so pressing the Back button does not step back onto `/login`.
2. **Adding a `PublicOnlyRoute` (Guest Guard) on `/login`:** Wrap the `/login` route in a guard that checks if `user` is already authenticated; if an already-logged-in user ever visits `/login` (via Back button or bookmark), immediately redirect them to their dashboard with `<Navigate to="/dashboard" replace />`.

---

### Q91. A nested route (like `/admin/exams/create`) URL changes in the address bar, but the child component never appears inside `AdminLayout` (or replaces the entire layout). What would you check?

**Answer:**

1. **If the child route never appears inside `AdminLayout`:** Check whether `AdminLayout` forgot to render React Router's **`<Outlet />`** component! In React Router, a parent layout route only renders its matched child route where `<Outlet />` is placed inside the parent's JSX.
2. **If the child route replaces the entire screen instead of rendering inside `AdminLayout`:** Check whether the `<Route path="exams/create" />` was accidentally declared as a sibling route outside `<Route path="/admin" element={<AdminLayout />}>` instead of nested inside it.
3. **Check relative vs absolute paths and wildcard `*`:** If using nested `<Routes>` inside a sub-component rather than a centralized route tree, ensure the parent route path ends with `/*` (`/admin/*`).

---

### Q92. A user copies a deep link (`/admin/exams/EX104/seating`) and pastes it into a new browser tab. What must work across the stack for that link to load properly?

**Answer:**

Four layers must work together seamlessly for a deep link to load in a fresh tab:
1. **Web Server SPA Fallback (`Nginx` / CDN):** The hosting server must return `/index.html` with HTTP `200` for `/admin/exams/EX104/seating` instead of a server-side `404`.
2. **Absolute Asset Paths in `index.html`:** The `<script src="/assets/...">` tags in `index.html` must use root-relative (`/assets/...`) paths (`base: "/"` in Vite), not relative `./assets/...` paths (which would wrongly request `/admin/exams/EX104/assets/...`).
3. **Async Auth Initialization Wait:** `AuthProvider` must restore the user's session before `ProtectedRoute` decides whether to redirect to `/login`; if the user is not logged in, `/login` should save `state={{ from: location }}` so after signing in they land directly on `/admin/exams/EX104/seating`.
4. **Independent Data Fetching by Route Param:** The `SeatingPage` component must read `const { examId } = useParams()` and fetch Exam `EX104` from the API itself, rather than assuming the exam object was already loaded into memory by a previous page!

---

### Q93. An administrator revokes an invigilator's role or changes a user's permissions while the user already has the application open. How should the React UI respond?

**Answer:**

Because client-side role state in React is only a cached snapshot of what the user's permissions were when they logged in:
1. **Immediate Backend Enforcement (`403 Forbidden`):** The very next API request the user makes to a restricted endpoint will be rejected by the backend with `403 Forbidden` (or `401` if their token/session was invalidated).
2. **Global API Interceptor Sync:** When the React HTTP interceptor receives a `403` due to stale role permissions (or when the user switches browser tabs back to the app and triggers a session revalidation check on `visibilitychange`), the app automatically refetches `GET /api/auth/me` to update the `UserContext` role, clears stale query caches (`queryClient.clear()`), notifies the user that their permissions were updated, and redirects them to their currently permitted dashboard.

---

### Q94. The same `/api/students` endpoint is called by three different components mounted on the same dashboard page at the same time. How would you avoid firing three identical network requests?

**Answer:**

If three mounted components (`StudentCountCard`, `RecentStudentsWidget`, and `AttendanceQuickFilter`) each run their own raw `useEffect(() => fetch("/api/students"), [])`, the browser will fire **three duplicate HTTP requests** simultaneously.

To eliminate duplicate network calls:
- **Use TanStack Query (`useQuery({ queryKey: ["students"], queryFn: getStudents })`):** When three components mount simultaneously and request the same `["students"]` query key, TanStack Query **automatically deduplicates** them into **one single HTTP request** and shares the resolved result with all three components!
- Alternatively, if not using a server-state library, **lift the data fetch** up to the parent `Dashboard` page (or an in-flight Promise cache in the service layer) and pass the data down to the three widgets via props.

---

## Section F: Testing, Deployment, and Production Debugging Scenarios (Q95–Q100)

### Q95. An automated test passes every time on your local laptop, but fails intermittently in the GitHub Actions CI pipeline. How would you debug it?

**Answer:**

Flaky or CI-only test failures are typically caused by **slower CPU execution in CI containers, timezone/locale differences, or shared state leakage between tests**:
1. **Race conditions and fixed timeouts:** A test waiting `setTimeout(..., 100)` or using a short `waitFor` timeout might pass on a fast M3/i9 laptop where rendering takes 15ms, but fail on a shared 2-core CI runner where it takes 150ms. Replace fixed sleeps with proper `await screen.findByRole(...)` queries.
2. **Timezone and Locale differences:** Your laptop may run in `Asia/Kolkata (IST, UTC+5:30)` while the Linux CI runner runs in `UTC`. Any test asserting on formatted dates (`toLocaleDateString()`) without fixing `TZ=UTC` in the test script will fail in CI!
3. **Case sensitivity on Linux CI:** Check for imports with mismatched casing that pass on Windows/macOS but fail on Linux CI.
4. **Test isolation leaks:** Ensure `afterEach(() => { cleanup(); vi.clearAllMocks(); localStorage.clear(); })` runs so one test's mocked timers or cached state does not pollute the next test.

---

### Q96. The production build (`npm run build`) succeeds with zero errors, but opening the deployed website shows a completely blank white screen. What would you inspect?

**Answer:**

I would open **Browser DevTools (Console and Network tabs)** on the deployed site immediately:
1. **Check the Console for an uncaught JavaScript error during initial render:** For example, `Cannot read properties of undefined` caused by reading a missing production environment variable or parsing unexpected data without an `<ErrorBoundary>`.
2. **Check the Network tab for `404` on `/assets/index-xxx.js`:** If the React app is deployed in a subdirectory (like `https://university.edu/seatsync/`) instead of the root domain `/`, but Vite's `base` config was left as `"/"`, the browser requests `https://university.edu/assets/index.js` (`404 Not Found`), so the React script never even loads!
3. **Check if Nginx returned `index.html` (`text/html`) for a `.js` file (`Uncaught SyntaxError: Unexpected token '<'`):** This happens when an old cached `index.html` requests a previous deployment's hashed JS bundle that was deleted on the server, and the SPA fallback mistakenly returns `index.html` instead of a `404` for missing `/assets/*` files.

---

### Q97. A component reads `window.innerWidth` or `localStorage.getItem("theme")` directly during rendering. Why does this fail in Server-Side Rendering (SSR / Next.js), and how do you fix it?

**Answer:**

During Server-Side Rendering (SSR), your component function executes first inside **Node.js on the server** to generate the initial HTML string.
1. **`ReferenceError: window / localStorage is not defined`:** Node.js does not have a browser `window`, `document`, or `localStorage` object, so accessing them directly in the component body crashes the server render.
2. **Hydration Mismatch Error:** Even if you guard it with `typeof window !== "undefined" ? localStorage.getItem("theme") : "light"`, the server renders `"light"` into the HTML, while the client's first render reads `"dark"` from `localStorage`. React compares the server HTML against the client's initial render output and throws a **Hydration Mismatch** error!

**How to fix it:**
- Read browser-only APIs (`window`, `localStorage`) inside **`useEffect`** (which only runs in the browser *after* initial hydration completes) or use `useSyncExternalStore` with a `getServerSnapshot` fallback.

---

### Q98. Your deployed React application works on the homepage (`/`), and clicking links to `/admin/students` works, but hitting Refresh (F5) on `/admin/students` returns a `404 Not Found` page from the web server. What is the issue and fix?

**Answer:**

In a Single-Page Application (SPA), `/admin/students` is a **virtual client-side route** managed by React Router in the browser using the HTML5 History API—there is no actual `/admin/students.html` file on the static hosting server. When you click a `<Link>` from `/`, the browser never asks the server for `/admin/students`. However, when you press F5 (Refresh), the browser sends an HTTP `GET /admin/students` request directly to the web server (Nginx, CloudFront, Vercel, Netlify).

To fix this, configure a **rewrite / fallback rule on the web server** (such as `try_files $uri $uri/ /index.html;` in Nginx, or a `/* -> /index.html (200)` rewrite rule in Netlify/Vercel/CloudFront) so the server responds with `index.html` and lets React Router render `/admin/students` on the client.

---

### Q99. You removed the "Approve Results" button from the React UI for non-admin users, yet a student was still able to approve results. What is the underlying problem?

**Answer:**

The underlying architectural flaw is **relying on client-side UI visibility ("Security by Obscurity") instead of server-side authorization**.

React runs entirely on the user's own computer. Any user can inspect the minified JavaScript bundle in Browser DevTools to discover the API endpoint path (`POST /api/exams/EX101/approve`) and invoke it directly from the browser console or Postman using their valid student authentication token.

To fix this permanently, the backend API endpoint must enforce **Role-Based Access Control (RBAC)** middleware that verifies the caller's authenticated role on the server (`if (req.user.role !== "admin") return res.status(403).json({ error: "Forbidden" })`) before executing any sensitive operation.

---

### Q100. Your React application runs smoothly for the first 10 minutes, but becomes progressively slower and crashes the browser tab after 3 hours of continuous use by an exam administrator. What would you investigate?

**Answer:**

Progressive slowdown over hours of single-page usage without a page refresh is the classic signature of a **client-side memory leak or accumulated background listeners**. I would open Chrome DevTools (**Memory** and **Performance** tabs), take two **Heap Snapshots** 15 minutes apart while navigating back and forth between screens, and inspect what objects are accumulating:

1. **Unremoved Event Listeners or Timers (`useEffect` missing cleanup):** Every time the admin opens and closes a page or modal, a new `window.addEventListener`, `setInterval`, or WebSocket listener is registered without cleaning up the old one, so after 200 page navigations, 200 intervals or listeners are firing simultaneously on every event!
2. **Unrevoked Blob URLs (`URL.createObjectURL`):** Generating PDF hall-ticket previews or student photo previews without calling `URL.revokeObjectURL()` in a cleanup function leaks multi-megabyte binary blobs in browser memory.
3. **Unbounded Client Cache or Log Array:** Appending real-time WebSocket events or polling logs into an array in React/Redux state forever without capping the array length (e.g., keeping only the latest 200 entries).
4. **Detached DOM Nodes:** Third-party imperative chart or table plugins retaining references to DOM nodes after React unmounted the component.

---

# Part 10: How to Answer Any React Scenario in an Interview

When an interviewer presents an unfamiliar scenario, structure your spoken answer in **five clear steps**:

1. **Identify the symptom and root cause:** State clearly what is happening in one sentence (*"The list displays stale student data because an earlier slow search request is finishing after a newer fast request and overwriting the state."*).
2. **Name the underlying React / Web concept:** Connect the bug to the core principle (*"This is an asynchronous race condition inside a `useEffect` data fetch."*).
3. **Explain the solution in plain sentences:** Describe how you would fix it before mentioning syntax (*"I would attach an `AbortController` to the fetch request inside `useEffect` and call `controller.abort()` in the Effect's cleanup function whenever the search query changes."*).
4. **Mention the production edge cases & backend boundary:** Show senior engineering awareness (*"I would also debounce the search input by 300ms to reduce server load, and if the dataset is large, use server-side pagination."*).
5. **State how you would verify the fix:** Mention the tool or test (*"I would verify this in the browser Network tab by throttling the connection to Slow 3G and typing rapidly to confirm earlier requests show as `(canceled)`."*).

---

# Part 11: Final React Interview Checklist

Before attending your interview, make sure you can explain these areas naturally in sentences:

- **Core React:** Component architecture, JSX, props, one-way data flow, `useState` snapshots, functional updates, conditional rendering (and avoiding the `0 &&` trap), list `key` reconciliation, controlled vs uncontrolled inputs, and `useRef`.
- **Hooks & Internals:** Why Rules of Hooks exist (Fiber linked list), `useEffect` synchronization and cleanup, stale closures, `useContext` vs `useReducer`, Virtual DOM vs Render phase vs Commit phase, and `<StrictMode>`.
- **Performance:** State colocation, component composition (`children`), React DevTools Profiler, `React.memo` + `useMemo` + `useCallback`, `React.lazy` code splitting, and list virtualization.
- **API, Security & Production:** `AbortController` race-condition prevention, HTTP status handling (`400/401/403/404/409/500`), optimistic updates, CORS preflight, XSS prevention, backend RBAC boundaries, and SPA fallback routing (`try_files` in Nginx).
- **Your Project Stories (SeatSync):** Prepare **three crisp stories**—one complex feature you built (such as bulk Excel import with preview or asynchronous seat allocation), one tricky bug you diagnosed (such as a stale closure, race condition, or auth refresh redirect bug), and one architectural trade-off you made (such as server-side vs client-side pagination).
