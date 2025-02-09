- Use function instead of arrow func in useEffect to show name func in dev tool 
- 
```
function App() {
  const ref = useRef();
  useEffect(() => {
    ref.current?.focus();
  }, []);
  return <input ref={ref} type="text" />;
}

=>

function App() {
  const ref = useCallback((inputNode) => {
    inputNode?.focus();
  }, []);

  return <input ref={ref} type="text" />;
}
```

```
ComponentWithChildren

ComponentProps<"button"> | ComponentProps<typeof OtherComponent>

ComponentType<{...}> => defined type for Component pass from props

MouseEventHandler<HTMLButtonElement> instead of (e: MouseEvent<HTMLButtonElement>) => void

NoInfer => locations: L[],  defaultLocation: NoInfer<L> => check "defaultLocation" have belong to locations
```
