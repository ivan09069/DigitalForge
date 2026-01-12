# Forge Floor

> UI doctrine and operational rules for DigitalForge systems.

---

## Zones

Every interface is divided into four functional zones.  
Nothing floats. Everything belongs.

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  ┌─────────────────┐  ┌─────────────────────────────────┐  │
│  │  FEED / INTAKE  │  │         CORE PROCESS            │  │
│  │                 │  │                                 │  │
│  │  Inputs         │  │  Live systems                   │  │
│  │  Raw data       │  │  Transformations                │  │
│  │  Queues         │  │  Outputs                        │  │
│  └─────────────────┘  └─────────────────────────────────┘  │
│                                                             │
│  ┌─────────────────┐  ┌─────────────────────────────────┐  │
│  │   HEAT / LOAD   │  │        CONTROL RAIL             │  │
│  │                 │  │                                 │  │
│  │  Stress         │  │  [ STOP ]                       │  │
│  │  Utilization    │  │  [ PAUSE ]                      │  │
│  │  Thresholds     │  │  [ RESET ]                      │  │
│  └─────────────────┘  └─────────────────────────────────┘  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

| Zone | Purpose | Systems |
|------|---------|---------|
| **Feed / Intake** | Inputs, raw data, queues | Base-Sniper |
| **Core Process** | Live transformations, outputs | SwarmSentinel-v3 |
| **Heat / Load** | Stress, utilization, thresholds | jit-command-center |
| **Control Rail** | Start, pause, kill | EchoForge-agents |

---

## State Colors

Color communicates state, never theme.  
Colors are never reused across states.

```
IDLE        ████  #164e63   dim cyan
ACTIVE      ████  #22d3ee   cyan + glow
HEATING     ████  #8b5cf6   violet
HOT         ████  #ec4899   magenta edge
CRITICAL    ████  #ef4444   red (rare)
COOLING     ████  #1e3a5f   desaturated blue
```

### Transitions

```
IDLE → ACTIVE      fade in (300ms)
ACTIVE → HEATING   gradient shift (500ms)
HEATING → HOT      pulse intensifies
HOT → CRITICAL     immediate
ANY → COOLING      slow desaturation (1000ms)
```

---

## Motion Rules

Motion is diagnostic only.

- If the system stops, motion stops
- No decorative animation
- No ambient movement
- Pulse indicates processing
- Strobe indicates critical

---

## Control Doctrine

Every live system exposes exactly three controls:

```
[ STOP ]    → Immediate halt, state preserved
[ PAUSE ]   → Suspend processing, resume possible
[ RESET ]   → Return to initial state
```

### Rules

- Controls are never hidden in menus
- Controls are physically separated
- PAUSE/STOP require no confirmation
- RESET requires single confirmation
- Controls are always in the same position

An operator in crisis hits muscle memory, not UI.

---

## Typography

- Monospace only
- Numbers over icons
- Labels are terse
- No decorative text

```css
font-family: 'JetBrains Mono', 'Fira Code', monospace;
```

---

## Layout Principles

### Density
- High information density
- No whitespace for aesthetics
- Whitespace indicates grouping only

### Hierarchy
- Position indicates importance
- Top-left: most critical
- Bottom-right: least urgent

### Boundaries
- Hard edges, not shadows
- Borders indicate zones
- Gaps indicate separation

---

## Anti-Patterns

❌ Decorative gradients  
❌ Rounded corners (except buttons)  
❌ Drop shadows  
❌ Skeleton loaders  
❌ Toast notifications  
❌ Modal dialogs for non-destructive actions  
❌ Color for branding  
❌ Animation for delight  

---

## Implementation

### CSS Variables

```css
:root {
  /* State colors */
  --state-idle: #164e63;
  --state-active: #22d3ee;
  --state-heating: #8b5cf6;
  --state-hot: #ec4899;
  --state-critical: #ef4444;
  --state-cooling: #1e3a5f;
  
  /* Surfaces */
  --surface-primary: #0a0a0a;
  --surface-secondary: #111111;
  --surface-border: #1f2937;
  
  /* Text */
  --text-primary: #e5e5e5;
  --text-secondary: #737373;
  --text-accent: #22d3ee;
}
```

### Zone Container

```html
<div class="forge-floor">
  <div class="zone zone-feed">...</div>
  <div class="zone zone-core">...</div>
  <div class="zone zone-heat">...</div>
  <div class="zone zone-control">...</div>
</div>
```

---

## Zone-Specific Rules

### Feed / Intake
- Show queue depth
- Show incoming rate
- Show filter ratio
- Expose kill switch per feed

### Core Process
- Show pipeline stages
- Show current stage highlighted
- Show throughput
- Show failure count

### Heat / Load
- Show percentage bars
- Show threshold markers
- Show trend direction
- Show time-to-threshold

### Control Rail
- Large touch targets
- Physical separation
- State-appropriate labels (PAUSE vs RESUME)
- Confirmation only for RESET

---

<p align="center"><sub>🔥 Forge Floor | DigitalForge</sub></p>
