# Game Design Document: Vitreous Gloom

## 1. Project Overview: The Deep Dive

**Vitreous Gloom** is a 2D "Post-Apocalyptic Gothic" hybrid. It combines the industrial tension of an extraction RTS with the tactical puzzle-solving of tile-placement board games. 

### 1.1 Theme: The Dying Light
The world is smothered by **The Gloom**, a necrotic fog that acts as a physical medium for undeath. Humanity’s last bastions are the **Sanctums**—massive Gothic cathedrals. These structures are powered by **Aetheric Glass**, which generates a "Lunar Veil" to keep the fog and the husks (zombies) at bay. As the glass ages, the Veil thins.

### 1.2 The Role of the Vitromancer
You play as a **Vitromancer**, a combat-engineer and artisan. You must leave the safety of the Sanctum to extract "Soul-Infused Ores" from infested zones, refine them into shards, and reconstruct the Great Windows.

### 1.3 Core Gameplay Loop
1. **The Extraction (RTS):** Deploy "Siphoner" bots into the Gloom to mine ore.
2. **The Defense (Horde):** Protect the bots from zombies attracted to the noise and light of extraction.
3. **The Refinement (Crafting):** Process ore into glass shards with specific colors and purity values (1-6).
4. **The Ward (Puzzle):** Place shards into a $5 \times 4$ window grid following "Aetheric Interference" rules (The Sagrada Mechanic).

---

## 2. Technical Architecture & Lego-Brick Strategy

The game is designed with a **Modular Component Architecture**. Each system is a "Lego brick" that communicates via a central **Event Bus**.

### 2.1 Core Data Interfaces
To ensure every sprint "plugs in" to the next, we define these global TypeScript interfaces:

```typescript
type MineralColor = 'Ruby' | 'Cobalt' | 'Amber' | 'Quartz' | 'Emerald';
type Purity = 1 | 2 | 3 | 4 | 5 | 6;

interface IShard {
    id: string;
    color: MineralColor;
    value: Purity;
}

interface IResourceNode {
    type: MineralColor;
    totalYield: number;
    currentYield: number;
}
```

---

## 3. The Sprints (Lego Bricks)

### Sprint 1: The Glass Grid (Puzzle Module)
* **Focus:** The core Sagrada mechanics.
* **Logic:** A `GridController` manages the window grid and validates placement.
* **The Rules:** * **Color Rule:** No two shards of the same color can touch orthogonally.
    * **Purity Rule:** No two shards of the same numerical value can touch orthogonally.
* **Lego Output:** A standalone UI where players can drag shards into a grid and see if the placement is valid.

### Sprint 2: Automated Siphoners (RTS Module)
* **Focus:** The worker-bot loop.
* **Logic:** Implement the `Siphoner` entity using a Finite State Machine: `IDLE` -> `MOVING` -> `MINING` -> `RETURNING`.
* **Lego Output:** A top-down map where bots extract minerals. It "plugs in" to Sprint 1 by populating the shard inventory.

### Sprint 3: The Threat & Lighting (Horror Module)
* **Focus:** Visibility and tension.
* **Logic:** * **Noise System:** Mining increases "Heat," triggering zombie spawns from map edges.
    * **2D Lighting:** The world is pitch black. Bots and the player provide the only light sources.
* **Lego Output:** Adds the "Horde Mode" to the Sprint 2 extraction loop.

### Sprint 4: Base Infrastructure (Building Module)
* **Focus:** Permanent upgrades and defenses.
* **Logic:** A `ConstructionManager` allows the player to build walls and turrets using the minerals gathered.
* **Lego Output:** Adds a tactical "Stardew/Tower Defense" layer to the survival phase.

### Sprint 5: The Master Loop (Meta-Progression)
* **Focus:** The "Quest" and "Tiers" system.
* **Logic:** An **Order System** where NPCs request specific windows. Completing a window (Sprint 1) triggers a "Warding Pulse" that clears the map and unlocks the next tier of ores.
* **Lego Output:** Ties all previous bricks into a finished, repeatable game loop.

---

## 4. Technical Constraints (Phaser + TypeScript)
* **Deployment:** Web-only (HTML5/Canvas).
* **Assets:** Pixel-art spritesheets with `nearest` texture filtering.
* **Events:** A global `Phaser.Events.EventEmitter` serves as the "studs" that connect the Lego bricks.

---
