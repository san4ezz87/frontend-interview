# Change Detection

## **1. Q:** **What is zone.js and how does it work?**

### **Answer**

Zone.js is a library that intercepts and tracks asynchronous operations in JavaScript. Zone.js works by monkey-patching browser APIs (like `setTimeout`, `addEventListener`, `Promise.then`, and `fetch`) to wrap them in execution contexts called "zones" that can intercept their start and completion.

---

## **2. Q:** **How does Angular's change detection use Zone.js?**

### **Answer**

Angular wraps the application in a dedicated zone called NgZone. When Zone.js detects that an asynchronous operation has completed (such as an event, timer, or HTTP request) - Angular triggers change detection.

---

## **3. Q:** **If an HTML element or Angular component emits an event but there's no listener, why isn't change detection triggered?**

### **Answer**

Change detection isn't triggered because without an event listener attached, there's no `addEventListener` call for Zone.js to intercept and monkey-patch. Since Zone.js can only track asynchronous operations it has wrapped, an unlistened event never enters Angular's zone and therefore doesn't notify NgZone to trigger change detection. In short: `.addEventListener` is patched, not `.emit`

---

## **4. Q: Explain the change detection process in Angular.**

### **Answer**

Angular performs change detection by traversing the component tree from top to bottom (parent to child). For each component, it checks if any bindings have changed by comparing current values with previous values. If changes are detected—either through modified component inputs, events, async operations, or HTTP requests—Angular updates the corresponding DOM elements to reflect the new state.

---

## **5. Q:** **During a change detection cycle, what does Angular do with a component that has the Default change detection strategy if nothing has changed inside it since the last cycle?**

### **Answer**

The Default strategy (also called "CheckAlways") checks the component on every change detection cycle regardless of whether anything changed within it. Angular will still compare all of the component's bindings with their previous values, even if no events, HTTP requests, or async operations occurred in that component.

---

## **6. Q:** **How does Angular handle OnPush components during change detection?**

### **Answer**

Angular only checks an OnPush component if it's marked as dirty. If the component is marked as dirty, Angular compares its bindings with their previous values and updates the DOM. If not marked as dirty, Angular skips the entire component subtree (all it's children).

---

## **7. Q:** **When does an OnPush component become marked as dirty?**

### **Answer**

An OnPush component is marked as dirty in the following cases:

1. **Input changes** - When any `@Input()` property receives a new reference (checked using `===` comparison)
2. **Events within the component** - When an event is handled inside the component:
    - Event listeners in the template (e.g., `(click)`)
    - `@Output()` emissions from child components
    - `@HostListener()` decorated methods
3. **Async pipe** - When an Observable or Promise bound with the async pipe emits a new value
4. **Manual trigger** - When `ChangeDetectorRef.markForCheck()` is called

---

## **8. Q: How does `ChangeDetectorRef.markForCheck()` work?**

### **Answer**

`markForCheck()` marks the current component and all ancestor components up to the root as dirty, ensuring they will be checked in the next change detection cycle. It's necessary to mark the entire path to the root because if any ancestor component uses OnPush strategy and isn't marked as dirty, the target component's subtree would be skipped during traversal. Note that `markForCheck()` only schedules the check—it doesn't trigger change detection immediately.

---

## **9. Q: How do Signals affect change detection in Angular?**

### **Answer**

When a signal used in a component's template is updated, Angular automatically marks that component as dirty and schedules change detection. Signals work seamlessly with OnPush components, triggering updates without requiring input changes or events.
