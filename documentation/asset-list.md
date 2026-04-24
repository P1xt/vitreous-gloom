# Asset Manifest: Vitreous Gloom (To-Do List)

This document serves as the comprehensive checklist for all visual and auditory assets required for **Vitreous Gloom**. All graphical assets are designed for a **2D Retro Pixel-Art** aesthetic, and audio assets are optimized for low-latency web playback.

---

## 1. Visual Assets (Sprites & Tiles)

### 1.1 Environment & Tilesets
* **[ ] Sanctum Cathedral Tileset (32x32):** Polished stone floors, Gothic arches, pews, alchemical workbenches, and the "Great Window" frame (static).
* **[ ] The Gloom Zone Tileset (32x32):** Corrupted earth, cracked stone, bone-strewn dirt, and "Fog of War" overlay tiles.
* **[ ] Mineral Veins:** 5 variants (Ruby, Cobalt, Amber, Quartz, Emerald). Each needs a "Full," "Diminished," and "Depleted" state.

### 1.2 Characters & Units (Animated)
* **[ ] The Vitromancer (Player):** 4-directional walk cycle, "Placing Glass" animation, and a "Flashlight/Lantern" holding pose.
* **[ ] The Siphoner (Worker Bot):** Small, multi-legged mechanical crawler. 
    * Animations: Walk, Deploy (Mining mode), and Damage/Sparking.
* **[ ] The Husk (Common Zombie):** Shambling, desaturated humanoid. 
    * Animations: Walk, Attack (Lunge), and Dissolve (Death).
* **[ ] The Ghost (Spectral Enemy):** Semi-transparent, floating. 
    * Animations: Idle bobbing, Fade-in/out.

### 1.3 Building & Defense Structures
* **[ ] Barricade/Wall:** Wooden and stone variants. Needs 3 stages of "Destruction" (75%, 50%, 25% health).
* **[ ] Auto-Turret:** Small rotating ball-joint turret. Needs a "Muzzle Flash" sprite.
* **[ ] Warding Lantern:** A static pole with a glowing glass orb.

### 1.4 UI & Puzzle Elements
* **[ ] Sagrada Shards:** 5 colors x 6 purity values (30 unique pixel-art icons). Shards should have a "gem-like" inner glow.
* **[ ] Grid UI:** The 5x4 window frame with "Restriction" icons (small ghost-like icons for color/value requirements).
* **[ ] HUD Icons:** Health heart, Heat meter (flame icon), and Resource counters for each mineral.

---

## 2. Lighting & VFX (Engine-Generated)

* **[ ] 2D Light Masks:** A circular "Soft Light" for lanterns and a "Conical Beam" for the player's flashlight.
* **[ ] Particle Effects:**
    * *Mining Sparks:* Mechanical yellow sparks.
    * *Gloom Fog:* Moving cloud particles for the map edges.
    * *Warding Pulse:* A circular white/blue shockwave for window completion.
    * *Shard Feedback:* Red electrical arcing for invalid placements.

---

## 3. Audio Assets (SFX & BGM)

### 3.1 Sound Effects (SFX)
* **[ ] UI/Puzzle:** Mechanical "clink" (shard placement), high-pitched "ding" (valid move), and glass shattering (invalid move).
* **[ ] Extraction:** Continuous low-frequency mechanical "whir" (mining), and metal-clanking (bot movement).
* **[ ] Combat:** Flesh squelch (zombie hit), low-pitched groan (husk aggro), and turret "pew" (firing).
* **[ ] Environmental:** The "Dinner Bell" alarm (high heat alert), and the "Holy Hum" (window completion).

### 3.2 Music (BGM)
* **[ ] "The Sanctum":** A somber, ambient pipe organ and choir track (low intensity).
* **[ ] "The Gloom":** Dark, industrial drone with rhythmic metallic clanging (increases in tempo as Heat rises).

---

## 4. Technical Export Specs

| Asset Type | Format | Notes |
| :--- | :--- | :--- |
| **Spritesheets** | `.png` | Packed with 2px extrusion; `Nearest` filtering. |
| **Tilemaps** | `.json` | Exported from Tiled Map Editor. |
| **Audio (Web)** | `.m4a` / `.ogg` | High-bitrate mono for SFX; Stereo for BGM. |
| **Palette** | **GPL/HEX** | Use a restricted 32-color Gothic palette (e.g., DB32 or customized). |

---

**Next Step:** With the **Asset To-Do List** complete, you can now track your visual progress. Since we are ready to code, should we move to **Sprint 1** and build the **TypeScript Grid Logic** for the shards?