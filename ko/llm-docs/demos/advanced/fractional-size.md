# Fractional Size

Use the [`useFractionalSize`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#usefractionalsize) option to prevent 1px misalignment errors in panels with fractional sizes.



## Summary

### Key Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| [`useFractionalSize`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#usefractionalsize) | `boolean` | `false` | Calculate sizes with fractional precision |

### Mode Comparison

| Setting | Size Measurement Method | Precision | Performance |
|---------|------------------------|-----------|-------------|
| `false` (default) | `offsetWidth` | Integer (rounded) | Fast |
| `true` | `getBoundingClientRect` | Fractional | Slightly slower |

## Details

### The 1px Misalignment Problem

When panel width is fractional, alignment errors can occur:

```
Viewport: 300px
Panel width: 33.33% = 99.99px

When using offsetWidth:
- Panel 1: 100px (rounded)
- Panel 2: 100px
- Panel 3: 100px
- Total: 300px (0.03px larger than actual)
```

When this error accumulates, the alignment of the last panel can be off.

### How It Works

```javascript
// false (default): uses offsetWidth
const width = panel.offsetWidth; // 100 (integer)

// true: uses getBoundingClientRect
const width = panel.getBoundingClientRect().width; // 99.99 (fractional)
```

Setting `useFractionalSize: true` makes Flicking internally calculate sizes with fractional precision.

### Use Cases

> **Info: When should you use useFractionalSize?**

**Recommended:**
- When panel width is set in % units (33.33%, 16.66%, etc.)
- When panel alignment is slightly off
- When precise rendering is needed on high-resolution displays

**Not necessary:**
- When panel width is an integer px (200px, 300px, etc.)
- When minor alignment errors are not a concern
- When performance is a priority

### Notes

> **Warning: Performance Consideration**
`getBoundingClientRect()` is slightly slower than `offsetWidth`. Consider the performance impact when there are many panels or frequent resizes occur.

## Related Links

### Related Options
- [`autoResize`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#autoresize): Auto resize
- [`useResizeObserver`](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/FlickingOptions.md#useresizeobserver): Whether to use ResizeObserver

## Code

### React
```jsx
import Flicking from "@egjs/react-flicking";
import { useEffect, useRef, useState } from "react";
import "@egjs/react-flicking/dist/flicking.css";
import "./styles.css";

const COLORS = ["#3e8ed0", "#00d1b2", "#f14668", "#ffe08a", "#48c78e", "#9c27b0"];

export default function App() {
  const fractionalRef = useRef(null);
  const integerRef = useRef(null);
  const [fractionalSize, setFractionalSize] = useState("");
  const [integerSize, setIntegerSize] = useState("");

  const updateSizes = () => {
    if (fractionalRef.current) {
      const panel = fractionalRef.current.element.querySelector(".flicking-panel");
      if (panel) {
        const rect = panel.getBoundingClientRect();
        const offset = panel.offsetWidth;
        setFractionalSize(`getBoundingClientRect: ${rect.width.toFixed(2)}px / offsetWidth: ${offset}px`);
      }
    }
    if (integerRef.current) {
      const panel = integerRef.current.element.querySelector(".flicking-panel");
      if (panel) {
        const rect = panel.getBoundingClientRect();
        const offset = panel.offsetWidth;
        setIntegerSize(`getBoundingClientRect: ${rect.width.toFixed(2)}px / offsetWidth: ${offset}px`);
      }
    }
  };

  useEffect(() => {
    // Measure sizes after a small delay (wait for render)
    const timer = setTimeout(updateSizes, 100);
    return () => clearTimeout(timer);
  }, [updateSizes]);

  const panels = [0, 1, 2, 3, 4, 5];

  return (
    <div>
      {/* useFractionalSize: true */}
      <div className="demo-container">
        <div className="demo-label">useFractionalSize: true</div>
        <div className="demo-info">Uses getBoundingClientRect (sub-pixel precision)</div>
        <Flicking ref={fractionalRef} align="prev" useFractionalSize={true} onReady={updateSizes}>
          {panels.map(i => (
            <div key={i} className="flicking-panel fractional-panel" style={{ background: COLORS[i % COLORS.length] }}>
              Panel {i + 1}
            </div>
          ))}
        </Flicking>
        <div className="size-display">
          Panel size: <strong>{fractionalSize}</strong>
        </div>
      </div>

      {/* useFractionalSize: false (default) */}
      <div className="demo-container">
        <div className="demo-label">useFractionalSize: false (default)</div>
        <div className="demo-info">Uses offsetWidth (integer rounding)</div>
        <Flicking ref={integerRef} align="prev" useFractionalSize={false} onReady={updateSizes}>
          {panels.map(i => (
            <div key={i} className="flicking-panel fractional-panel" style={{ background: COLORS[i % COLORS.length] }}>
              Panel {i + 1}
            </div>
          ))}
        </Flicking>
        <div className="size-display">
          Panel size: <strong>{integerSize}</strong>
        </div>
      </div>
    </div>
  );
}
```

### Vue3
```vue
<template>
  <div>
    <!-- useFractionalSize: true -->
    <div class="demo-container">
      <div class="demo-label">useFractionalSize: true</div>
      <div class="demo-info">Uses getBoundingClientRect (sub-pixel precision)</div>
      <Flicking
        ref="fractionalFlicking"
        align="prev"
        :useFractionalSize="true"
        @ready="updateSizes"
      >
        <div
          v-for="i in 6"
          :key="'frac-' + i"
          class="flicking-panel fractional-panel"
          :style="{ background: colors[(i - 1) % colors.length] }"
        >
          Panel {{ i }}
        </div>
      </Flicking>
      <div class="size-display">
        Panel size: <strong>{{ fractionalSize }}</strong>
      </div>
    </div>

    <!-- useFractionalSize: false (default) -->
    <div class="demo-container">
      <div class="demo-label">useFractionalSize: false (default)</div>
      <div class="demo-info">Uses offsetWidth (integer rounding)</div>
      <Flicking
        ref="integerFlicking"
        align="prev"
        :useFractionalSize="false"
        @ready="updateSizes"
      >
        <div
          v-for="i in 6"
          :key="'int-' + i"
          class="flicking-panel fractional-panel"
          :style="{ background: colors[(i - 1) % colors.length] }"
        >
          Panel {{ i }}
        </div>
      </Flicking>
      <div class="size-display">
        Panel size: <strong>{{ integerSize }}</strong>
      </div>
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
      colors: ["#3e8ed0", "#00d1b2", "#f14668", "#ffe08a", "#48c78e", "#9c27b0"],
      fractionalSize: "",
      integerSize: ""
    };
  },
  methods: {
    updateSizes() {
      this.$nextTick(() => {
        if (this.$refs.fractionalFlicking) {
          const panel = this.$refs.fractionalFlicking.$el.querySelector(".flicking-panel");
          if (panel) {
            const rect = panel.getBoundingClientRect();
            const offset = panel.offsetWidth;
            this.fractionalSize = `getBoundingClientRect: ${rect.width.toFixed(2)}px / offsetWidth: ${offset}px`;
          }
        }
        if (this.$refs.integerFlicking) {
          const panel = this.$refs.integerFlicking.$el.querySelector(".flicking-panel");
          if (panel) {
            const rect = panel.getBoundingClientRect();
            const offset = panel.offsetWidth;
            this.integerSize = `getBoundingClientRect: ${rect.width.toFixed(2)}px / offsetWidth: ${offset}px`;
          }
        }
      });
    }
  }
};
</script>
```

### JavaScript
```js
import Flicking from "@egjs/flicking";
import "@egjs/flicking/dist/flicking.css";
import "./styles.css";

// useFractionalSize: true
const fractionalFlicking = new Flicking("#flick-fractional", {
  align: "prev",
  useFractionalSize: true
});

// useFractionalSize: false (default)
const integerFlicking = new Flicking("#flick-integer", {
  align: "prev",
  useFractionalSize: false
});

// Display panel sizes
function updateSizes() {
  const fracPanel = document.querySelector("#flick-fractional .flicking-panel");
  const intPanel = document.querySelector("#flick-integer .flicking-panel");

  if (fracPanel) {
    const rect = fracPanel.getBoundingClientRect();
    const offset = fracPanel.offsetWidth;
    document.getElementById("frac-size").textContent =
      `getBoundingClientRect: ${rect.width.toFixed(2)}px / offsetWidth: ${offset}px`;
  }

  if (intPanel) {
    const rect = intPanel.getBoundingClientRect();
    const offset = intPanel.offsetWidth;
    document.getElementById("int-size").textContent =
      `getBoundingClientRect: ${rect.width.toFixed(2)}px / offsetWidth: ${offset}px`;
  }
}

fractionalFlicking.on("ready", updateSizes);
integerFlicking.on("ready", updateSizes);
```

### HTML (for vanilla JS)
```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="/styles.css" />
</head>
<body>
  <!-- useFractionalSize: true -->
  <div class="demo-container">
    <div class="demo-label">useFractionalSize: true</div>
    <div class="demo-info">Uses getBoundingClientRect (sub-pixel precision)</div>
    <div id="flick-fractional" class="flicking-viewport">
      <div class="flicking-camera">
        <div class="flicking-panel fractional-panel" style="background: #3e8ed0">Panel 1</div>
        <div class="flicking-panel fractional-panel" style="background: #00d1b2">Panel 2</div>
        <div class="flicking-panel fractional-panel" style="background: #f14668">Panel 3</div>
        <div class="flicking-panel fractional-panel" style="background: #ffe08a">Panel 4</div>
        <div class="flicking-panel fractional-panel" style="background: #48c78e">Panel 5</div>
        <div class="flicking-panel fractional-panel" style="background: #9c27b0">Panel 6</div>
      </div>
    </div>
    <div class="size-display">
      Panel size: <strong id="frac-size">Measuring...</strong>
    </div>
  </div>

  <!-- useFractionalSize: false (default) -->
  <div class="demo-container">
    <div class="demo-label">useFractionalSize: false (default)</div>
    <div class="demo-info">Uses offsetWidth (integer rounding)</div>
    <div id="flick-integer" class="flicking-viewport">
      <div class="flicking-camera">
        <div class="flicking-panel fractional-panel" style="background: #3e8ed0">Panel 1</div>
        <div class="flicking-panel fractional-panel" style="background: #00d1b2">Panel 2</div>
        <div class="flicking-panel fractional-panel" style="background: #f14668">Panel 3</div>
        <div class="flicking-panel fractional-panel" style="background: #ffe08a">Panel 4</div>
        <div class="flicking-panel fractional-panel" style="background: #48c78e">Panel 5</div>
        <div class="flicking-panel fractional-panel" style="background: #9c27b0">Panel 6</div>
      </div>
    </div>
    <div class="size-display">
      Panel size: <strong id="int-size">Measuring...</strong>
    </div>
  </div>

</body>
</html>
```

### CSS
```css
.flicking-viewport {
  margin-bottom: 8px;
}

.flicking-panel {
  height: 120px;
  margin-right: 10px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  font-weight: bold;
  color: white;
}

/* Settings that cause fractional widths */
.fractional-panel {
  width: 33.33%;
  box-sizing: border-box;
}

.demo-container {
  margin-bottom: 32px;
}

.demo-info {
  font-size: 14px;
  color: #888;
  margin-bottom: 12px;
}
.size-display {
  margin-top: 8px;
  padding: 8px 12px;
  background: #f5f5f5;
  border-radius: 4px;
  font-size: 13px;
  color: #333;
  font-family: monospace;
}
.size-display strong {
  color: #3e8ed0;
}
```
