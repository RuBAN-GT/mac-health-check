---
name: mac-health-check
description: Check your Mac's current temperature, load, memory, swap, and power usage with `macmon`.
homepage: https://github.com/vladkens/macmon
metadata:
  {
    "openclaw":
      {
        "emoji": "🌡️",
        "os": ["darwin"],
        "requires": { "bins": ["macmon"] },
        "install":
          [
            {
              "id": "brew",
              "kind": "brew",
              "formula": "macmon",
              "bins": ["macmon"],
              "label": "Install macmon (brew)"
            }
          ]
      }
  }
---

# Mac Health Check

Use `macmon` as the source of truth for live Mac telemetry.

## Installation

### Via ClawHub

```bash
clawhub install mac-health-check
```

### Manual

```bash
git clone https://github.com/RuBAN-GT/mac-health-check-skill.git ~/.openclaw/skills/mac-health-check
```

## Quick start

For a one-shot snapshot, run:

```bash
python {baseDir}/scripts/macmon_status.py
```

Default one-shot mode uses `-i 200` so the first sample returns faster.

For raw JSON:

```bash
python {baseDir}/scripts/macmon_status.py --format json --pretty
```

For saved `macmon` output or stdin:

```bash
macmon pipe -s 1 > /tmp/macmon.jsonl
python {baseDir}/scripts/macmon_status.py --input /tmp/macmon.jsonl
cat /tmp/macmon.jsonl | python {baseDir}/scripts/macmon_status.py --input -
```

## Verify setup

Run:

```bash
macmon pipe -s 1
```

If this prints a JSON sample, the skill is ready to use.

## What to report

Default summary includes:

- CPU average temperature
- GPU average temperature
- Performance CPU usage and frequency
- Efficiency CPU usage and frequency
- GPU usage and frequency
- System, CPU, and GPU power
- RAM usage
- Swap usage

Keep the explanation practical and short unless the user asks for a deeper breakdown.

## Interpretation hints

- `CPU temp under ~60C`: usually calm or light work
- `~60C to 85C`: normal active load
- `85C+`: hot; mention sustained load or possible thermal pressure
- `swap usage above 0`: memory pressure may be starting
- `high system power + high temps`: likely sustained work

Do not overclaim danger from one sample. Call it a snapshot.

## Failure modes

- If `macmon` is missing, say so plainly.
- If `macmon` is installed but errors out, ask the user to run `macmon pipe -s 1` manually and paste the JSON.
- When reading from files or stdin, treat the last non-empty line as the sample.

## Reference

Use [references/sample-output.md](references/sample-output.md) when you need a reminder of the common JSON fields emitted by `macmon`.
