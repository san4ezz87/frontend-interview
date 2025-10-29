# Dependency Injection

## **1. Q: Explain the DI token resolution process step by step.**

### **Answer**

Angular's dependency injection follows a hierarchical lookup process that continues until either a provider is found or the null injector is reached:

1. **Element injector of the caller** - Starts at the component/directive where `inject()` is called
2. **Ancestor element injectors** - Traverses up the component tree through parent element injectors
3. **Module injector hierarchy** - If not found in element injectors, searches the module injector chain:
    1. **Lazy-loaded module injector** (if the component is in a lazy-loaded module)
    2. **Root injector** (application-level providers)
    3. **Platform injector** (platform-level providers shared across apps)
4. **Null injector** - The final injector that always throws an error

If the token is not found and the `@Optional()` decorator is not used, Angular throws a "NullInjectorError". With `@Optional()`, it returns `null` instead.

---

## **2. Q: What are dependency resolution modifiers? Name them and explain how each of them works.**

### **Answer**

Dependency resolution modifiers alter the standard DI token lookup behavior by skipping injectors or limiting the search scope. Angular provides four modifiers:

1. **`@Optional()`** - Prevents throwing a `NullInjectorError` if the dependency is not found. Returns `null` instead.
2. **`@Self()`** - Limits the lookup to only the current injector (the component's own element injector). Does not search parent injectors.
3. **`@SkipSelf()`** - Skips the current injector and starts the lookup from the parent injector, moving up the hierarchy.
4. **`@Host()`** - Limits the lookup to the host component's injector. Searches up to the host element but stops before reaching the module injector hierarchy.

---

## **3. Q: How can you provide a dependency in Angular?**

### **Answer**

Angular offers multiple ways to provide dependencies:

1. **`@Injectable()` decorator with `providedIn`** - Tree-shakable providers at the class level:

- `providedIn: 'root'` - Available application-wide (singleton)
- `providedIn: 'platform'` - Shared across multiple Angular apps
- `providedIn: 'any'` - Unique instance per lazy-loaded module
- `providedIn: SomeModule` - Scoped to a specific module

2. **`providers` array** - Explicit registration in:

- `@NgModule()` - Module-level providers
- `@Component()` - Component-level providers (new instance per component)
- `@Directive()` - Directive-level providers
- `Injector.create()` - Dynamic injector creation
- `TestBed.configureTestingModule()` - Test environment providers

3. **`viewProviders`** - Component-only, restricts provider visibility to the component's view (not projected content)

---

## **4. Q: What types of providers exist in Angular?**

### **Answer**

Angular supports four provider types:

1. **Class provider** - `{ provide: Token, useClass: SomeClass }`
    - Instantiates the specified class when the token is requested
    - `@Injectable({ providedIn: 'root' })` is shorthand for `{ provide: SomeClass, useClass: SomeClass }`
2. **Existing provider (Alias)** - `{ provide: AliasToken, useExisting: ExistingToken }`
    - Creates an alias to an already registered provider
    - Both tokens resolve to the same instance
3. **Value provider** - `{ provide: Token, useValue: someValue }`
    - Provides a constant value, object, or primitive
    - Useful for configuration objects or constants
4. **Factory provider** - `{ provide: Token, useFactory: (dep1, dep2) => new SomeClass(dep1, dep2), deps: [Dep1Token, Dep2Token] }`
    - Uses a factory function to create the dependency with custom logic
    - `deps` array specifies dependencies to inject into the factory function

---

## **5. Q: When is the `useFactory` function called?**

### **Answer**

The factory function is called lazily during the first injection of the token. When `inject(Token)` or the token is injected via constructor for the first time, Angular invokes the factory function and caches the result. Subsequent injections return the same cached instance (singleton behavior within that injector's scope).

---

## **6. Q: What is an injection context and when is it available?**

### **Answer**

An injection context is the runtime environment that Angular's DI system requires to resolve dependencies using the `inject()` function. It's automatically available in these scenarios:

1. **Constructor injection** - During construction of classes instantiated by DI (e.g., `@Injectable`, `@Component`, `@Directive`)
2. **Field initializers** - In class field initializers of DI-managed classes
3. **Factory functions** - In `useFactory` functions defined in provider configurations
4. **InjectionToken factories** - In the `factory` function of an `InjectionToken` definition
5. **Framework hooks and callbacks** that run within an injection context:

- Route guards (`CanActivate`, `CanDeactivate`, etc.)
- Route resolvers
- Interceptors (HTTP interceptors)
- `APP_INITIALIZER` functions

6. **Manual context creation** - Inside `runInInjectionContext()` to manually establish an injection context

---

## **7. Q: What is forwardRef?**

### **Answer**

Allows to refer to references which are not yet defined.
For instance, [`forwardRef`](https://angular.dev/api/core/forwardRef) is used when the `token` which we need to refer to for the purposes of DI is declared, but not yet defined. It is also used when the `token` which we use when creating a query is not yet defined.
[`forwardRef`](https://angular.dev/api/core/forwardRef) is also used to break circularities in standalone components imports.

---
