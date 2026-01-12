# UI Zones

> Feed / Core / Heat / Control

---

## Overview

Every DigitalForge interface is divided into four functional zones.  
Each zone has a single responsibility.  
Nothing floats. Everything belongs.

```
┌───────────────────┬─────────────────────────────────────┐
│                   │                                     │
│   FEED / INTAKE   │           CORE PROCESS              │
│                   │                                     │
│   Data enters     │   Data transforms                   │
│                   │                                     │
├───────────────────┼─────────────────────────────────────┤
│                   │                                     │
│   HEAT / LOAD     │          CONTROL RAIL               │
│                   │                                     │
│   Stress visible  │   Actions available                 │
│                   │                                     │
└───────────────────┴─────────────────────────────────────┘
```

---

## Zone 1: Feed / Intake

**Purpose:** Data enters the system.

### Responsibilities
- Display incoming data streams
- Show queue depths
- Show filter ratios
- Expose source controls

### Visual Elements
```
┌─────────────────────────────────────┐
│ FEED / INTAKE                       │
├─────────────────────────────────────┤
│                                     │
│ source-a    ████████░░  1.2k/sec   │
│ source-b    ██████░░░░  847/sec    │
│ source-c    ██░░░░░░░░  203/sec    │
│                                     │
│ QUEUE: 2,847 items                  │
│ FILTERED: 67% noise removed         │
│                                     │
│ [ THROTTLE ] [ PAUSE ] [ CUT ]     │
└─────────────────────────────────────┘
```

### Controls
- **THROTTLE**: Reduce intake rate
- **PAUSE**: Stop intake, preserve queue
- **CUT**: Kill feed entirely

### Metrics
- Items per second (per source)
- Queue depth
- Filter ratio (signal/noise)
- Source health

---

## Zone 2: Core Process

**Purpose:** Data transforms into output.

### Responsibilities
- Display pipeline stages
- Show current processing
- Show throughput
- Show failure counts

### Visual Elements
```
┌─────────────────────────────────────────────────────┐
│ CORE PROCESS                                        │
├─────────────────────────────────────────────────────┤
│                                                     │
│ ┌──────┐   ┌──────┐   ┌──────┐   ┌──────┐         │
│ │INGEST│ → │SCORE │ → │ROUTE │ → │EMIT  │         │
│ │ ░░░░ │   │ ████ │   │ ░░░░ │   │ ░░░░ │         │
│ └──────┘   └──────┘   └──────┘   └──────┘         │
│              ↑ ACTIVE                               │
│                                                     │
│ THROUGHPUT: 127/sec                                 │
│ LATENCY: 23ms avg                                   │
│ FAILURES: 3 (0.02%)                                 │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Stage States
- **Empty (░░░░)**: Idle
- **Filled (████)**: Processing
- **Highlighted**: Currently active

### Metrics
- Items per second (throughput)
- Processing latency
- Failure rate
- Backlog per stage

---

## Zone 3: Heat / Load

**Purpose:** Stress is visible.

### Responsibilities
- Display resource utilization
- Show thresholds
- Show trends
- Surface anomalies

### Visual Elements
```
┌─────────────────────────────────────┐
│ HEAT / LOAD                         │
├─────────────────────────────────────┤
│                                     │
│ CPU  [████████░░░░░░░░]  52%  →    │
│ MEM  [███████████░░░░░]  71%  ↑    │
│ NET  [██████████████░░]  89%  ↑↑   │
│ DISK [███░░░░░░░░░░░░░]  18%  →    │
│                                     │
│ ──────────────────────────────      │
│ THRESHOLDS                          │
│ NET: 89% / 95% (6% to critical)    │
│ MEM: 71% / 90% (19% to critical)   │
│                                     │
└─────────────────────────────────────┘
```

### Indicators
- **→** Stable
- **↑** Increasing
- **↓** Decreasing
- **↑↑** Rapid increase
- **!** Near threshold

### Metrics
- Percentage of capacity
- Distance to threshold
- Trend direction
- Rate of change

---

## Zone 4: Control Rail

**Purpose:** Actions are available.

### Responsibilities
- Expose system controls
- Show current system state
- Provide emergency actions
- Maintain action history

### Visual Elements
```
┌─────────────────────────────────────┐
│ CONTROL RAIL                        │
├─────────────────────────────────────┤
│                                     │
│ SWARM-SENTINEL              ACTIVE  │
│ ┌──────┐ ┌───────┐ ┌───────┐       │
│ │ STOP │ │ PAUSE │ │ RESET │       │
│ └──────┘ └───────┘ └───────┘       │
│                                     │
│ BASE-SNIPER                 PAUSED  │
│ ┌──────┐ ┌────────┐ ┌───────┐      │
│ │ STOP │ │ RESUME │ │ RESET │      │
│ └──────┘ └────────┘ └───────┘      │
│                                     │
│ ─────────────────────────────       │
│ LAST ACTION: PAUSE base-sniper      │
│ 2 minutes ago                       │
│                                     │
└─────────────────────────────────────┘
```

### Controls
- **STOP**: Halt immediately
- **PAUSE / RESUME**: Suspend/continue
- **RESET**: Return to initial state

### Rules
- Controls never hidden
- Physically separated
- State-appropriate labels
- Action history visible

---

## Zone Sizing

Default proportions (adjustable):

```
┌───────────────────┬─────────────────────────────────────┐
│                   │                                     │
│       25%         │               50%                   │
│                   │                                     │
├───────────────────┼─────────────────────────────────────┤
│                   │                                     │
│       25%         │               50%                   │
│                   │                                     │
└───────────────────┴─────────────────────────────────────┘
```

Core Process gets the most space.  
It's where the work happens.

---

## CSS Grid Implementation

```css
.forge-floor {
  display: grid;
  grid-template-columns: 1fr 2fr;
  grid-template-rows: 1fr 1fr;
  grid-template-areas:
    "feed core"
    "heat control";
  gap: 1px;
  background: var(--surface-border);
  height: 100vh;
}

.zone-feed { grid-area: feed; }
.zone-core { grid-area: core; }
.zone-heat { grid-area: heat; }
.zone-control { grid-area: control; }

.zone {
  background: var(--surface-primary);
  padding: 1rem;
  overflow: auto;
}
```

---

## Zone Communication

Zones communicate through events, not direct coupling:

```typescript
// Feed emits
events.emit('intake:queued', { count: 2847 });

// Core listens
events.on('intake:queued', ({ count }) => {
  updateBacklog(count);
});

// Core emits
events.emit('process:throughput', { rate: 127 });

// Heat listens
events.on('process:throughput', ({ rate }) => {
  updateLoad('processing', rate);
});
```

---

<p align="center"><sub>🔥 UI Zones | DigitalForge</sub></p>
