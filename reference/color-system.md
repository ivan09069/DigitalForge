# Color System

> State colors. Never theme.

---

## Principle

Color communicates system state.  
Color is never decorative.  
Colors are never reused across states.

---

## State Map

| State | Hex | RGB | Usage |
|-------|-----|-----|-------|
| **Idle** | `#164e63` | `22, 78, 99` | Available but not active |
| **Active** | `#22d3ee` | `34, 211, 238` | Running normally |
| **Heating** | `#8b5cf6` | `139, 92, 246` | Load increasing |
| **Hot** | `#ec4899` | `236, 72, 153` | Pre-critical |
| **Critical** | `#ef4444` | `239, 68, 68` | Immediate attention |
| **Cooling** | `#1e3a5f` | `30, 58, 95` | Returning to baseline |
| **Disabled** | `#1f2937` | `31, 41, 55` | Not available |
| **Success** | `#22c55e` | `34, 197, 94` | Operation complete (flash) |

---

## Visual Reference

```
IDLE        ████████████████  #164e63
ACTIVE      ████████████████  #22d3ee
HEATING     ████████████████  #8b5cf6
HOT         ████████████████  #ec4899
CRITICAL    ████████████████  #ef4444
COOLING     ████████████████  #1e3a5f
DISABLED    ████████████████  #1f2937
SUCCESS     ████████████████  #22c55e
```

---

## Glow Values

Active states include glow for emphasis:

| State | Glow | Spread |
|-------|------|--------|
| Active | `rgba(34, 211, 238, 0.4)` | 20px |
| Heating | `rgba(139, 92, 246, 0.4)` | 25px |
| Hot | `rgba(236, 72, 153, 0.4)` | 30px |
| Critical | `rgba(239, 68, 68, 0.6)` | 40px |

---

## Text Pairing

| Background | Text |
|------------|------|
| Idle | `#22d3ee` or `#e5e5e5` |
| Active | `#000000` or `#0a0a0a` |
| Heating | `#ffffff` |
| Hot | `#ffffff` |
| Critical | `#ffffff` |
| Cooling | `#e5e5e5` |

---

## CSS Implementation

```css
:root {
  /* State colors */
  --color-idle: #164e63;
  --color-active: #22d3ee;
  --color-heating: #8b5cf6;
  --color-hot: #ec4899;
  --color-critical: #ef4444;
  --color-cooling: #1e3a5f;
  --color-disabled: #1f2937;
  --color-success: #22c55e;
  
  /* Glow variants */
  --glow-active: 0 0 20px rgba(34, 211, 238, 0.4);
  --glow-heating: 0 0 25px rgba(139, 92, 246, 0.4);
  --glow-hot: 0 0 30px rgba(236, 72, 153, 0.4);
  --glow-critical: 0 0 40px rgba(239, 68, 68, 0.6);
}

/* State classes */
.state-idle { 
  background: var(--color-idle); 
  color: var(--color-active);
}

.state-active { 
  background: var(--color-active); 
  color: #000;
  box-shadow: var(--glow-active);
}

.state-heating { 
  background: var(--color-heating); 
  color: #fff;
  box-shadow: var(--glow-heating);
}

.state-hot { 
  background: var(--color-hot); 
  color: #fff;
  box-shadow: var(--glow-hot);
  animation: pulse-hot 1s ease-in-out infinite;
}

.state-critical { 
  background: var(--color-critical); 
  color: #fff;
  box-shadow: var(--glow-critical);
  animation: pulse-critical 0.5s ease-in-out infinite;
}

@keyframes pulse-hot {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.85; }
}

@keyframes pulse-critical {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.9; }
}
```

---

## Accessibility

Color is primary but never sole indicator.

| State | Secondary Indicator |
|-------|---------------------|
| Idle | Solid border, no motion |
| Active | Glow + subtle pulse |
| Heating | Faster pulse |
| Hot | Rapid pulse + thick border |
| Critical | Strobe + icon change |

---

## Anti-Patterns

❌ Using state colors for branding  
❌ Same color for different states  
❌ Gradients between states  
❌ User-configurable state colors  
❌ Color without secondary indicator  

---

<p align="center"><sub>🔥 Color System | DigitalForge</sub></p>
