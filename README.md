# Vitreous Gloom: The Necrotic Wardens

**Vitreous Gloom** is an experimental 2D "Post-Apocalyptic Gothic" hybrid game. It blends high-stakes **Extraction RTS** mechanics, **Horde Defense**, and **Pattern-Matching Puzzle Crafting** into a cohesive survival loop. Built for the web using **Phaser 3** and **TypeScript**, it delivers a pixel-art horror experience with a technical focus on modularity and performant web deployment.

---

## 1. The Premise: Light as a Dying Resource

In the world of *Vitreous Gloom*, a necrotic fog known as "The Gloom" has consumed the land, turning all living matter into mindless husks. Humanity’s last bastions are the **Sanctum Cathedrals**—massive stone fortresses protected by ancient, aetherically-charged stained glass windows. These windows act as "Warding Shields," but centuries of decay have cracked them.

You play as a **Vitromancer**, a specialized survivalist-artist tasked with venturing into the dead zones to extract rare "Soul-Infused Ores." You must refine these minerals into glass shards and reconstruct the Great Windows before the light fails and the Cathedrals are overrun.

---

## 2. Core Gameplay Pillars

### A. The Extraction (RTS & Horde Defense)
To build the glass, you need minerals. You must leave the safety of the Sanctum and enter procedural "Gloom Zones."
* **The Siphoners:** You deploy automated worker bots (reminiscent of *Starcraft* SCVs) to mine resource nodes.
* **The Dinner Bell:** Extraction is loud. Mining noise increases a global "Heat" meter, attracting waves of zombies (The Arc Raiders influence). 
* **2D Tactical Defense:** You must build temporary fortifications, traps, and turrets to protect your Siphoners while they finish their collection cycle.

### B. The Crafting (Sagrada Puzzle Mechanics)
Back at the workshop, the resources you've gathered are used in a complex, grid-based crafting system inspired by the board game *Sagrada*.
* **The Ward Grid:** Each cathedral window has a specific pattern requiring shards of specific colors (minerals) and purities (values 1-6).
* **Aetheric Interference:** Based on Sagrada rules, shards of the same color or the same purity value cannot be placed orthogonally adjacent to one another.
* **Volatile Crafting:** Misplacing a shard doesn't just ruin the art—it causes aetheric feedback, attracting ghosts to your base or damaging your character.

### C. The Progression (Terraria-style Tiers)
* **Tier 1 (Lead Glass):** Found in the outskirts; used for minor village shrines and basic gear.
* **Tier 2 (Shadow Iron):** Found in infested catacombs; used to craft windows that unlock advanced alchemical weaponry.
* **Endgame (Sun-Star Ore):** Found at the heart of the Hive; used to craft the "Eternal Solar Window."

---

## 3. Technical Architecture: Why Phaser & TypeScript?

This project is built from the ground up for **Web Deployment**, bypassing the heavy overhead of traditional desktop engines.

### Why Phaser 3?
* **Web Native:** Phaser 3 provides an ultra-lightweight footprint (<2MB engine overhead), ensuring instant-play capability in any modern browser.
* **Lighting & Shaders:** We utilize Phaser’s `Light2D` pipeline to simulate the oppressive horror atmosphere. The game world is dark by default, with dynamic lights attached to player flares, siphoner bots, and the finished glowing windows.
* **Pixel-Perfect Rendering:** Designed for retro aesthetics, the engine ensures crisp pixel art without blur or shimmering.

### Why TypeScript?
* **Scalable Architecture:** Managing the state between an RTS, a Puzzle, and a Base-Builder requires strict data typing. TypeScript ensures that the `Resource` type extracted in the field matches the `Shard` type required in the workshop.
* **Modular "Lego" Design:** The game is built using a **Component-Based Architecture**. Each system (GridManager, ThreatManager, ConstructionManager) is a standalone TypeScript class that communicates via a central **Event Bus**.

---

## 4. Development Roadmap (Sprint-Based)

The project is developed in "Lego-brick" phases to ensure a playable prototype at every stage:

1.  **Sprint 1: The Shard Grid:** Implementation of the 5x4 Sagrada grid and adjacency validation logic.
2.  **Sprint 2: The Siphoner:** Movement AI and resource extraction logic for worker bots.
3.  **Sprint 3: The Gloom:** Implementation of the "Heat" system, zombie pathfinding, and 2D lighting.
4.  **Sprint 4: The Sanctum:** Tilemap-based base building and persistent inventory systems.
5.  **Sprint 5: The Loop:** Connecting the resource tiers and the "Order" system for window patterns.

---

## 5. Deployment

* **Primary Target:** Web (HTML5/Canvas).
* **Secondary Target:** Progressive Web App (PWA) for offline capability.
* **Build Tool:** Vite + TypeScript for fast hot-reloading and optimized production bundles.
