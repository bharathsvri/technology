# Custom Hooks

## Convention

Name starts with **`use`**; may call other hooks.

```ts
function useWindowWidth() {
  const [w, setW] = useState(window.innerWidth);
  useEffect(() => {
    const onResize = () => setW(window.innerWidth);
    window.addEventListener('resize', onResize);
    return () => window.removeEventListener('resize', onResize);
  }, []);
  return w;
}
```

## Sharing logic

Extract repeated state+effect patterns across components.

Next: [Context API](09-Context-API.md).
