# Technical Specification: Vitreous Gloom

This document outlines the technical implementation strategy for **Vitreous Gloom**. It defines the system architecture, data structures, and specific technical requirements for each development sprint using **Phaser 3** and **TypeScript**.

---

## 1. System Architecture & High-Level Blueprint

The system follows a **Decoupled Event-Driven Architecture**. Because the game combines three distinct genres (RTS, Puzzle, and Building), the codebase is organized into **independent managers** that do not reference each other directly. Instead, they communicate through a global **Event Bus**.

### 1.1 The High-Level Flow
1.  **State Management:** A global `GameState` object tracks player inventory, unlocked tiers, and current "Warding" status.
2.  **Scene Orchestration:**
    * `WorldScene`: Handles the RTS extraction and 2D lighting (Sprints 2, 3, 4).
    * `WorkshopScene`: A UI-overlay scene handling the Sagrada puzzle (Sprint 1).
3.  **Communication:** When a mineral is mined in the `WorldScene`, a `MINERAL_COLLECTED` event is fired. the `WorkshopScene` listens for this to update the shard inventory.

### 1.2 Global Data Interfaces
```typescript
/** core/types.ts **/

export type MineralColor = 'Ruby' | 'Cobalt' | 'Amber' | 'Quartz' | 'Emerald';
export type Purity = 1 | 2 | 3 | 4 | 5 | 6;

export interface IShard {
    uuid: string;
    color: MineralColor;
    value: Purity;
}

export interface IGridCell {
    x: number;
    y: number;
    requiredColor?: MineralColor;
    requiredValue?: Purity;
    currentShard: IShard | null;
}
```

---

## 2. Sprint Specifications

### Sprint 1: The Glass Grid (Puzzle Module)
**Objective:** Create a robust, rule-checking grid system that handles the Sagrada placement logic.

* **Logic Component (`GridValidator.ts`):** * A utility class with a `validatePlacement(grid, shard, x, y)` method.
    * **Logic:** Check North, South, East, and West neighbors. Return `false` if `neighbor.color === shard.color` or `neighbor.value === shard.value`.
* **UI Component (`WindowGrid.ts`):**
    * A `Phaser.GameObjects.Container` representing a $5 \times 4$ grid.
    * Implements Drag-and-Drop using Phaser's `input.on('drag')`.
* **Deliverable:** A functional screen where a user can move shards into a grid. Invalid moves are rejected and shards snap back to the inventory.

### Sprint 2: Automated Siphoners (RTS Module)
**Objective:** Implement the resource gathering loop and worker AI.

* **Entity Logic (`Siphoner.ts`):**
    * A Finite State Machine (FSM) class.
    * `States`: `IDLE`, `MOVING`, `MINING`, `RETURNING`.
    * Use `navmesh` or basic A* for pathfinding to `ResourceNode` entities.
* **Inventory System (`Registry.ts`):**
    * A global store that maps `MineralColor` to counts.
    * **Event:** `GameEvents.emit('RESOURCE_DEPOSITED', data)`.
* **Deliverable:** A top-down scene where a player clicks an ore vein, and a bot navigates to it, "mines" for 5 seconds, and updates the global inventory.

### Sprint 3: The Threat & Lighting (Horror Module)
**Objective:** Integrate tension via restricted visibility and reactive enemy spawning.

* **Lighting Engine:**
    * Enable `this.lights.enable()` in the World Scene.
    * Set `this.lights.setAmbientColor(0x000000)`.
    * Attach `Phaser.GameObjects.Light` to the Player and Siphoners.
* **Heat System (`ThreatManager.ts`):**
    * A counter that increments by `+1` every second a Siphoner is mining.
    * At `Heat > 50`, trigger `ZombieSpawner.spawn()`.
* **Zombie AI:**
    * Simple "Move-To" logic targeting the nearest `Light` source.
* **Deliverable:** An extraction mission where visibility is limited to the bot's vicinity, and enemies emerge from the dark based on mining activity.

### Sprint 4: Base Infrastructure (Building Module)
**Objective:** Provide the player with tools to manipulate the environment.

* **Construction (`BuildManager.ts`):**
    * A "Ghost Placement" system where players select a `Wall` or `Turret`.
    * **Constraint:** Requires specific resources from the inventory (e.g., 10 Quartz).
* **Combat Interaction:**
    * Zombies check for `overlap` with Walls. If touching, they play an attack animation and reduce Wall health.
* **Deliverable:** A "Build Mode" overlay allowing players to spend minerals to place static defenses around their mining operation.

### Sprint 5: The Master Loop (Meta-Progression)
**Objective:** Connect the modules into a meaningful progression cycle.

* **The Order System:**
    * A JSON manifest of "Windows" that need fixing.
    * Example: "The South Transept Window" requires 3 Rubies and 2 Cobalts arranged in a specific pattern.
* **Progression Logic:**
    * Completing a window fires `WORLD_WARDED`. This triggers a `SceneTransition`, resets the `Heat`, and clears all current enemies.
    * Unlocks higher `Purity` levels (e.g., can now refine level 4-6 shards).
* **Deliverable:** An end-to-end game experience from landing in the Gloom to restoring a Cathedral window.

---

## 3. Technical Requirements

* **Language:** TypeScript 5.x
* **Framework:** Phaser 3.80+
* **Bundler:** Vite (for fast HMR and optimized web chunks)
* **Assets:** * Spritesheets: Packed via TexturePacker (Extrude: 2px to prevent bleeding).
    * Audio: `.m4a` (Safari) and `.ogg` (Chrome/Firefox) compatibility.
* **Performance Target:** 60fps on modern mobile and desktop browsers; initial load size < 10MB.

