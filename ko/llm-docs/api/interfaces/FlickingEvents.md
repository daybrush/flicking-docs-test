# FlickingEvents

> Events of the Flicking component.

## Properties

### afterResize

**Type:** `AfterResizeEvent`

Event that fires when Flicking's [resize](https://daybrush.github.io/flicking-docs-test/llm-docs/api/classes/Flicking.md#resize) is called, after updating the sizes of panels and viewport.

**Remarks:** See [AfterResizeEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/AfterResizeEvent.md) for more details.

### beforeResize

**Type:** `BeforeResizeEvent`

Event that fires when Flicking's [resize](https://daybrush.github.io/flicking-docs-test/llm-docs/api/classes/Flicking.md#resize) is called, before updating the sizes of panels and viewport.

**Remarks:** See [BeforeResizeEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/BeforeResizeEvent.md) for more details.

### changed

**Type:** `ChangedEvent`

Event that fires AFTER the active panel change completes.

**Remarks:** See [ChangedEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/ChangedEvent.md) for more details.

### holdEnd

**Type:** `HoldEndEvent`

Event that fires when user stopped dragging.

**Remarks:** See [HoldEndEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/HoldEndEvent.md) for more details.

### holdStart

**Type:** `HoldStartEvent`

Event that fires when user started dragging.

**Remarks:** See [HoldStartEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/HoldStartEvent.md) for more details.

### move

**Type:** `MoveEvent`

Event that fires for every movement.

**Remarks:** See [MoveEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/MoveEvent.md) for more details.

### moveEnd

**Type:** `MoveEndEvent`

Event that fires when the movement is finished by user input release or animation end.

**Remarks:** See [MoveEndEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/MoveEndEvent.md) for more details.

### moveStart

**Type:** `MoveStartEvent`

Event that fires once before first Flicking.event:move | move event.

**Remarks:** See [MoveStartEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/MoveStartEvent.md) for more details.

### needPanel

**Type:** `NeedPanelEvent`

Event that fires when an empty panel area is visible at the edge of viewport.

**Remarks:** See [NeedPanelEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/NeedPanelEvent.md) for more details.

### panelChange

since v4.1.0

**Type:** `PanelChangeEvent`

Event that fires when a panel is added or removed.

**Remarks:** See [PanelChangeEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/PanelChangeEvent.md) for more details.

### reachEdge

**Type:** `ReachEdgeEvent`

Event that fires when camera reaches the maximum/minimum range.

**Remarks:** See [ReachEdgeEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/ReachEdgeEvent.md) for more details.

### ready

**Type:** `ReadyEvent`

Event that fires when Flicking's [init()](https://daybrush.github.io/flicking-docs-test/llm-docs/api/classes/Flicking.md#init) is called.

**Remarks:** See [ReadyEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/types/ReadyEvent.md) for more details.

### restored

**Type:** `RestoredEvent`

Event that fires AFTER Flicking has returned to [currentPanel](https://daybrush.github.io/flicking-docs-test/llm-docs/api/classes/Flicking.md#currentpanel).

**Remarks:** See [RestoredEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/RestoredEvent.md) for more details.

### select

**Type:** `SelectEvent`

Event that fires when panel is statically click / touched.

**Remarks:** See [SelectEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/SelectEvent.md) for more details.

### visibleChange

**Type:** `VisibleChangeEvent`

Event that fires when visible panel inside the viewport changes.

**Remarks:** See [VisibleChangeEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/VisibleChangeEvent.md) for more details.

### willChange

**Type:** `WillChangeEvent`

Event that fires BEFORE the active panel changes.

**Remarks:** See [WillChangeEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/WillChangeEvent.md) for more details.

### willRestore

**Type:** `WillRestoreEvent`

Event fires BEFORE returning to the current panel when drag doesn't reach threshold.

**Remarks:** See [WillRestoreEvent](https://daybrush.github.io/flicking-docs-test/llm-docs/api/interfaces/WillRestoreEvent.md) for more details.
