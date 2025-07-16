# Core Concept

- "Contained gestures" = isolated gestures that don't trigger surrounding elements or other gestures by accident
- Essential for professional-feeling interactions

# Drag Selection Prevention

## Problems to Solve

- Dragging pointer beyond target bounds accidentally selects text
- Cursor changes to whatever it's hovering (text cursor, etc.)
- Should maintain consistent grabbing cursor throughout gesture

## Solution: Disable Pointer Events

```css
.gesture-grabbing {
  cursor: grabbing;
  user-select: none;
  -webkit-user-select: none; /* Safari compatibility */

  * {
    pointer-events: none;
    user-select: none;
    -webkit-user-select: none;
  }
}
```

## Utility Pattern

```javascript
export const grab = {
  start: () => document.body.classList.add("cursor-grabbing"),
  end: () => document.body.classList.remove("cursor-grabbing"),
};

function onPanStart() {
  grab.start();
  // ...
}

function onPanEnd() {
  grab.end();
  // ...
}
```

# Touch Action Property

## Problem

- Browser tries to scroll when custom touch interactions conflict with system gestures
- Without `touch-action`, dragging triggers page scrolling

## Solution

```css
/* Disable all touch behaviors */
touch-action: none;

/* Specific disabling */
touch-action: pan-y; /* Disable vertical scroll, allow horizontal */
```

## Best Practice

- Be specific about which native behaviors to disable
- Apply to elements with custom interactions

# Gesture Conflicts

## Click vs Drag Problem

- Drag events trigger both `pointerdown` and `pointerup`
- `pointerup` fires click event as side effect
- Need to cancel click when dragging occurred

## State Management Solution

```javascript
// Track gesture state
function onPointerMove(e) {
  if (state.current === "press") {
    state.current = "drag";
  }
}

function onPointerUp(e) {
  if (state.current === "drag") {
    state.current = "drag-end";
  }
}

// Cancel conflicting click
function onClick(e) {
  if (state.current === "drag-end") {
    e.preventDefault();
    e.stopPropagation();
  }
}
```

# Drag Threshold

## Purpose

- Prevent accidental drag triggers from imperfect motor skills
- Allow small mouse movements during intended clicks
- Common in professional tools (Figma has pixel threshold)

## Implementation

```javascript
function onPointerMove(e) {
  const threshold = 10;

  if (state.current === "press") {
    const dx = e.clientX - origin.current.x;
    const dy = e.clientY - origin.current.y;
    const distance = Math.sqrt(dx * dx + dy * dy);

    if (distance >= threshold) {
      state.current = "drag";
    }
  }
}
```

## Guidelines

- Few pixels usually sufficient
- Adjust based on target size
- Doesn't feel slow when deliberately dragging

# Pointer Capture

## Problem

- Dragging from edge fails when pointer leaves target area
- Drag stops completely when pointer moves off element

## Solution: setPointerCapture API

```javascript
function onPointerDown(e) {
  ref.current?.setPointerCapture(e.pointerId);
}

function onPointerUp(e) {
  ref.current?.releasePointerCapture(e.pointerId);
}
```

## Benefits

- Events stay bound to target element even when pointer moves outside
- Works from edges and continues beyond bounds
- Essential for smooth dragging experience

# Key Implementation Checklist

- [ ] Disable pointer events during gesture lifecycle
- [ ] Set appropriate `touch-action` value
- [ ] Implement gesture conflict resolution
- [ ] Add drag threshold for accidental movement
- [ ] Use pointer capture for edge cases
- [ ] Maintain consistent cursor throughout gesture

# When to Use Libraries vs Custom

- **Use libraries** (`@use-gesture/react`) for most cases -> they handle edge cases
- **Custom implementation** when external dependencies don't make sense (Next.js Dev Tools example)
- Understanding library internals helps with custom implementations
