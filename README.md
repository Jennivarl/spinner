# GenLayer Consensus Spinner

A loading spinner for the GenLayer Portal, shaped like the thing it is waiting on.

**Live demo:** https://jennivarl.github.io/genlayer-spinner/

Five validators start out of step, converge, agree for a beat, then begin the next round. It is a GenLayer round, running as a loading state.

## Why this and not a rotating arc

A generic spinner would work anywhere and mean nothing. GenLayer's defining mechanic is five validators executing independently and then reaching agreement, so the spinner is built from that:

- **Five nodes**, because that is the size of a GenLayer round, not an arbitrary count that happened to look balanced.
- **Out of phase at the start of every cycle**, because independent execution is what happens before consensus. The stagger is the message.
- **It never resolves and stops.** A transaction that fails consensus rotates its leader and runs again. A spinner that settled once would be telling the wrong story about what the user is waiting for.

## Using it

```html
<link rel="stylesheet" href="spinner.css">

<div class="gl-spinner" role="status" aria-label="Loading">
  <span></span><span></span><span></span><span></span><span></span>
</div>
```

Size and colour are custom properties, so it takes a theme without editing the stylesheet:

```html
<div class="gl-spinner" style="--gl-spinner-size: 32px; --gl-spinner-color: #fff">
```

| Property | Default | Purpose |
|---|---|---|
| `--gl-spinner-size` | `48px` | Everything else scales from this |
| `--gl-spinner-color` | `#6d3bf5` | Node colour |
| `--gl-spinner-duration` | `2.4s` | One full round |

## Details that matter in a real UI

**It stays on the compositor.** Only `transform` and `opacity` animate, and each node composes its orbit and scale into a single transform, so there is no layout or paint work while the page is already busy loading. A spinner that causes jank during a load is worse than no spinner.

**It respects `prefers-reduced-motion`.** Rather than freezing, which reads as a hung page, the nodes settle onto the ring and pulse together. It still says "working" without the orbit.

**It is announced.** Every instance carries `role="status"` and a label, so a screen reader says something instead of nothing.

**It holds up small.** Verified at 16px, where the node count still reads and does not smear into a grey ring.

## Files

| File | What it is |
|---|---|
| `spinner.css` | The spinner. This is the deliverable. |
| `index.html` | The demo page, showing it at real UI sizes and inside a loading card. |

No build step and no dependencies. The live demo is GitHub Pages serving this repository directly, so the deployed page and the source here are the same files.

## Licence

MIT. Free for the Portal or anyone else to use.
