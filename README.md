# ctrovalidate-react

**Headless form validation hook for React.**

`ctrovalidate-react` provides `useCtrovalidate`, a single hook that wraps [`ctrovalidate-core`](https://www.npmjs.com/package/ctrovalidate-core)'s validation engine with reactive state, stable callbacks, and automatic abort handling for async rules.

[![npm version](https://img.shields.io/npm/v/ctrovalidate-react.svg)](https://www.npmjs.com/package/ctrovalidate-react)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

---

## Installation

```bash
npm install ctrovalidate-react ctrovalidate-core
```

**Requirements:** React >=16.8.0

---

## Quick Start

```tsx
import { useCtrovalidate } from 'ctrovalidate-react';

interface LoginForm { email: string; password: string; }

function LoginPage() {
  const { values, errors, handleChange, handleBlur, validateForm, isValidating } =
    useCtrovalidate<LoginForm>({
      initialValues: { email: '', password: '' },
      schema: { email: 'required|email', password: 'required|minLength:8' },
    });

  return (
    <form onSubmit={async (e) => { e.preventDefault(); if (await validateForm()) { /* submit */ } }}>
      <input value={values.email} onChange={(e) => handleChange('email', e.target.value)} onBlur={() => handleBlur('email')} />
      {errors.email && <span>{errors.email}</span>}

      <input type="password" value={values.password} onChange={(e) => handleChange('password', e.target.value)} onBlur={() => handleBlur('password')} />
      {errors.password && <span>{errors.password}</span>}

      <button type="submit" disabled={Object.values(isValidating).some(Boolean)}>Login</button>
    </form>
  );
}
```

---

## API

```typescript
const {
  values,         // Current form state
  errors,         // Partial<Record<keyof T, string>> — undefined if valid
  isDirty,        // Tracks touched fields
  isValidating,   // Tracks async validation in progress
  handleChange,   // Updates value, marks dirty, validates
  handleBlur,     // Marks dirty, validates (unless validateOnBlur: false)
  validateField,  // Validate a single field
  validateForm,   // Validate all fields
  reset,          // Reset to initial values, clear errors
} = useCtrovalidate<T>({
  schema,            // Required: field-to-rules mapping
  initialValues,     // Optional: form defaults (default: {})
  validateOnBlur,    // Optional: validate on blur (default: true)
  customRules,       // Optional: custom sync/async rule functions
  aliases,           // Optional: rule combination aliases
  messages,          // Optional: custom error messages
  locale,            // Optional: locale override for translator
});
```

---

## Abort Handling

Each field has its own `AbortController`. Re-validating a field aborts any in-flight async rule. All controllers are aborted on unmount — no stale state updates.

---

## Related Packages

- **[ctrovalidate-core](https://www.npmjs.com/package/ctrovalidate-core)** — Validation engine
- **[ctrovalidate-browser](https://www.npmjs.com/package/ctrovalidate-browser)** — Vanilla JS DOM integration
- **[ctrovalidate-vue](https://www.npmjs.com/package/ctrovalidate-vue)** — Vue composable
- **[ctrovalidate-svelte](https://www.npmjs.com/package/ctrovalidate-svelte)** — Svelte stores
- **[ctrovalidate-next](https://www.npmjs.com/package/ctrovalidate-next)** — Next.js server actions

---

## License

MIT © [Ctrotech](https://github.com/ctrotech-tutor)

Full documentation: https://ctrovalidate.vercel.app
