# Signals

> Predictive Signal Layer (PSL) specification.

---

## Purpose

The Predictive Signal Layer answers one question:

> **"Where should I look next?"**

It does not:
- Tell operators what to do
- Issue commands
- Block interaction
- Create anxiety

It does:
- Guide attention
- Surface pre-threshold conditions
- Fade gracefully
- Respect operator autonomy

---

## Signal Types

### Drift
Something is moving toward a threshold.

```
Trigger:   metric > threshold × 0.85
Visual:    Slow pulse at edge of relevant panel
Color:     Violet → Magenta gradient
Fade:      When trend reverses or threshold addressed
```

### Correlation
Two or more metrics are moving together unusually.

```
Trigger:   Correlation coefficient > 0.8 over window
Visual:    Subtle connector line between elements
Color:     Cyan (informational)
Fade:      When correlation breaks
```

### Anomaly
Pattern deviation detected.

```
Trigger:   Value outside 2σ of rolling baseline
Visual:    Soft glow around anomalous element
Color:     Amber
Fade:      After acknowledgment or normalization
```

### Cooldown
System returning to baseline after stress.

```
Trigger:   Metric decreasing after spike
Visual:    Desaturation gradient
Color:     Blue → Dim
Fade:      When baseline reached
```

---

## Signal Rules

### Never Block
Signals appear in peripheral vision.

```
❌ Cover content
❌ Require dismissal
❌ Pause workflows
❌ Demand acknowledgment
```

### Never Text
Signals are purely visual.  
No labels. No tooltips. No explanations.

If an operator needs to read to understand, the signal has failed.

### Never Escalate
Signals do not become alerts.  
They exist in a separate layer.

```
┌─────────────────────────────────┐
│  ALERT LAYER (rare, explicit)   │
├─────────────────────────────────┤
│  SIGNAL LAYER (continuous)      │  ← PSL
├─────────────────────────────────┤
│  CONTENT LAYER (data, controls) │
└─────────────────────────────────┘
```

### Always Fade
Signals have a natural lifespan:
- When condition resolves
- After maximum duration (30s default)
- When higher-priority signal appears

---

## Implementation

### CSS

```css
:root {
  --psl-drift: linear-gradient(90deg, #8b5cf6, #ec4899);
  --psl-correlation: #22d3ee;
  --psl-anomaly: #f59e0b;
  --psl-cooldown: #3b82f6;
  
  --psl-fade-in: 800ms;
  --psl-fade-out: 1200ms;
  --psl-pulse: 2s;
  --psl-max-duration: 30s;
}

.psl-signal {
  position: absolute;
  pointer-events: none;
  opacity: 0;
  transition: opacity var(--psl-fade-in) ease-out;
}

.psl-signal.active {
  opacity: 1;
}

.psl-drift {
  background: var(--psl-drift);
  animation: pulse var(--psl-pulse) ease-in-out infinite;
}
```

### JavaScript

```javascript
const psl = {
  emit(type, target, intensity = 1) {
    const signal = document.createElement('div');
    signal.className = `psl-signal psl-${type}`;
    signal.dataset.target = target;
    signal.style.setProperty('--intensity', intensity);
    
    const targetEl = document.querySelector(`[data-psl="${target}"]`);
    targetEl?.appendChild(signal);
    
    requestAnimationFrame(() => signal.classList.add('active'));
    
    // Auto-fade
    setTimeout(() => this.fade(type, target), 30000);
  },
  
  fade(type, target) {
    const signal = document.querySelector(
      `.psl-signal.psl-${type}[data-target="${target}"]`
    );
    if (signal) {
      signal.classList.remove('active');
      setTimeout(() => signal.remove(), 1200);
    }
  }
};
```

### Usage

```javascript
// Drift signal when approaching threshold
if (cpu.value > cpu.threshold * 0.85) {
  const intensity = (cpu.value - cpu.threshold * 0.85) / (cpu.threshold * 0.15);
  psl.emit('drift', 'cpu-panel', intensity);
}

// Clear when condition resolves
if (cpu.value < cpu.threshold * 0.80) {
  psl.fade('drift', 'cpu-panel');
}
```

---

## Calibration

| Metric | Target |
|--------|--------|
| Time to notice | < 3 seconds |
| False positive rate | < 5% |
| Signal fatigue reports | 0 per week |
| Operator trust score | > 8/10 |

---

## Anti-Patterns

❌ Signal soup (too many simultaneous)  
❌ Sticky signals (don't fade)  
❌ Text labels  
❌ Required actions  
❌ Escalation to alerts  
❌ Blocking behavior  

---

## Philosophy

The best signal is one the operator acts on without consciously noticing.

It should feel like intuition, not notification.

---

<p align="center"><sub>🔥 Signals | DigitalForge</sub></p>
