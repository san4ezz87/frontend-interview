# Interview

---

## This Binding

### **Q: First problem - what will be logged?**

[https://stackblitz.com/edit/stackblitz-starters-v6dl7gjh?file=1.js](https://stackblitz.com/edit/stackblitz-starters-v6dl7gjh?file=1.js)

```jsx
const object = {
  message: 'Hello, World!',
  logMessage() {
    console.log(this.message); // What is logged?
  }
};

setTimeout(object.logMessage, 1000);
```

**Answer:**

`undefined`

---

### **Q: Second problem - what will be logged? Compare result from Pet and Party?**

[https://stackblitz.com/edit/stackblitz-starters-v6dl7gjh?file=2.js](https://stackblitz.com/edit/stackblitz-starters-v6dl7gjh?file=2.js)

```jsx
function Pet(name) {
  this.name = name;
  this.getName = () => this.name;
}

const cat = new Pet('Fluffy');
console.log(cat.getName()); // What is logged?
const { getName } = cat;
console.log(getName());     // What is logged?

const Party = {
  who: 'World',
  greet() {
    return `Hello, ${this.who}!`;
  },
  farewell: () => {
    return `Goodbye, ${this.who}!`;
  }
};
console.log(Party.greet());    // What is logged?
console.log(Party.farewell()); // What is logged?
```

**Answer:**

Pet will print "Fluffy", "Fluffy".

Party will print "Hello World" and "Goodbye, undefined".

---

## Event Loop

### **Q: What parts of the JavaScript runtime work together with the event loop?**

**Answer:**

1. **Call Stack** – executes synchronous code.
2. **Heap** – stores objects and data.
3. **Microtask Queue** – for Promise callbacks and queueMicrotask().
4. **Macrotask Queue** – for setTimeout, setInterval, and I/O tasks.
5. **Rendering Queue** – (browser only) handles visual updates and requestAnimationFrame() before the next paint.

---

### **Q: In which queue will be a click event callback placed?**

**Answer:**

A click event callback is placed in the macrotask queue (also called the task queue).

---

### **Q: When callback will be set to queue? Who will set a timer for setTimout?**

**Answer:**

**The timer is set by the browser's Web API, and once the specified time has elapsed, its callback is placed in the macrotask queue.**

---

### **Q: What order will be for console.log**

[https://stackblitz.com/edit/stackblitz-starters-v6dl7gjh?file=3.js](https://stackblitz.com/edit/stackblitz-starters-v6dl7gjh?file=3.js)

```jsx
console.log(1);

setTimeout(function () {
  console.log(2);
}, 0);

new Promise(function (resolve, reqect) {
  console.log(6);
  resolve(3);
})
  .then(function (val) {
    console.log(val);
  })
  .then(function () {
    console.log(4);
  });

console.log(5);
```

**Answer:**

`1, 6, 5, 3, 4, 2`

---

## Debounce & Throttle

### **Q: The user types quickly in a search bar, and you want to call the API only once they stop typing.**

**Answer:**

Use **debounce** to delay execution until the user pauses.

```jsx
function debounce(fn, delay) {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn(args), delay);
  };
}
```

---

### **Q: The page listens to scroll to load more items, but the handler fires too often and lags the UI.**

**Answer:**

Use **throttle** to limit the scroll handler to run every few milliseconds.

```jsx
function throttle(func, delay) {
    let timeout=null
    return () => {
        if(!timeout) {
            func()
            timeout=setTimeout(() => {
                timeout=null
            }, delay)
        }
    }
}
```

---

## TypeScript

### **Q: Create a utility type to extract the type of the 'data'?**

[https://stackblitz.com/edit/stackblitz-starters-d3kwju6q?file=data-type.ts](https://stackblitz.com/edit/stackblitz-starters-d3kwju6q?file=data-type.ts)

```tsx
/**
 * Type utility to extract the type of the `data` property
 * from an object, or return `undefined` if the property does not exist.
 */

type DataType<T> = TODO;

type test2 = DataType<{ data: string; code: number }>;
type testCase2 = Expect<Equal<test2, string>>;

type test3 = DataType<{ error: string }>;
type testCase3 = Expect<Equal<test3, undefined>>;

type test4 = DataType<{
  data: { content: string; keywords: string[] };
  code: number;
}>;
type testCase4 = Expect<Equal<test4, { content: string; keywords: string[] }>>;
```

**Answer:**

```tsx
type DataType<T> = T extends {data: infer Res} ? Res : undefined;
```

---

## Authentication

### **Q: How would you implement authentication in a modern web application? What are the main approaches for passing credentials from client to server?**

**Answer:**

1. cookies
2. authorization header
3. maybe other

---

### **Q: What advantages has cookies?**

**Answer:**

from the client app perspective - there is nothing to do to pass authorization with requests, the cookie added by browser automatically.

---

### **Q: If you have multiple microservices on different subdomains (api1.company.com, api2.company.com), how do you share cookies?**

**Answer:**

- Set cookie with Domain=.company.com (parent domain)
- Use API Gateway pattern (single auth domain)
- Token-based auth is cleaner for microservices

---

### **Q: Authorization headers - Walk me through your implementation using [axios/fetch/custom hook]**

**Answer:**

- axios - axios.interceptors.request.use
- wrapper around fetch
- custom react hook

---

### **Q: How do you handle multiple concurrent requests when the token expires?**

**Answer:**

1. Queue requests during refresh
2. Single refresh call for all waiting requests
3. Resume queued requests with new token

**Comments:**

#### Red Flags to Watch For

🚩 **Can't explain token refresh - indicates no real production experience**

🚩 **"Just use a library" without understanding what it does**

---

### **Q: You're reviewing a component from a codebase that uses Angular, but you primarily work with React.**

1. What is this template doing? What pattern is being used here?
2. How would you achieve the same functionality in React?
3. Is this pattern available in vanilla JavaScript/Web Components?

```html
<h5>
  <ng-content select="[title]" />
</h5>
<p>
  <ng-content select="[instructions]" />
</p>
<section>
  <ng-content select="[presentation]" />
  <tk-divider vertical />
  <ng-content select="[interaction]" />
</section>
<ng-content />
```

**Answer:**

#### What Strong Candidates Will Demonstrate

##### Recognition Phase

They should identify:

- **Pattern name:** Content projection / transclusion / slots / named slots
- **Purpose:** Allowing parent components to inject content into specific locations within the child component's template
- **The selector mechanism:** select="[title]" is selecting elements with the title attribute
- **Default slot:** The final `ng-content` without a selector catches any unmatched content

##### Framework Translation Phase

**Excellent answer indicators:**

***React candidates* might say:**

- "React doesn't have a direct equivalent to named slots"
- "I'd use props to pass content/components" or "Use children with a selector function"

---

## Additional Resources

[Change Detection](change-detection.md.md)

[Dependency Injection](dependency-injection.md)
