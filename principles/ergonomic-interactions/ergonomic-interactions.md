# Core Principles

**Affordance vs Ergonomics**

- Affordance = how obvious it is to use something before touching it
- Ergonomics = how easy it is to actually use once you're interacting
- Both matter more than fancy animations

**The Usability vs Aesthetics Tension**

- There's always a trade-off between making things beautiful and making them usable
- Consider your audience -> design nerds might figure out subtle interactions, general users need more obvious cues
- The best designs elegantly solve both

# Specific Patterns

**Form Interactions**

- Always let users submit forms and get errors rather than disabling buttons
- Much easier to see what's wrong when you can actually try to submit
- Exception: single field forms where it's obvious

**Input Field Focus**

- Clicking icons next to inputs should focus the input field
- Wrap with `<label>` element to make the whole thing clickable
- Use `focus-within` for styling the container, not just the input

**Scroll Affordances**

- Add left margins to indicate content continues off-screen
- Shows where content begins and hints at more content to the right

**Drag Interactions**

- Use visual handles to indicate draggable surfaces
- Critical: make hit areas bigger than visual areas (invisible padding)
- Users shouldn't have to be pixel-perfect to grab things

# Advanced Concepts

**Focus Management**

- Never just remove focus rings -> customize them instead
- Manually transfer focus to overlays/dialogs when they open
- Return focus to trigger when closing

**Touch Gestures**

- Always provide two ways: visual button + gesture
- Helps discoverability since gestures are invisible
- Users learn gestures as shortcuts to visible actions

**Fitts' Law Applications**

- Make interactive elements close to where the pointer already is
- Rauno's nav example: clicking shifts page so mouse is already on next item
- Mobile: put important actions near thumb reach
