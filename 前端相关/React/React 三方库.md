# React 三方库

## 状态管理

### zustand
现阶段主推的状态管理库。

- [WebSite](https://zustand-demo.pmnd.rs/)
- [GitHub](https://github.com/pmndrs/zustand)
- [Document](https://zustand.docs.pmnd.rs/getting-started/introduction)

```ts
import { create } from 'zustand'

const useStore = create((set) => ({
  count: 1,
  inc: () => set((state) => ({ count: state.count + 1 })),
}))

function Counter() {
  const { count, inc } = useStore()
  return (
    <div>
      <span>{count}</span>
      <button onClick={inc}>one up</button>
    </div>
  )
}
```