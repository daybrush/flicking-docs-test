# Optimize Size Update

Use the [`optimizeSizeUpdate`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#optimizesizeupdate) option to skip forced panel rendering (`forceRenderAllPanels`) when the size change occurs on an axis irrelevant to the Flicking direction, optimizing performance.

Try swiping through panels and compare the **flickering** difference between the two carousels.
With `optimizeSizeUpdate: false`, all 200 panels are inserted into and removed from the DOM on every height change, causing flickering.
With `true`, this process is skipped when only the height changes, resulting in smooth operation without flickering.



## Summary

### Key Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| [`optimizeSizeUpdate`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#optimizesizeupdate) | `boolean` | `false` | Skip forced panel rendering when irrelevant axis changes based on direction |

### Direction-Based Behavior

| Flicking Direction | Force Rendering Condition with optimizeSizeUpdate: true |
|--------------------|-------------------------------------------------------|
| `horizontal` (default) | Force renders all panels only when **width changes** |
| `vertical` | Force renders all panels only when **height changes** |

## Details

### Flow: When autoResize Detects a Height Change

In an `autoResize: true` + `renderOnlyVisible: true` environment, the following flow occurs when only the viewport height changes:

```
1. Viewport height changes (panel height differences, external layout changes, etc.)

2. autoResize's ResizeObserver detects the height change
   -> flicking.resize() called

3. Inside resize(), forceRenderAllPanels()
   -> All hidden panels are inserted into DOM (cameraEl.appendChild)
   -> Browser briefly renders all panels -> unnecessary DOM manipulation!

4. renderer.render()
   -> Non-visible panels removed from DOM again

With optimizeSizeUpdate: true applied:
   -> If width hasn't changed in a horizontal Flicking
   -> forceRenderAllPanels() is skipped -> DOM manipulation prevented
```

### How optimizeSizeUpdate Works

Inside Flicking's `resize()`, `forceRenderAllPanels()` is called to accurately measure all panel sizes. This method renders all panels to the DOM so their sizes can be measured.

When using [`renderOnlyVisible`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#renderonlyvisible) or virtual rendering, non-visible panels are normally removed from the DOM. During `resize()`, inserting and removing all these panels becomes increasingly expensive as the panel count grows.

With `optimizeSizeUpdate: true`, this forced rendering is skipped when only the axis irrelevant to the Flicking direction has changed.

```
resize() internal flow:

1. viewport.resize()                          <- Always executed
2. forceRenderAllPanels()                     <- Part controlled by optimizeSizeUpdate
   - false: Always renders all panels to DOM
   - true: Only renders when the relevant axis changes (horizontal->width, vertical->height)
3. updatePanelSize() -> render()               <- Always executed
```

### Prerequisites

> **Info: When This Is Effective**
This option is effective when used with **`autoResize: true`** in combination with **`renderOnlyVisible: true`** or **virtual rendering**.
In normal rendering mode, all panels are always in the DOM, so `forceRenderAllPanels()` has virtually no cost.

### Use Cases

> **Info: When should you use this?**
- **`autoResize: true` + `renderOnlyVisible: true` + many panels**: Reduces unnecessary DOM manipulation when only viewport height changes, improving performance
- **Horizontal slider inside vertical scroll**: Prevents unnecessary rendering of hidden panels on height changes from scrolling
- **Virtual rendering environment**: Prevents unnecessary render/unmount of virtual panels

### Notes

> **Warning: Caution**
- Only works when `autoResize: true`
- Has no effect without `renderOnlyVisible` or virtual rendering
- If panel size depends on the container height (e.g., `height: 100%`), using this option may prevent panel sizes from being updated

## Related Links

### Related Options
- [`optimizeSizeUpdate`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#optimizesizeupdate): Size update optimization
- [`autoResize`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#autoresize): Auto resize (dependent option)
- [`renderOnlyVisible`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#renderonlyvisible): Render only visible panels

### Related Demos
- [Auto Resize](https://daybrush.github.io/flicking-docs-test/llm-docs/demos/advanced/auto-resize.md): Resize detection method settings
- [Resize Debounce](https://daybrush.github.io/flicking-docs-test/llm-docs/demos/advanced/resize-debounce.md): Control resize call frequency
- [Render Only Visible](https://daybrush.github.io/flicking-docs-test/llm-docs/demos/advanced/render-only-visible.md): Render only visible panels

## Code

### React
```jsx
import Flicking from "@egjs/react-flicking";
import "@egjs/react-flicking/dist/flicking.css";
import "./styles.css";

const PANEL_COUNT = 200;
const PANELS = Array.from({ length: PANEL_COUNT }, (_, i) => i + 1);
const HEIGHTS = ["panel-h120", "panel-h130", "panel-h140", "panel-h150", "panel-h160"];
const HEIGHT_LABELS = [120, 130, 140, 150, 160];

export default function App() {
  return (
    <div>
      <div className="demo-hint">
        <strong>autoResize: true + renderOnlyVisible: true</strong> with 200 panels of varying heights.
        <br />
        Swipe through the panels and compare the <strong>flickering</strong> between the two carousels.
      </div>

      <div className="demo-section">
        <div className="demo-label">optimizeSizeUpdate: false (default)</div>
        <Flicking renderOnlyVisible={true} autoResize={true} optimizeSizeUpdate={false}>
          {PANELS.map(n => (
            <div key={n} className={`flicking-panel ${HEIGHTS[(n - 1) % 5]}`}>
              Panel {n} ({HEIGHT_LABELS[(n - 1) % 5]}px)
            </div>
          ))}
        </Flicking>
      </div>

      <div className="demo-section">
        <div className="demo-label">optimizeSizeUpdate: true</div>
        <Flicking renderOnlyVisible={true} autoResize={true} optimizeSizeUpdate={true}>
          {PANELS.map(n => (
            <div key={n} className={`flicking-panel ${HEIGHTS[(n - 1) % 5]}`}>
              Panel {n} ({HEIGHT_LABELS[(n - 1) % 5]}px)
            </div>
          ))}
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
    <div class="demo-hint">
      <strong>autoResize: true + renderOnlyVisible: true</strong> with
      200 panels of varying heights.<br/>
      Swipe through the panels and compare the <strong>flickering</strong> between the two carousels.
    </div>

    <div class="demo-section">
      <div class="demo-label">optimizeSizeUpdate: false (default)</div>
      <Flicking
        :renderOnlyVisible="true"
        :autoResize="true"
        :optimizeSizeUpdate="false"
      >
        <div
          v-for="n in 200"
          :key="n"
          :class="'flicking-panel ' + heights[(n - 1) % 5]"
        >
          Panel {{ n }} ({{ heightLabels[(n - 1) % 5] }}px)
        </div>
      </Flicking>
    </div>

    <div class="demo-section">
      <div class="demo-label">optimizeSizeUpdate: true</div>
      <Flicking
        :renderOnlyVisible="true"
        :autoResize="true"
        :optimizeSizeUpdate="true"
      >
        <div
          v-for="n in 200"
          :key="n"
          :class="'flicking-panel ' + heights[(n - 1) % 5]"
        >
          Panel {{ n }} ({{ heightLabels[(n - 1) % 5] }}px)
        </div>
      </Flicking>
    </div>
  </div>
</template>

<script>
import Flicking from "@egjs/vue3-flicking";
import "@egjs/vue3-flicking/dist/flicking.css";

export default {
  components: { Flicking },
  data() {
    return {
      heights: ["panel-h120", "panel-h130", "panel-h140", "panel-h150", "panel-h160"],
      heightLabels: [120, 130, 140, 150, 160]
    };
  }
};
</script>
```

### JavaScript
```js
import Flicking from "@egjs/flicking";
import "@egjs/flicking/dist/flicking.css";
import "./styles.css";

const PANEL_COUNT = 200;
const HEIGHTS = ["panel-h120", "panel-h130", "panel-h140", "panel-h150", "panel-h160"];
const HEIGHT_LABELS = [120, 130, 140, 150, 160];

function createPanels(cameraEl) {
  for (let i = 0; i < PANEL_COUNT; i++) {
    const panel = document.createElement("div");
    panel.className = `flicking-panel ${HEIGHTS[i % 5]}`;
    panel.textContent = `Panel ${i + 1} (${HEIGHT_LABELS[i % 5]}px)`;
    cameraEl.appendChild(panel);
  }
}

createPanels(document.querySelector("#flick-off .flicking-camera"));
createPanels(document.querySelector("#flick-on .flicking-camera"));

new Flicking("#flick-off", {
  renderOnlyVisible: true,
  autoResize: true,
  optimizeSizeUpdate: false
});

new Flicking("#flick-on", {
  renderOnlyVisible: true,
  autoResize: true,
  optimizeSizeUpdate: true
});
```

### HTML (for vanilla JS)
```html
<div class="demo-hint">
  <strong>autoResize: true + renderOnlyVisible: true</strong> with
  200 panels of varying heights.<br/>
  Swipe through the panels and compare the <strong>flickering</strong> between the two carousels.
</div>

<div class="demo-section">
  <div class="demo-label">optimizeSizeUpdate: false (default)</div>
  <div id="flick-off" class="flicking-viewport">
    <div class="flicking-camera"></div>
  </div>
</div>

<div class="demo-section">
  <div class="demo-label">optimizeSizeUpdate: true</div>
  <div id="flick-on" class="flicking-viewport">
    <div class="flicking-camera"></div>
  </div>
</div>
```

### CSS
```css
.demo-section {
  margin-bottom: 16px;
}

.demo-section .demo-label {
  font-weight: bold;
  margin-bottom: 6px;
  font-size: 14px;
}

.demo-hint {
  font-size: 13px;
  color: #888;
  margin-bottom: 12px;
}

.flicking-panel {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
}

/* Cycle through 5 different heights */
.panel-h120 {
  background: #3e8ed0;
  height: 120px;
}
.panel-h130 {
  background: #00d1b2;
  height: 130px;
}
.panel-h140 {
  background: #f14668;
  height: 140px;
}
.panel-h150 {
  background: #ffe08a;
  color: #333;
  height: 150px;
}
.panel-h160 {
  background: #48c78e;
  height: 160px;
}
```
