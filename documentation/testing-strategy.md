# Testing Strategy: Vitreous Gloom

This document defines the quality assurance framework for **Vitreous Gloom**. Given the modular "Lego-brick" architecture of the project, the testing strategy focuses on **isolated unit stability** and **event-driven integration**.

---

## 1. Overall Testing Philosophy & Architecture

### 1.1 The "Isolation First" Rule
Because we are building the game in sprints that must "plug in" to previous work, each module must be tested in a **Headless State** (logic without graphics) before being integrated into the Phaser scene.

### 1.2 The Testing Stack
* **Unit Testing:** [Vitest](https://vitest.dev/) (High-speed, Vite-native testing for TypeScript logic).
* **Integration Testing:** Phaser's built-in **Scene Events**. We will mock the Event Bus to ensure modules "hear" each other.
* **System/Manual Testing:** Visual verification within the browser to ensure pixel-perfect rendering and lighting performance.

### 1.3 The "Event-Mock" Architecture
To test a module like the *Threat Manager* without running the whole game, we mock the `GameEvents` bus:
1.  Emit a fake `MINING_START` event.
2.  Assert that the `ThreatManager` correctly increments the internal `Heat` state.

---

## 2. Sprint-Level Testing Specifications

### Sprint 1: The Glass Grid (Puzzle)
**Focus:** Pure logic and adjacency validation.
* **Unit Tests:**
    * Verify `GridValidator` rejects shards of the same color placed adjacently.
    * Verify `GridValidator` rejects shards of the same purity value placed adjacently.
    * Verify `GridValidator` respects specific cell restrictions (e.g., "Must be Blue").
* **Integration Tests:**
    * Ensure dragging a shard from the `InventoryContainer` to the `GridSlot` updates the `ISaveData` object.
* **Crucial Use Case:** "As a player, if I place a Red 5 next to a Blue 5, the placement should be rejected."

### Sprint 2: Automated Siphoners (RTS)
**Focus:** Pathfinding and State Machine transitions.
* **Unit Tests:**
    * Verify the `Siphoner` FSM correctly transitions from `MOVING` to `MINING` when reaching a node.
    * Verify resource nodes decrement their `remainingYield` correctly.
* **Integration Tests:**
    * When `Siphoner` reaches the `RETURNING` state at the base, ensure a `RESOURCE_COLLECTED` event is fired.
* **Crucial Use Case:** "As a player, when I click an ore vein, the bot should navigate around obstacles and begin extracting."

### Sprint 3: The Threat & Lighting (Horror)
**Focus:** Reactive difficulty and visibility.
* **Unit Tests:**
    * Verify `ThreatManager` calculates heat gain based on the number of active bots.
    * Verify the `SpawnRate` increases as `Heat` crosses 50% and 80%.
* **Integration Tests:**
    * Ensure `Zombie` AI targets the `IVector2` coordinates of the nearest active light source.
* **Crucial Use Case:** "When two bots mine simultaneously, zombies should spawn twice as fast and move toward the mining area."

### Sprint 4: Base Infrastructure (Building)
**Focus:** Persistence and collision.
* **Unit Tests:**
    * Verify structures cannot be built if the inventory has insufficient minerals.
    * Verify `Structure` health reaches zero and triggers a `DESTRUCTION` event.
* **Integration Tests:**
    * Ensure zombies "stop" and play an attack animation when colliding with a `Wall` entity.
* **Crucial Use Case:** "Placing a wall in a bottleneck should force zombies to attack the wall rather than pathfinding through it."

### Sprint 5: The Master Loop (Progression)
**Focus:** State persistence and win/loss conditions.
* **Unit Tests:**
    * Verify `ISaveData` is correctly serialized and written to `localStorage`.
* **Integration Tests:**
    * Verify that completing a window in the `WorkshopScene` triggers a screen-clearing pulse in the `WorldScene`.
* **Crucial Use Case:** "Completing the final shard placement in a window should result in a victory screen and unlock the next tier of resources."

---

## 3. System-Wide Crucial Use Cases (Final Pass)

| Use Case ID | Requirement | Test Type |
| :--- | :--- | :--- |
| **UC-01** | Game must maintain 60FPS during a 50-husk swarm. | Performance |
| **UC-02** | Closing the browser and reopening must restore the player's exact shard inventory. | Persistence |
| **UC-03** | The "Gloom" vignette must follow the player's light source smoothly. | Visual |
| **UC-04** | A player must be able to fail an extraction but keep the shards they successfully returned to base. | Balance |

