---
title: Use Activity Component for Show/Hide
impact: MEDIUM
impactDescription: preserves state/Render Tree
tags: rendering, activity, visibility, state-preservation
---

## Use Activity Component for Show/Hide

Use React Native's `<Activity>` to preserve state/Render Tree for expensive components that frequently toggle visibility.

**Usage:**

```tsx
import { Activity } from 'react'

function Dropdown({ isOpen }: Props) {
  return (
    <Activity mode={isOpen ? 'visible' : 'hidden'}>
      <ExpensiveMenu />
    </Activity>
  )
}
```

Avoids expensive re-renders and state loss.
