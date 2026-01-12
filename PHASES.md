# Phases

> Forge Floor → Signal Tower → Blacksite

---

## Overview

DigitalForge operates in deliberate phases.  
No phases are skipped.

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   PHASE 1              PHASE 2              PHASE 3         │
│   ─────────            ─────────            ─────────       │
│                                                             │
│   FORGE FLOOR    →    SIGNAL TOWER    →    BLACKSITE       │
│                                                             │
│   Visible work        Controlled           Silent systems   │
│   Active processes    broadcast            Asymmetric       │
│   Measured output     External             advantage        │
│                       observability                         │
│                                                             │
│   [CURRENT]           [FUTURE]             [EARNED]         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Phase 1: Forge Floor

**Status:** Current

### Definition
The working environment where systems are built, stressed, and refined.

### Characteristics
- All work is visible
- Processes are active and measurable
- Output is tracked and exposed
- Failure is expected and learned from

### Systems
| System | Zone | Function |
|--------|------|----------|
| SwarmSentinel-v3 | Core Process | Sentiment pipeline |
| jit-command-center | Heat / Load | Monitoring |
| Base-Sniper | Feed / Intake | Token data |
| EchoForge-agents | Control Rail | Orchestration |

### Exit Criteria
- [ ] All four zones have active systems
- [ ] PSL integrated across dashboards
- [ ] Control doctrine implemented
- [ ] 30 days stable operation

---

## Phase 2: Signal Tower

**Status:** Future

### Definition
The broadcast layer. Controlled external observability.

### Characteristics
- Selected metrics are externalized
- Public dashboards (read-only)
- API endpoints for integrations
- Signal feeds for subscribers

### What Gets Broadcast
- System health (aggregate)
- Signal quality scores
- Regime classifications
- Performance metrics

### What Stays Internal
- Raw data feeds
- Strategy logic
- Execution details
- Wallet states

### Entry Criteria
- Forge Floor exit criteria met
- Observability stack stable
- Rate limiting implemented
- Access controls verified

### Exit Criteria
- [ ] Public dashboard live
- [ ] API documented
- [ ] Rate limiting proven under load
- [ ] 90 days stable broadcast

---

## Phase 3: Blacksite

**Status:** Earned

### Definition
Silent systems. Asymmetric advantage.

### Characteristics
- No external visibility
- Competitive edge systems
- Execution-sensitive logic
- Zero broadcast

### What Lives Here
- Execution engines
- Alpha strategies
- Timing systems
- Edge infrastructure

### Entry Criteria
- Signal Tower exit criteria met
- Operational security verified
- Isolation architecture proven
- Trust established

### Rules
- No public documentation
- No external API
- No shared infrastructure with broadcast systems
- Air-gapped from Signal Tower

---

## Phase Transitions

### Forge Floor → Signal Tower

```
Trigger:    30 days stable + all zones active
Process:    
  1. Audit all exposed metrics
  2. Build public dashboard
  3. Implement rate limiting
  4. Deploy to isolated infrastructure
  5. Announce availability
Rollback:   Always possible
```

### Signal Tower → Blacksite

```
Trigger:    90 days stable broadcast + security audit passed
Process:    
  1. Identify systems for isolation
  2. Build air-gapped infrastructure
  3. Migrate selected systems
  4. Verify zero leakage
  5. No announcement
Rollback:   Not expected
```

---

## Current Status

```
Phase 1: FORGE FLOOR
├── SwarmSentinel-v3    [████████░░] 80%
├── jit-command-center  [██████░░░░] 60%
├── Base-Sniper         [████░░░░░░] 40%
└── EchoForge-agents    [██░░░░░░░░] 20%

Overall: [██████░░░░] 50%

Estimated transition: TBD
```

---

## Philosophy

### Why Phases?

Phases exist because:
- Premature broadcast destroys edge
- Premature hiding destroys trust
- Premature complexity destroys systems

Each phase earns the next.

### Why Not Skip?

Skipping phases means:
- Building on unverified foundations
- Exposing unstable systems
- Hiding systems that need stress

Every shortcut is debt.

### Why This Order?

```
Forge Floor    → Proves the systems work
Signal Tower   → Proves the systems can be observed safely
Blacksite      → Protects the systems that create edge
```

The order is non-negotiable.

---

<p align="center"><sub>🔥 Phases | DigitalForge</sub></p>
