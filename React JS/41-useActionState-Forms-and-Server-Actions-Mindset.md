# React: `useActionState`, progressive forms, and the Server Actions mindset

## What changed
React 19 family APIs (as implemented in React and frameworks like **Next.js App Router**) encourage treating **mutations** (create/update/delete) as **actions**: async functions that run on the server or a trusted boundary and return the next **UI state** or **field errors** instead of scattering imperative `fetch` calls across effects.

## `useActionState` mental model
- You pass React an **action** `(prevState, formData) => nextState` (often async).
- React gives you **`[state, dispatch, isPending]`** where `dispatch` is tied to **form submission** semantics.
- Ideal for **login**, **settings save**, **checkout steps**—flows where users expect a **pending** indicator and inline validation messages.

## Compare with what you already use
- **`react-hook-form` + Zod** (`28-Form-Libraries-react-hook-form-and-Zod.md`) stays excellent for **large client-only** forms with fast field-level validation.
- **`useActionState`** shines when the **authoritative validation** lives on the server or you want **minimal client JS** for a simple form.

## Progressive enhancement (when the framework supports it)
Forms can **work without JavaScript** if actions are wired through a meta-framework that serializes submissions to the server. Treat that as an optional product requirement, not a universal React guarantee.

## Pitfalls
- Do not mix **controlled** field state and server actions blindly—pick one source of truth per field group.
- **`isPending`** is not a substitute for **per-row** mutation tracking in dense tables; use keys or nested state patterns (`38-useOptimistic-and-useFormStatus-React-19.md`).

## Related docs
- `10-Forms-in-React.md`, `28-Form-Libraries-react-hook-form-and-Zod.md`
- `23-React-Server-Components-Overview.md`
- `38-useOptimistic-and-useFormStatus-React-19.md`
