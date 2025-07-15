# Core Principles

- Great interactions simulate real-world physics
- Digital movement should have momentum, weight, and natural responses
- Match animation energy to gesture energy

# Spring Motion

## Why Springs Over Easing

- Traditional easing feels robotic for physical movements
- Springs use physical properties (stiffness, damping, mass) vs fixed duration
- **Key advantage**: Fluidly interruptible -> can smoothly change direction mid-animation

## Spring Configuration Mental Model

- **Higher stiffness** = faster movement
- **Lower damping** = more bounce/overshoot
- **Mass** = usually makes interactions feel sluggish (avoid)
- No universal config -> tune per animation

## When to Use Bounce

- **With momentum**: swipe gestures, throws, velocity-based interactions
- **Without momentum**: avoid for hover, simple press, static interactions
- Consider gesture velocity when determining bounce amount

## Example Configs

```javascript
// Bouncy (for momentum-based interactions)
{ stiffness: 250, damping: 10 }

// Controlled (for precise interactions)
{ stiffness: 250, damping: 25 }
```

# Conceptual Weight

## Weight Principles

- Heavy objects (images, documents) = more resistance/friction
- Light objects (cursors, small UI) = snappy response
- Destructive actions = heavy feel to prevent accidents

## Applications

- Clipboard following cursor: add slight spring resistance
- Delete actions: require hold buttons, confirmation dialogs
- File dragging: match weight to conceptual size/importance

# Advanced Techniques

## Damping/Rubber Banding

- Purpose: communicate boundaries without hard stops
- Keeps interface always responsive to input
- Implementation: square root function for gradual resistance beyond bounds

```javascript
function dampen(val, [min, max], factor = 2) {
  if (val > max) {
    const extra = val - max;
    const dampenedExtra = Math.sqrt(extra);
    return max + dampenedExtra * factor;
  }
  // ... similar for min
}
```

## Velocity-Based Interactions

- Swipes = drag + velocity calculation
- Project landing position based on throw speed
- Creates effortless, natural gesture feel

# Interaction Metaphors

- Cross off todo items: swift stroke with momentum/bounce
- Screen edge collision: elements rebound like bouncing ball
- Physical button bridge: elastic feedback connects physical press to digital response
