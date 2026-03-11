# 🏛️ Project Sphinx — Technical Standards & Asset Integrity

![Version](https://img.shields.io/badge/Version-1.1-blue)
![Engine](https://img.shields.io/badge/Engine-Unreal%20Engine%205.6.1-purple)
![Language](https://img.shields.io/badge/Language-EN%20%2F%20ES-green)

> **Internal language:** English | **Lore language:** Spanish

---

## 📋 Table of Contents

1. [Zero Third-Party Asset Policy](#1-zero-third-party-asset-policy)
2. [Naming Conventions](#2-enhanced-naming-conventions-assets)
3. [Programming Standards](#3-programming-standards-blueprints)
4. [GitHub Workflow](#4-github-workflow-ue-561)
5. [Environment & Tech Specs](#5-environment--tech-specs)

---

## 1. Zero Third-Party Asset Policy

To ensure **100% original authorship**, the following rules apply:

| Rule | Description |
| --- | --- |
| ✅ **Final Content** | Every Mesh, Texture, Material, Sound, and Animation in the final build must be created **from scratch** by the team. |
| 🟡 **Placeholders** | Third-party assets (packs, Megascans, etc.) are allowed **ONLY** as temporary placeholders during the **Greybox** phase. |
| 🔴 **Purge Responsibility** | The team member who imports a placeholder asset is **strictly responsible** for deleting it and cleaning up all references before final build submission. |
| 🔍 **Auditing** | Weekly checks will be performed to ensure no "Starter Content" or external folders remain in the `_Sphinx/` directory. |

---

## 2. Enhanced Naming Conventions (Assets)

> Follow the **`Prefix_Name`** pattern at all times. Expand this list as needed.

### 🧠 Logic

| Asset Type | Prefix | Example |
| --- | --- | --- |
| Blueprint Class | `BP_` | `BP_Kimovish_Player` |
| Actor Component | `AC_` | `AC_OxygenSystem` |
| Blueprint Interface | `BPI_` | `BPI_Damageable` |
| Blueprint Function Library | `BFL_` | `BFL_InventoryUtils` |
| Blueprint Macro Library | `BML_` | `BML_MathHelpers` |

### 🎬 Animation

| Asset Type | Prefix | Example |
| --- | --- | --- |
| Animation Blueprint | `ABP_` | `ABP_Elena_Main` |
| Animation Sequence | `AS_` | `AS_Elena_Walk` |
| Animation Montage | `AM_` | `AM_Elena_Attack` |
| Blend Space | `BS_` | `BS_Elena_Locomotion` |

### 🎨 Visuals

| Asset Type | Prefix | Example |
| --- | --- | --- |
| Static Mesh | `SM_` | `SM_Lab_Door` |
| Skeletal Mesh | `SK_` | `SK_Elena_Abomination` |
| Material | `M_` | `M_Master_Concrete` |
| Material Instance | `MI_` | `MI_Wall_Dirty_01` |
| Material Function | `MF_` | `MF_Overlay_Dust` |
| Texture | `T_` | `T_Concrete_BC` *(BaseColor)* |
| Niagara System | `NS_` | `NS_Gas_Leak` |

### 🔊 Audio

| Asset Type | Prefix | Example |
| --- | --- | --- |
| Sound Wave | `S_` | `S_Heartbeat_Loop` |
| Sound Cue | `SC_` | `SC_Elena_Breath` |

### 🤖 Data / AI

| Asset Type | Prefix | Example |
| --- | --- | --- |
| Data Asset | `DA_` | `DA_Item_Details` |
| Structure | `ST_` | `ST_InventorySlot` |
| Enumeration | `E_` | `E_PlayerState` |
| Behavior Tree | `BT_` | `BT_Elena_Logic` |
| Blackboard | `BB_` | `BB_Elena_Vars` |
| Environment Query (EQS) | `EQS_` | `EQS_FindHidingSpot` |

### 🖥️ UI & Misc

| Asset Type | Prefix | Example |
| --- | --- | --- |
| Widget Blueprint | `WBP_` | `WBP_HUD_Main` |
| Camera Shake | `CS_` | `CS_Explosion_Hit` |
| Curve Table | `CT_` | `CT_OxygenDepletion` |

---

## 3. Programming Standards (Blueprints)

- **Variable Names:** Use `PascalCase`. *(e.g., `CurrentPlayerHealth`)*
- **Boolean Prefix:** Always use `b` followed by the state. *(e.g., `bIsDead`, `bHasKey`)*
- **Input Events:** Do **not** use "Input Action" nodes directly in the Character BP. Use the **Enhanced Input System** (`IA_ActionName`).

### 📐 Graph Organization

- ✅ Use **Comments** (`C`) for every logic block.
- ✅ Variables must be **categorized** *(e.g., "Stats", "Inventory", "Debug")*.
- 🔴 **Clean up** unused nodes and variables before pushing to GitHub.

---

## 4. GitHub Workflow (UE 5.6.1)

> ⚠️ Blueprint files are **binary** and cannot be merged. Follow these rules strictly.

- 🔒 **Locking System:** Before opening a Blueprint, notify the team via your communication channel.
  > *Example: "Working on `BP_Elena_AI` — DO NOT TOUCH"*

- 🧩 **Modular Logic:** Use **Actor Components** for specific mechanics *(e.g., Oxygen, Noise Level)*. This allows multiple people to work simultaneously without file conflicts.

- 💬 **Commit Messages:** Short and descriptive.
  > *Example: `FIX: Adjusted Elena's hearing radius`*

- 📤 **Commit Frequency:** Push **small, working changes often**. Never wait days to push a massive update.

---

## 5. Environment & Tech Specs

| Spec | Value |
| --- | --- |
| **Lighting** | Static only. Lightmaps must be **baked** for final delivery. |
| **Scale** | `1 Unreal Unit = 1 cm` |
| **Pivot Points** | All props must have their pivot at the **base or center** for easy placement. *(Responsibility: Camilo)* |
| **Collisions** | Custom `UCX_` collisions for complex meshes to keep the **NavMesh clean** for Elena. |
