# UI/UX Wireframes: Vitreous Gloom (Full Scene-by-Scene)

This document provides a comprehensive wireframe layout for every state/scene in **Vitreous Gloom**. These layouts ensure that the transition from a cozy hub to a terrifying extraction zone is visually distinct and functional.

---

## 1. Scene 1: The Main Menu (The "Sanctum Entrance")
**Context:** The initial load screen. High-contrast, moody pixel art.

```text
+-------------------------------------------------------------+
|                                                             |
|                   [ VITREOUS GLOOM ]                        |
|                 (Title in Gothic Type)                      |
|                                                             |
|                      [ NEW VIGIL ]                          |
|                    [ RESUME VIGIL ]                         |
|                      [ SETTINGS ]                           |
|                        [ EXIT ]                             |
|                                                             |
|                                                             |
| v1.0.4-beta                            (C) 2026 Vitromancy  |
+-------------------------------------------------------------+
```
* **Background:** A slow-panning view of a dark cathedral interior, illuminated by a single flickering candle.

---

## 2. Scene 2: The Sanctum Hub (The "Stardew" Workshop)
**Context:** Safe zone for upgrading, saving, and choosing missions.

```text
+-------------------------------------------------------------+
| [HUD: INV] Ruby:10  Cobalt:5  Quartz:30                     |
|                                                             |
|      (WORLD VIEW)                         [ UPGRADE BENCH ] |
|      +--------------------------+         +---------------+ |
|      | Player can move here.    |         | - Siphoners   | |
|      | NPCs stand here.         |         | - Turrets     | |
|      | [ORDERS BOARD]           |         | - Armor       | |
|      +--------------------------+         +---------------+ |
|                                                             |
| [E] TALK TO ARCH-VITROMANCER          [SPACE] VIEW ORDERS   |
+-------------------------------------------------------------+
```
* **Visuals:** Warm colors (ambers, gold). No "Gloom" vignetting.

---

## 3. Scene 3: The Orders Board (Quest Selection)
**Context:** Modal overlay in the Hub.

```text
+-------------------------------------------------------------+
| [X] CLOSE                      SELECT A CATHEDRAL TO WARD   |
|                                                             |
|   +-----------------------+     +-----------------------+   |
|   | [ICON] NORTH TRANSEPT |     | [ICON] THE CRYPT      |   |
|   | Req: 5 Ruby, 2 Cobalt |     | Req: 10 Amber, 5 Quartz|  |
|   | Reward: Tier 2 Tools  |     | Reward: Holy Oil      |   |
|   | [ START EXPEDITION ]  |     | [ LOCKED ]            |   |
|   +-----------------------+     +-----------------------+   |
|                                                             |
+-------------------------------------------------------------+
```

---

## 4. Scene 4: The Expedition Zone (The "Extraction" Tactical View)
**Context:** Main gameplay. Real-time RTS/Shooter combat.

```text
+-------------------------------------------------------------+
| [HP: |||||]                                      [HEAT: !!] |
| [EN: |||||]                                      [ 12 % ]   |
|                                                             |
|      (GLOOM WORLD)                                          |
|      [O] <- Mineral Node                                    |
|       |                                                     |
|      [B] <- Siphoner Bot (Mining...)         (ALERT LOG)    |
|       ^                                      Bot 01 Attacked|
|      [P] <- Player (Defending)               Horde Incoming!|
|                                                             |
| [1:BOT] [2:WALL] [3:TURRET] [4:LANTERN]       [TAB] WORKSHOP|
+-------------------------------------------------------------+
```
* **Visuals:** Dark, desaturated. High-intensity lighting centered on `[P]` and `[B]`.

---

## 5. Scene 5: The Workshop Overlay (The "Sagrada" Puzzle)
**Context:** Accessible during Scene 4 or back at the Hub. **Does not pause Scene 4.**

```text
+-------------------------------------------------------------+
| [X] CLOSE (TAB)                                 (INVENTORY) |
|                                                 [R5] [R2]   |
|      +--------------------------+               [C1] [Q6]   |
|      |      THE WARD GRID       |               [A3] [A2]   |
|      |      (5x4 WINDOW)        |                           |
|      |  [ ]  [R]  [ ]  [3]  [ ] |      (SHARD BINS)         |
|      |  [ ]  [ ]  [B]  [ ]  [ ] |      +-------------+      |
|      |  [6]  [ ]  [ ]  [ ]  [ ] |      | [DRAG SHARD]|      |
|      |  [ ]  [ ]  [G]  [5]  [ ] |      +-------------+      |
|      +--------------------------+                           |
|                                         (WORLD THREAT LEVEL)|
| [TOOL: REFRACT]  [TOOL: SHATTER]        HEAT: [||||||||--]  |
+-------------------------------------------------------------+
```

---

## 6. Scene 6: The Warding Ritual (Victory/Transition)
**Context:** Triggered when the grid in Scene 5 is completed correctly.

```text
+-------------------------------------------------------------+
|                                                             |
|                                                             |
|                    [ WARD RESTORED ]                        |
|                                                             |
|             (Large VFX Pulse on Window Art)                 |
|                                                             |
|                Resources Gained: +500 Exp                   |
|                New Tech Unlocked: Siphoner MK II            |
|                                                             |
|                    [ RETURN TO HUB ]                        |
|                                                             |
+-------------------------------------------------------------+
```

---

## 7. Scene 7: Game Over (The "Extinguished" Screen)
**Context:** Triggered when Player HP or Cathedral Heart reaches zero.

```text
+-------------------------------------------------------------+
|                                                             |
|                   [ THE LIGHT FADES ]                       |
|                                                             |
|                 The Sanctum has fallen.                     |
|                                                             |
|                  Hordes Survived: 12                        |
|                  Minerals Gathered: 450                     |
|                                                             |
|                      [ TRY AGAIN ]                          |
|                      [ ABANDON ]                            |
|                                                             |
+-------------------------------------------------------------+
```

---

## 8. Technical UI Implementation Details

* **Scene Layering (Phaser Strategy):**
    * `BackgroundScene`: (MainMenu, SanctumHub, WorldScene)
    * `OverlayScene`: (OrdersBoard, WorkshopOverlay) — Launched using `this.scene.launch('Workshop')`.
    * `UIScene`: (HUD elements) — Always on top, never sleeps.
* **Responsiveness:** Use Phaser's `Scale Manager`. Target a base resolution of `1280x720` but allow the `WorkshopOverlay` to scale down on smaller browser windows to ensure the player can still see the `WorldScene` at the edges.
* **Interaction:**
    * **Click-to-Target:** For RTS bot commands.
    * **Drag-and-Drop:** For Sagrada grid shards.
    * **Hotkeys (1-9):** For building/defensive structures.

---

**Next Step:** With the full scene-by-scene wireframes documented, the project's visual flow is complete. 

**Are you ready to move into the implementation of Sprint 1 (The Grid Logic), or would you like to review the Scene Management code in Phaser first?**