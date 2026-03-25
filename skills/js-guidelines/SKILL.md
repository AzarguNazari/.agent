---
name: js-guidelines
description: >-
  JavaScript and TypeScript frontend style—scope, guard clauses, errors, DOM,
  loops, and forbidden patterns.
---

## Trigger
Use when writing or reviewing JavaScript/TypeScript frontend code.

## Core Rules

### Declarations
- Declare variables as close to usage as possible
- Remove all unused variables, functions, imports, classes

### Comments
- No `TODO` / `FIXME` unless linked to a Jira ticket
- Self-documenting code preferred

### Scope
- Use IIFEs to avoid polluting global scope
```js
// BAD
var myVar = 'hello';

// GOOD
(function() { var myVar = 'hello'; })();
```

### Early Return (Guard Clauses)
- Always exit early when dependencies are missing
- Prefer `continue` in loops over wrapping body in `if`
```js
// GOOD
if (!this.element || !this.otherElement) return false;

// GOOD in loop
for (let i = 0; i < 1000; i++) {
    if (!foo(i)) continue;
    bar(i);
}
```

### Error Handling
- No empty catch blocks — always handle or rethrow with context

### Loops & Collections
- No `for-in` over arrays — use `for-of`, `.forEach()`, `.map()` etc.
- `for-in` only for iterating object keys
- Never use Array as a map/hash — use `Object` or `Map`

### DOM & Styling
- No inline styles from JS except dynamic positioning or show/hide
- Use CSS classes for all other styling
- Select elements via `data-*` attributes, not class names or tags
```js
// GOOD
this.element = '[data-my-module]';
```

### Private Methods
- Prefix private methods with `_`
```js
_myPrivateMethod(str) { ... }
```

### Dynamic Access — Forbidden
- No dynamic property/method access via bracket notation with conditionals
```js
// FORBIDDEN
myNameSpace[condition ? 'foo' : 'bar']();
```

### Prototype — Forbidden
- Never modify built-in prototypes
```js
// FORBIDDEN
Array.prototype.foo = function() { ... };
```

## Output format
When generating JS code, apply all rules above. Flag violations with `// VIOLATION: <rule>`.
