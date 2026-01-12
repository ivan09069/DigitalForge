# Signal: Drift Detection

> WIP - Concept only

---

## Status: 🔧 Forging

---

## Concept

Drift signal activates when a metric is trending toward its threshold.

```
     threshold (85%)
         │
         │    ╭─── drift signal appears here
         │   ╭╯
         │  ╭╯
         ▼ ╭╯
─────────●───────────────────────────
        ╱
       ╱  current: 72%
      ╱   trending: +2%/min
     ╱    time to threshold: ~6min
────╱────────────────────────────────
```

---

## Trigger Logic

```javascript
// Pseudocode - not tested

function checkDrift(metric) {
  const distance = metric.threshold - metric.value;
  const percentToThreshold = distance / metric.threshold;
  
  // Trigger at 85% of way to threshold
  if (percentToThreshold < 0.15 && metric.trend > 0) {
    return {
      active: true,
      intensity: 1 - (percentToThreshold / 0.15),
      eta: distance / metric.trend  // minutes
    };
  }
  
  return { active: false };
}
```

---

## Visual Spec

```css
/* Not tested */

.signal-drift {
  position: absolute;
  inset: 0;
  pointer-events: none;
  border: 2px solid transparent;
  border-image: linear-gradient(
    90deg, 
    #8b5cf6, 
    #ec4899
  ) 1;
  opacity: 0;
  transition: opacity 800ms;
}

.signal-drift.active {
  opacity: var(--intensity, 0.5);
  animation: drift-pulse 2s ease-in-out infinite;
}

@keyframes drift-pulse {
  0%, 100% { border-width: 2px; }
  50% { border-width: 3px; }
}
```

---

## Questions

- Should intensity scale linearly or exponentially?
- What's the minimum display time before fade?
- How to handle multiple simultaneous drift signals?

---

## Notes

```
2026-01-05: Concept documented
            Logic sketched
            Visual spec drafted
            Needs testing
```

---

<p align="center"><sub>🔧 Forging | DigitalForge</sub></p>
