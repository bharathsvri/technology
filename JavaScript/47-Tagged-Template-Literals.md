# Tagged Template Literals

**Tagged templates** call a **function** with **static strings** and **cooked/raw** substitutions—used by **styled-components**, **lit-html**, **sql** tagged libs, and **i18n**.

```js
function highlight(strings, ...values) {
  return strings.reduce((acc, s, i) => acc + s + (values[i] ?? ''), '');
}
highlight`a=${1}`; // strings: ['a=',''], values: [1]
```

---

## `strings.raw`

Access **raw** escapes (`\n` unchanged)—important for **regex** or **CSS-in-JS** generators.

---

## Security

Never pass **untrusted** user input into **SQL** tagged templates without **parameterization**—tags are **not magic** sanitizers by themselves.

---

## Related

- [Functions and Parameters](06-Functions-and-Parameters.md)
- [Regular Expressions](23-Regular-Expressions.md)
