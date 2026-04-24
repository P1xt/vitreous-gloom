# Data Schema: Vitreous Gloom

This document defines the data structures, types, and persistence schemas for **Vitreous Gloom**. By establishing these interfaces upfront, we ensure that the "Lego bricks" built in separate sprints can communicate via a unified data language.

---

## 1. Big Picture: The Data Flow
The game's data is categorized into three layers:
1.  **Static Definitions (Manifests):** Read-only JSON files defining the properties of minerals, window patterns, and building costs.
2.  **Live State (The Registry):** In-memory TypeScript objects tracking current bot positions, active enemies, and the current "Heat" level.
3.  **Persistence (Save Data):** A serialized JSON object stored in `localStorage` to save player progress across sessions.

---

## 2. Global Core Types
These types are used across all modules and sprints.

```typescript
/** types/core.ts **/

export type MineralColor = 'Ruby' | 'Cobalt' | 'Amber' | 'Quartz' | 'Emerald';
export type PurityValue = 1 | 2 | 3 | 4 | 5 | 6;

export interface IVector2 {
    x: number;
    y: number;
}
```

---

## 3. Sprint-Level Data Schemas

### Sprint 1: The Glass Grid (Puzzle)
This schema handles the logic of the Sagrada-style window.

```typescript
/** types/puzzle.ts **/

export interface IShard {
    uuid: string;         // Unique ID for dragging/tracking
    color: MineralColor;
    value: PurityValue;
}

export interface ICell {
    row: number;
    col: number;
    colorRestriction?: MineralColor; // null if any color fits
    valueRestriction?: PurityValue;  // null if any value fits
    occupant: IShard | null;         // The shard currently in the slot
}

export interface IWindowPattern {
    id: string;
    name: string;
    description: string;
    grid: ICell[][];      // 5x4 layout
    rewardExp: number;
}
```

### Sprint 2: Automated Siphoners (RTS)
This schema manages the resource nodes and the worker bot state.

```typescript
/** types/rts.ts **/

export enum SiphonerState {
    IDLE,
    MOVING,
    MINING,
    RETURNING,
    REPAIRING
}

export interface ISiphoner {
    id: string;
    position: IVector2;
    state: SiphonerState;
    cargo: {
        type: MineralColor | null;
        amount: number;
        maxCapacity: number;
    };
    health: number;
    miningSpeed: number; // Resources per second
}

export interface IResourceNode {
    id: string;
    type: MineralColor;
    position: IVector2;
    remainingYield: number;
    richness: number; // Multiplier for purity rolls (1.0 - 2.0)
}
```

### Sprint 3: The Threat (Horror)
This schema tracks the "Heat" and enemy attributes.

```typescript
/** types/horror.ts **/

export interface IThreatState {
    currentHeat: number;    // 0 to 100
    noiseMultiplier: number; // Scaling based on active bots
    spawnRate: number;      // Seconds between spawns
}

export interface IEnemy {
    id: string;
    type: 'Husk' | 'Stalker' | 'Ghost';
    health: number;
    speed: number;
    damage: number;
    lastLightPosition: IVector2; // Used for "investigating" last known light
}
```

### Sprint 4: Base Infrastructure (Building)
This schema defines the cost and properties of structures.

```typescript
/** types/building.ts **/

export interface IStructure {
    id: string;
    type: 'Wall' | 'Turret' | 'Refinery' | 'Lantern';
    position: IVector2;
    health: number;
    cost: {
        mineral: MineralColor;
        amount: number;
    }[];
}

export interface ITurret extends IStructure {
    range: number;
    fireRate: number;
    targetId: string | null;
}
```

### Sprint 5: Meta-Progression (Save Data)
The final schema used to store the "Lego" state into browser memory.

```typescript
/** types/persistence.ts **/

export interface ISaveData {
    version: string;
    timestamp: number;
    inventory: {
        rawOre: Record<MineralColor, number>;
        refinedShards: IShard[];
    };
    unlockedTiers: number;       // 1-3
    completedWindows: string[];  // Array of pattern IDs
    baseLayout: IStructure[];
    playerStats: {
        totalLightRestored: number;
        hordesSurvived: number;
    };
}
```

---

## 4. Resource Manifest (Example JSON)
This is a sample of the static data used to populate the game world.

```json
{
  "minerals": [
    {
      "id": "ruby",
      "color": "Ruby",
      "hex": "#ff3e3e",
      "heatGeneration": 1.5,
      "description": "Volatile blood-ore. High heat, high ward value."
    },
    {
      "id": "quartz",
      "color": "Quartz",
      "hex": "#e0e0e0",
      "heatGeneration": 0.5,
      "description": "Stable lead-glass base. Low heat generation."
    }
  ]
}
```

