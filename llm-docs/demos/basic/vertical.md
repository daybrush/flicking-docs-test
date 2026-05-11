# Vertical

The [`horizontal`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#horizontal) option sets the direction of panel movement. When `true`, panels move horizontally (left/right); when `false`, panels move vertically (up/down).



## Summary

### Key Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| [`horizontal`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#horizontal) | `boolean` | `true` | Panel movement direction (`true`: horizontal, `false`: vertical) |

### Comparison by Value

| Value | Behavior | Suitable For |
|-------|----------|--------------|
| `true` | Drag left/right to move panels | General carousels, image galleries, banner sliders |
| `false` | Drag up/down to move panels | Vertical card stacks, story viewers, vertical onboarding |

## Details

### horizontal: true in Detail
This is the default. Panels are arranged horizontally, and users move panels by dragging left/right. Used in most carousel UIs.

### horizontal: false in Detail
Panels are arranged vertically, and users move panels by dragging up/down. Suitable for vertical scroll UIs or fullscreen story viewers.

**Important**: In vertical mode, a **fixed height** must be set on the viewport. Without a height, panels will not be visible.

```css
.flicking-viewport {
  height: 300px; /* Required in vertical mode */
}
```

### Related Options
- **Relationship with adaptive**: The `adaptive` option only works when `horizontal: true`. It has no effect in vertical mode.
- **Relationship with nested**: If parent and child Flicking have different horizontal values, it works naturally without the `nested` option.
- **Relationship with inputType**: The default inputType `["mouse", "touch"]` supports both horizontal and vertical.

### Use Cases

> **Info: When to use?**
- **horizontal: true**: Image galleries, product sliders, banners, tab-style UI
- **horizontal: false**: TikTok/Instagram Stories style, vertical card stacks, fullscreen onboarding

### Notes

> **Warning: Viewport height required in vertical mode**
When setting `horizontal: false`, panels will not be displayed if a fixed height is not set on the viewport. Make sure to set a `height` value in CSS.

> **Warning: Scroll conflict on touch devices**
In vertical mode, up/down dragging may conflict with page scrolling. If necessary, review the `preventDefaultOnDrag` option or CSS `touch-action` settings.

## Related Links

### Related Options
- [`adaptive`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#adaptive): Adjust viewport to panel height (horizontal: true only)
- [`nested`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#nested): Nested Flicking behavior
- [`inputType`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#inputtype): Input type settings

### Related Demos
- [Nested](https://daybrush.github.io/flicking-docs-test/llm-docs/demos/basic/nested.md): Nested Flicking (nested not needed for different directions)
- [Adaptive](https://daybrush.github.io/flicking-docs-test/llm-docs/demos/basic/adaptive.md): Panel height adaptation

## Code

### React
```jsx
import Flicking from "@egjs/react-flicking";
import "@egjs/react-flicking/dist/flicking.css";
import "./styles.css";

export default function App() {
  return (
    <div>
      {/* horizontal: true (default) */}
      <div className="demo-container">
        <div className="demo-label">horizontal: true (default, horizontal)</div>
        <Flicking horizontal={true} align="center">
          <div className="flicking-panel panel-1">1</div>
          <div className="flicking-panel panel-2">2</div>
          <div className="flicking-panel panel-3">3</div>
          <div className="flicking-panel panel-4">4</div>
          <div className="flicking-panel panel-5">5</div>
        </Flicking>
      </div>

      {/* horizontal: false (vertical) */}
      <div className="demo-container">
        <div className="demo-label">horizontal: false (vertical)</div>
        <Flicking horizontal={false} align="center" panelsPerView={3}>
          <div className="flicking-panel panel-1">1</div>
          <div className="flicking-panel panel-2">2</div>
          <div className="flicking-panel panel-3">3</div>
          <div className="flicking-panel panel-4">4</div>
          <div className="flicking-panel panel-5">5</div>
        </Flicking>
      </div>
    </div>
  );
}
```

### Vue3
```vue
<template>
  <div>
    <!-- horizontal: true (default) -->
    <div class="demo-container">
      <div class="demo-label">horizontal: true (default, horizontal)</div>
      <Flicking :options="{ horizontal: true, align: 'center' }">
        <div class="flicking-panel panel-1">1</div>
        <div class="flicking-panel panel-2">2</div>
        <div class="flicking-panel panel-3">3</div>
        <div class="flicking-panel panel-4">4</div>
        <div class="flicking-panel panel-5">5</div>
      </Flicking>
    </div>

    <!-- horizontal: false (vertical) -->
    <div class="demo-container">
      <div class="demo-label">horizontal: false (vertical)</div>
      <Flicking :options="{ horizontal: false, align: 'center', panelsPerView: 3 }">
        <div class="flicking-panel panel-1">1</div>
        <div class="flicking-panel panel-2">2</div>
        <div class="flicking-panel panel-3">3</div>
        <div class="flicking-panel panel-4">4</div>
        <div class="flicking-panel panel-5">5</div>
      </Flicking>
    </div>
  </div>
</template>

<script>
import Flicking from "@egjs/vue3-flicking";
import "@egjs/vue3-flicking/dist/flicking.css";

export default {
  components: { Flicking }
};
</script>
```

### JavaScript
```js
import Flicking from "@egjs/flicking";
import "@egjs/flicking/dist/flicking.css";
import "./styles.css";

// horizontal: true (default)
new Flicking("#flick-horizontal", {
  horizontal: true,
  align: "center"
});

// horizontal: false (vertical)
new Flicking("#flick-vertical", {
  horizontal: false,
  align: "center",
  panelsPerView: 3
});
```

### HTML (for vanilla JS)
```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="/styles.css" />
</head>
<body>
  <!-- horizontal: true (default) -->
  <div class="demo-container">
    <div class="demo-label">horizontal: true (default, horizontal)</div>
    <div id="flick-horizontal" class="flicking-viewport">
      <div class="flicking-camera">
        <div class="flicking-panel panel-1">1</div>
        <div class="flicking-panel panel-2">2</div>
        <div class="flicking-panel panel-3">3</div>
        <div class="flicking-panel panel-4">4</div>
        <div class="flicking-panel panel-5">5</div>
      </div>
    </div>
  </div>

  <!-- horizontal: false (vertical) -->
  <div class="demo-container">
    <div class="demo-label">horizontal: false (vertical)</div>
    <div id="flick-vertical" class="flicking-viewport">
      <div class="flicking-camera">
        <div class="flicking-panel panel-1">1</div>
        <div class="flicking-panel panel-2">2</div>
        <div class="flicking-panel panel-3">3</div>
        <div class="flicking-panel panel-4">4</div>
        <div class="flicking-panel panel-5">5</div>
      </div>
    </div>
  </div>

</body>
</html>
```

### CSS
```css
.flicking-viewport {
  height: 200px;
}
.flicking-panel {
  width: 100%;
}
```
