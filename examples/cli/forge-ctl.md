# CLI: forge-ctl

> WIP - Command structure only

---

## Status: 🔧 Forging

---

## Commands

```bash
forge-ctl <command> [options]

Commands:
  status        Show all systems
  start <sys>   Start a system
  stop <sys>    Stop a system
  pause <sys>   Pause a system
  reset <sys>   Reset a system to initial state
  heat          Show heat/load overview
  tail <sys>    Follow system logs
```

---

## Planned Output

```bash
$ forge-ctl status

SYSTEM              STATE      LOAD    UPTIME
swarm-sentinel      ACTIVE     47%     3h 22m
jit-command         ACTIVE     23%     3h 22m
base-sniper         PAUSED     --      --
echoforge-agents    IDLE       --      --

FORGE FLOOR: 2/4 active
```

```bash
$ forge-ctl heat

AGGREGATE LOAD
──────────────────────────────────────
CPU  [████████░░░░░░░░░░░░]  41%
MEM  [██████████░░░░░░░░░░]  52%
NET  [████░░░░░░░░░░░░░░░░]  19%
──────────────────────────────────────

HOTTEST: swarm-sentinel (47%)
COOLEST: base-sniper (paused)
```

---

## Implementation

```python
# forge_ctl.py - stub

import click

@click.group()
def cli():
    """DigitalForge control interface."""
    pass

@cli.command()
def status():
    """Show all systems."""
    # TODO: connect to state store
    click.echo("Not implemented")

@cli.command()
@click.argument('system')
def stop(system):
    """Stop a system."""
    # TODO: send stop signal
    click.echo(f"Stopping {system}...")

if __name__ == '__main__':
    cli()
```

---

## Notes

```
2026-01-05: Command structure defined
            Output format designed
            Implementation pending
```

---

<p align="center"><sub>🔧 Forging | DigitalForge</sub></p>
