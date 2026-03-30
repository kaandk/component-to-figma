# Auto-Layout Optimized Patterns for Figma Capture

These patterns ensure HTML components translate cleanly into Figma auto-layout frames.

## Page Container

```tsx
<div style={{
  backgroundColor: "#FFFFFF",
  minHeight: "100vh",
  padding: 64,
  display: "flex",
  flexDirection: "column",
  gap: 48,
}}>
  {/* Title section */}
  <div style={{ display: "flex", flexDirection: "column", gap: 8 }}>
    <h1 style={{ fontSize: 24, fontWeight: 700, color: "#1A1A2E", margin: 0 }}>
      Component Name
    </h1>
    <p style={{ fontSize: 14, color: "#8C8C9A", margin: 0 }}>
      Description of extracted components
    </p>
  </div>

  {/* Component grid */}
  <div style={{
    display: "flex",
    flexWrap: "wrap",
    gap: 32,
    alignItems: "flex-start",
  }}>
    {/* Cards go here */}
  </div>
</div>
```

## Card Wrapper

Each component gets wrapped in a card with a labeled header:

```tsx
function WidgetCard({ label, children, width = 230 }: {
  label: string;
  children: React.ReactNode;
  width?: number;
}) {
  return (
    <div style={{
      width,
      backgroundColor: "#FFFFFF",
      borderRadius: 16,
      border: "1px solid #F3F4F6",
      overflow: "hidden",
      boxShadow: "0 1px 3px rgba(0,0,0,0.08), 0 1px 2px rgba(0,0,0,0.04)",
    }}>
      <div style={{
        padding: "6px 12px",
        fontSize: 10,
        fontWeight: 600,
        textTransform: "uppercase",
        letterSpacing: "0.05em",
        color: "#8C8C9A",
        borderBottom: "1px solid #F3F4F6",
      }}>
        {label}
      </div>
      {children}
    </div>
  );
}
```

## Style Translation Rules

### Tailwind to Inline Mapping

| Tailwind | Inline style |
|---|---|
| `flex flex-col gap-2` | `{ display: "flex", flexDirection: "column", gap: 8 }` |
| `items-center justify-between` | `{ alignItems: "center", justifyContent: "space-between" }` |
| `p-3` | `{ padding: 12 }` |
| `text-xs text-gray-500` | `{ fontSize: 12, color: "#6B7280" }` |
| `font-semibold` | `{ fontWeight: 600 }` |
| `rounded-full` | `{ borderRadius: 9999 }` |
| `rounded-2xl` | `{ borderRadius: 16 }` |
| `w-10` | `{ width: 40 }` |
| `h-5` | `{ height: 20 }` |
| `bg-gray-100` | `{ backgroundColor: "#F3F4F6" }` |
| `border border-gray-100` | `{ border: "1px solid #F3F4F6" }` |
| `flex-1` | `{ flex: 1 }` |
| `shrink-0` | `{ flexShrink: 0 }` |
| `overflow-hidden` | `{ overflow: "hidden" }` |

### What to Strip

- `motion.div` -> plain `div`
- `animate`, `initial`, `transition` props -> remove entirely
- `onClick`, `onMouseEnter`, `onMouseLeave` -> remove
- `useState`, `useEffect` -> replace with static values
- `cursor-pointer` / `hover:` styles -> remove
- `className` -> convert to `style` object
- CSS `transition` properties -> remove
- `absolute`, `fixed`, `sticky` positioning -> remove (use flex)
- `transform`, `rotate`, `scale`, `blur` -> remove
- `z-index` -> remove

### Figma Auto-Layout Best Practices

- Every container should be a flex container (`display: "flex"`)
- Use `gap` instead of margins between children
- Use explicit `width` on leaf components (cards, badges)
- Use `flex: 1` for elements that should fill available space
- Avoid `margin` — use parent `gap` and `padding` instead
- Set explicit `height` on progress bars, badges, etc.
- Use `alignItems: "flex-start"` on wrap containers to prevent stretching
