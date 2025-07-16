# Core Principles

- Choreography = orchestrating when things happen in sequence
- Elements should move at different speeds and times, especially if conceptually distinct
- Nature doesn't move in perfect synchronicity -> neither should interfaces

# Disney's Follow Through Concept

- "Things don't come to a stop all at once guys; first there's one part and then another"
- Bodies in motion don't move as single units -> parts move at different speeds
- Secondary elements (coat, ears) continue moving after main action stops
- Software elements should behave similarly based on conceptual relationships

# Staggering Technique

## Basic Implementation

```javascript
.map((item, index) => (
  <motion.div
    key={index}
    transition={{
      delay: index * 0.05,
    }}
  />
))
```

## Benefits

- Produces visual interest with minimal complexity
- Mimics natural movement (school of fish, tree leaves)
- Communicates what's happening on screen more clearly
- Prevents jarring all-at-once appearances

## Applications

- Grid content loading (OpenAI example)
- Text animations
- List item reveals
- Secondary element fade-ins

# Staging Interactions

## Damping for Weight

```javascript
function onPan(_, { offset }) {
  const damping = 0.5;
  const dampenedY = offset.y * damping;
  y.set(dampenedY);
}
```

- Creates sensation of weight
- Prevents accidental triggers
- Makes abstract elements feel more meaningful

## Distance-Based Choreography

```javascript
function onPan(_, { offset }) {
  const damping = collapsed ? 1 : 0.5;
  const dampenedY = offset.y * damping;

  if (dampenedY > 35) {
    // 35px padding for anticipation
    setCollapsed(true);
  }
}
```

- Add snap point padding for anticipation
- Reset damping after snap for satisfying feedback
- Consider moved distance, not just time delays

# Morph Transitions

## Common Mistake

- Animating root element causes content to jarring re-center
- Instead: animate inner container, let parent resize as side effect

## Correct Structure

```html
<footer>
  <motion.div animate={{ width: bounds.width }}>
    <!-- animated content -->
  </motion.div>
  <!-- sibling elements -->
</footer>
```

# Overlapping Layers Problem

## Why It's Bad

- Produces visual clutter
- Breaks rhythm
- Looks unappealing
- Confuses users
- Like conflicting instruments in music

## Solutions

### Blur Filter

- Use 1-2px blur for subtle softening
- Makes overlapping letters blend smoothly
- Adds depth to motion
- Cleans up messy choreography

### Staggered Text Animation

```javascript
.map((letter, index) => ({
  animate: {
    opacity: 1,
    filter: "blur(0px)",
    transition: {
      delay: index * 0.01,
    }
  },
  exit: {
    transition: {
      stiffness: transition.stiffness * 2, // Faster cleanup
    },
  }
}))
```

### Faster Exit Animations

- Double exit velocity for quicker cleanup
- Gets rid of stale state faster
- More responsive interface
- First letters appear immediately (no lag feeling)

# Crossfading Icons

## Problem

- Different shapes create jarring overlaps
- Especially apparent with icon transitions

## Solution

- Scale by ~0.5 and blur by ~7px during transition
- Avoid scaling to 0 or excessive blur (creates abrupt transitions)
- Similar shapes crossfade better naturally
- Tune values based on visual size of elements

# Key Principles

- Transition immediately in response to user input
- Use choreography to avoid overlapping layers
- Nature-inspired timing feels familiar
- Combine damping with choreography for nuanced interactions
- Consider both time delays and distance thresholds
- Blur is powerful tool for cleaning up motion
- Faster exits = more responsive interfaces
