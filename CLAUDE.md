# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a LocalGPT workspace - a persistent memory and skills directory for an AI agent. It stores long-term memory, persona configuration, and custom skills (primarily 3D world generation skills).

## Core Files

| File | Purpose |
|------|---------|
| `MEMORY.md` | Curated long-term knowledge that persists across sessions |
| `HEARTBEAT.md` | Pending tasks for autonomous/heartbeat mode |
| `SOUL.md` | AI persona and behavioral guidelines |
| `LocalGPT.md` | Standing instructions injected into every conversation |
| `memory/*.md` | Daily logs and notes |

## Skills Structure

Skills are stored in `skills/<skill-name>/` directories:

```
skills/<skill-name>/
  SKILL.md       # YAML frontmatter + description
  world.ron      # 3D scene definition (RON format)
  export/        # GLB exports of the scene
  history.jsonl  # Operation history (JSONL)
```

### SKILL.md Format

```yaml
---
name: "skill-name"
description: "Description"
user-invocable: true
metadata:
  type: "world"
useWhen:
  - contains: "trigger-phrase"
---
# skill-name

Description text here.
```

### world.ron Format

RON (Rusty Object Notation) files define 3D scenes with:

- **Entities**: Objects with transforms, shapes, materials, behaviors, audio
- **Shapes**: `Sphere`, `Cuboid`, `Cylinder`, `Cone`, `Capsule`, `Torus`
- **Behaviors**: `Orbit`, `Pulse`, `Bob`, `PathFollow`, `Spin`
- **Materials**: PBR properties (color, metallic, roughness, emissive, alpha_mode)
- **Audio**: Ambient sources (`Forest`, `Wind`, `Fire`) and SFX

### history.jsonl Format

JSON Lines with operation records:
```json
{"seq":0,"op":{"SpawnEntity":{...}},"inverse":{"DeleteEntity":{...}},"timestamp_ms":0}
```

Operations include: `SpawnEntity`, `DeleteEntity`, `SpawnAudioEmitter`, `RemoveAudioEmitter`, `SetAmbience`, `Batch`

## Entity Hierarchy

Entities can have parent-child relationships via `parent: Some((parent_id))`. Child transforms are relative to parent.

## Common Behaviors

- **Orbit**: Rotate around a center point or entity
- **Pulse**: Scale oscillation between min/max values
- **Bob**: Translate oscillation along an axis
- **PathFollow**: Move along waypoints (loop or once)
- **Spin**: Continuous rotation
