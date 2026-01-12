# State Machine

> System state transitions.

---

## States

Every DigitalForge system exists in exactly one state.

```
┌──────────────────────────────────────────────────────────┐
│                                                          │
│  ┌────────┐                              ┌────────┐     │
│  │  IDLE  │ ←─────────────────────────── │COOLING │     │
│  └────────┘                              └────────┘     │
│       │                                       ↑         │
│       ↓                                       │         │
│  ┌────────┐    ┌─────────┐    ┌─────┐    ┌───────┐     │
│  │ ACTIVE │ ─→ │ HEATING │ ─→ │ HOT │ ─→ │CRITICAL│    │
│  └────────┘    └─────────┘    └─────┘    └───────┘     │
│       ↑             │            │            │         │
│       │             ↓            ↓            ↓         │
│       └─────────────┴────────────┴────────────┘         │
│                     (to COOLING)                        │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## State Definitions

| State | Definition | Duration |
|-------|------------|----------|
| **IDLE** | System available, not processing | Indefinite |
| **ACTIVE** | System processing normally | Indefinite |
| **HEATING** | Load increasing, attention suggested | Transient |
| **HOT** | Pre-critical, action may be needed | Transient |
| **CRITICAL** | Immediate attention required | Brief |
| **COOLING** | Returning to baseline | Transient |
| **DISABLED** | System unavailable | Until enabled |

---

## Transitions

### Valid Transitions

```
IDLE      → ACTIVE     (start processing)
ACTIVE    → HEATING    (load threshold crossed)
ACTIVE    → IDLE       (stop processing)
HEATING   → HOT        (load continues rising)
HEATING   → COOLING    (load decreasing)
HOT       → CRITICAL   (threshold breached)
HOT       → COOLING    (load decreasing)
CRITICAL  → COOLING    (intervention or auto-recovery)
COOLING   → ACTIVE     (baseline reached, still processing)
COOLING   → IDLE       (baseline reached, stopped)
ANY       → DISABLED   (system taken offline)
DISABLED  → IDLE       (system re-enabled)
```

### Invalid Transitions

```
IDLE      → HEATING    (must go through ACTIVE)
IDLE      → HOT        (must go through ACTIVE, HEATING)
ACTIVE    → CRITICAL   (must go through HEATING, HOT)
COOLING   → HOT        (must go through ACTIVE, HEATING)
```

---

## Transition Timing

| Transition | Animation | Duration |
|------------|-----------|----------|
| IDLE → ACTIVE | Fade in | 300ms |
| ACTIVE → HEATING | Gradient shift | 500ms |
| HEATING → HOT | Pulse intensify | 300ms |
| HOT → CRITICAL | Immediate | 0ms |
| ANY → COOLING | Desaturation | 1000ms |
| COOLING → IDLE | Fade out | 500ms |

---

## Thresholds

State transitions are triggered by thresholds:

```javascript
const thresholds = {
  heating: 0.70,    // 70% of capacity
  hot: 0.85,        // 85% of capacity
  critical: 0.95,   // 95% of capacity
  cooling: 0.60     // Below 60% triggers cooling
};

function determineState(load, currentState) {
  if (load >= thresholds.critical) return 'CRITICAL';
  if (load >= thresholds.hot) return 'HOT';
  if (load >= thresholds.heating) return 'HEATING';
  if (load < thresholds.cooling && currentState !== 'IDLE') {
    return 'COOLING';
  }
  if (load > 0) return 'ACTIVE';
  return 'IDLE';
}
```

---

## Hysteresis

To prevent state flapping, transitions require sustained conditions:

| Transition | Sustain Time |
|------------|--------------|
| → HEATING | 3 seconds above threshold |
| → HOT | 2 seconds above threshold |
| → CRITICAL | Immediate |
| → COOLING | 5 seconds below threshold |
| → IDLE | 10 seconds at zero |

---

## Implementation

### State Class

```typescript
type SystemState = 
  | 'IDLE' 
  | 'ACTIVE' 
  | 'HEATING' 
  | 'HOT' 
  | 'CRITICAL' 
  | 'COOLING'
  | 'DISABLED';

interface StateMachine {
  current: SystemState;
  previous: SystemState | null;
  enteredAt: number;
  
  transition(to: SystemState): boolean;
  canTransition(to: SystemState): boolean;
  timeSinceTransition(): number;
}
```

### Valid Transition Map

```typescript
const validTransitions: Record<SystemState, SystemState[]> = {
  IDLE: ['ACTIVE', 'DISABLED'],
  ACTIVE: ['IDLE', 'HEATING', 'DISABLED'],
  HEATING: ['ACTIVE', 'HOT', 'COOLING', 'DISABLED'],
  HOT: ['HEATING', 'CRITICAL', 'COOLING', 'DISABLED'],
  CRITICAL: ['HOT', 'COOLING', 'DISABLED'],
  COOLING: ['IDLE', 'ACTIVE', 'DISABLED'],
  DISABLED: ['IDLE']
};
```

---

## Events

State transitions emit events:

```typescript
interface StateEvent {
  system: string;
  from: SystemState;
  to: SystemState;
  timestamp: number;
  trigger: 'threshold' | 'manual' | 'timeout' | 'error';
}

// Example
{
  system: 'swarm-sentinel',
  from: 'ACTIVE',
  to: 'HEATING',
  timestamp: 1704499200000,
  trigger: 'threshold'
}
```

---

## Logging

All transitions are logged:

```
[2026-01-05T16:30:00Z] swarm-sentinel: ACTIVE → HEATING (threshold: cpu=72%)
[2026-01-05T16:31:15Z] swarm-sentinel: HEATING → HOT (threshold: cpu=87%)
[2026-01-05T16:31:45Z] swarm-sentinel: HOT → COOLING (manual: operator)
[2026-01-05T16:33:00Z] swarm-sentinel: COOLING → ACTIVE (threshold: cpu=58%)
```

---

<p align="center"><sub>🔥 State Machine | DigitalForge</sub></p>
