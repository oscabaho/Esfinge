# 🏛️ Project Sphinx — Technical Standards & Architecture Guidelines

![Version](https://img.shields.io/badge/Version-1.2-blue)
![Engine](https://img.shields.io/badge/Engine-Unreal%20Engine%205.6.1-purple)
![Language](https://img.shields.io/badge/Language-EN%20%2F%20ES-green)

> **Internal language:** English | **Lore language:** Spanish
> **Core Philosophy:** Modularity, Scalability, and Defensive Programming.

---

## 📋 Table of Contents

1. [Zero Third-Party Asset Policy](#1-zero-third-party-asset-policy)
2. [Core Architecture & Design Patterns](#2-core-architecture--design-patterns)
3. [Enhanced Naming Conventions](#3-enhanced-naming-conventions-assets)
4. [Workstation & GitHub Workflow](#4-workstation--github-workflow-ue-561)
5. [Environment & Tech Specs](#5-environment--tech-specs)

---

## 1. Zero Third-Party Asset Policy

To ensure **100% original authorship**, the following rules apply:

| Rule | Description |
| --- | --- |
| ✅ **Final Content** | Every Mesh, Texture, Material, Sound, and Animation in the final build must be created **from scratch** by the team. |
| 🟡 **Placeholders** | Third-party assets (packs, Megascans, etc.) are allowed **ONLY** as temporary placeholders during the **Greybox** phase. |
| 🔴 **Purge Responsibility** | The team member who imports a placeholder asset is **strictly responsible** for deleting it and cleaning up all references before final build submission. |

---

## 2. Core Architecture & Design Patterns

We follow strict Object-Oriented principles and SOLID design to ensure our Blueprints remain scalable and bug-free.

### 🏗️ 2.1. The Observer Pattern (Reactive UI)

We **strictly prohibit** the use of `Event Tick` for updating User Interfaces or polling states.

* **The Rule:** Data holders (like `AC_Inventory` or `AC_Health`) must broadcast changes using **Event Dispatchers** (e.g., `OnInventoryChanged`, `OnItemPickedUp`).
* **The Reaction:** UI Widgets (`WBP_DetectionBar`, `WBP_Objective`) bind to these dispatchers upon creation and update themselves *only* when the event fires.

### 🖥️ 2.2. Modular UI Orchestration (The Hub Pattern)

We do not inject widgets directly from the `BP_Player` or `PlayerController`. This causes monolithic "Spaghetti Code".

* **WBP_GameLayout:** This is our **Master UI Hub**. It contains an `Overlay` panel (`HUDLayer`).
* **Dynamic Injection:** The GameLayout is responsible for spawning sub-widgets (`WBP_Inventory`, `WBP_Objective`) and injecting them into the `HUDLayer` using `Add Child to Overlay` and manipulating `Overlay Slots` for alignment and padding.
* **Result:** The Player Blueprint is completely unaware of the UI's internal structure.

### 🛡️ 2.3. Defensive Programming (Safety Checks)

* **Static Composition Support:** Every Widget must check if its dependencies are valid (`Is Valid` node) during `Event Construct` before attempting to bind to them.
* **Why?** This prevents fatal *Accessed None* errors and allows designers to drag-and-drop widgets inside the UMG Designer to preview them safely without crashing the editor.

### 🎯 2.4. Visual & Rendering Standards

* **Interactable Outlines:** We use **CustomStencil** rendering combined with a Laplace edge-detection Post Process Material. This ensures that item outlines remain a consistent pixel thickness regardless of camera distance.
* **Text Formatting:** Always use `Wrap Text At` in TextBlocks (e.g., `WBP_PickupToast`) to prevent long item names or localization differences from breaking the layout. Auto-wrap is prone to engine bugs.

---

## 3. Enhanced Naming Conventions (Assets)

> Follow the **`Prefix_Name`** pattern at all times.

### 🧠 Logic & Data

| Asset Type | Prefix | Example |
| --- | --- | --- |
| Blueprint Class | `BP_` | `BP_Kimovish_Player` |
| Actor Component | `AC_` | `AC_Inventory` |
| Blueprint Interface | `BPI_` | `BPI_Interactable` |
| Data Asset | `DA_` | `DA_Item_Details` |
| Structure | `S_` | `S_ItemData` |
| Enumeration | `E_` | `E_PlayerState` |

### 🎨 Visuals & UI

| Asset Type | Prefix | Example |
| --- | --- | --- |
| Static Mesh | `SM_` | `SM_Lab_Door` |
| Skeletal Mesh | `SK_` | `SK_Elena_Abomination` |
| Material | `M_` | `M_Highlight_PP` |
| Material Instance | `MI_` | `MI_Wall_Dirty_01` |
| Texture | `T_` | `T_Concrete_BC` |
| Widget Blueprint | `WBP_` | `WBP_GameLayout` |

*(Note: Animations `AS_`, Audio `S_`, etc., remain standard as per UE documentation).*

---

## 4. Workstation & GitHub Workflow (UE 5.6.1)

> ⚠️ Blueprint files are **binary** and cannot be merged. This workflow minimizes conflicts.

### 🧩 4.1. Component-Based Workstation Pattern

Do not code heavy logic inside the main `BP_Player`. Instead, create specific **Actor Components** (`AC_OxygenSystem`, `AC_Inventory`, `AC_MaskHealth`).

* **The Benefit:** Multiple developers can work on the player simultaneously without locking the `BP_Player` file. You just edit your specific component.

### 🔒 4.2. File Locking Communication

Before opening a core Blueprint or Level, explicitly notify the team in the designated channel.

> *Example: "Locking `WBP_GameLayout` — DO NOT TOUCH"*

### 📤 4.3. Commit Strategies

* **Atomic Commits:** Push **small, working changes often**. Never wait days to push a massive update.
* **Commit Messages:** Short and descriptive, stating *what* was changed and *why*.

  > *Example: `REFACTOR: Migrated UI creation from BP_Player to WBP_GameLayout to decouple architecture`*

---

## 5. Environment & Tech Specs

| Spec | Value |
| --- | --- |
| **Lighting** | Static only. Lightmaps must be **baked** for final delivery. |
| **Scale** | `1 Unreal Unit = 1 cm` |
| **Pivot Points** | All props must have their pivot at the **base or center** for easy placement. |
| **Collisions** | Custom `UCX_` collisions for complex meshes to keep the **NavMesh clean** for AI. |

---

## 6. Evaluation Feedback & Iterations (Version 1.2)

This section documents external design feedback and the technical/artistic solutions implemented to address each point, ensuring continuous improvement of the project.

### 🎯 Objective Clarity & Visual Feedback

**Feedback:** *"No es claro cómo completar el arma ni el objetivo final. Falta claridad visual en objetos recolectables."*

* **Visual Clarity (Custom Stencil):** Implemented a highly optimized Raycast system that triggers a Custom Stencil render when looking at interactable items. A Laplace edge-detection post-process draws a consistent outline around the item, ensuring it stands out regardless of lighting or distance.
* **Objective Tracking (`WBP_Objective`):** Added a dynamic HUD tracker that counts required components (e.g., "Buscando recursos X/4"), keeping the player constantly informed of their immediate goal.
* **Real-time Toasts (`WBP_PickupToast`):** Created a Reactive UI Toast Manager that displays elegant, temporary side-screen notifications whenever a vital item is collected.

### 📡 Holographic Diegetic Radar System

**Goal:** Provide immersive, diegetic spatial awareness without cluttering the HUD.

* **Decoupled Logic (`AC_RadarSystem`):** A dedicated Actor Component handles overlap detection, spatial math (world to local relative coordinates), and a 45-second cooldown mechanism. It broadcasts state changes strictly via Event Dispatchers (`OnRadarActivated`, `OnRadarDeactivated`, `OnRadarPing`).
* **Target Interface (`BPI_StealthTarget`):** Used to securely validate and filter which actors appear on the radar, automatically ignoring the owning player's overlaps to prevent self-detection bugs.
* **State-Driven UI (`WBP_Radar`):** The widget strictly acts as a listener. It uses robust `TimerHandles` (replacing unreliable latent Delays) to manage its animation lifecycle, ensuring seamless fade-in/fade-out transitions and safe state cancellation.
* **Hub Integration (`WBP_GameLayout`):** The Master Hub dynamically instantiates the radar widget at runtime, anchors it cleanly to the `HUDLayer`, and binds to the component's dispatchers, maintaining our perfect architectural decoupling.

### ⚙️ Technical & Environment Fixes

* **Camera Issues:** *"Problemas en la cámara en primera persona."*
  * **Solution (Implemented):** Modified the camera behavior to adopt a 100% pure first-person perspective, completely removing the previous unstable hybrid (3rd/1st person) mechanics and fixing clipping issues.
* **Material Warnings:** *"Advertencias de materiales (Nanite)."*
  * **Solution (Implemented):** Resolved console warnings by explicitly disabling Nanite on incompatible materials (such as translucency and complex masks) while maintaining performance.
* **Content Organization:** *"Organizar Content Browser."*
  * **Solution (Implemented):** Executed a strict cleanup, organizing folders and assets with highly descriptive naming conventions to maintain a professional workspace.
* **Prop Scale Inconsistency:** *"Escala inconsistente de props."*
  * **Status (In Progress):** The Art Team is currently in charge of this standard. Several updated asset deliveries have been integrated, but the task remains open until the final art batches are delivered.
