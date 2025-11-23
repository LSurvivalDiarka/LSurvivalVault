<div align="center">

<img src="https://imgur.com/j1Zjtd0.png" alt="LSurvival Vault Logo" width="150"/>

# 📦 LSurvival Vault
### Advanced Virtual Storage for RocketMod 4

![RocketMod](https://img.shields.io/badge/RocketMod-4-blue?style=flat-square&logo=csharp)
![Unturned](https://img.shields.io/badge/Game-Unturned-green?style=flat-square&logo=steam)
![Database](https://img.shields.io/badge/Database-LiteDB_v5-orange?style=flat-square)
![Status](https://img.shields.io/badge/Status-Stable-success?style=flat-square)

<p align="center">
  <b>LSurvival Vault</b> is a high-performance infrastructure plugin designed to manage persistent virtual inventories. 
  <br>
  It utilizes <b>memory injection</b> and <b>NoSQL databases</b> to provide secure, scalable, and lag-free storage solutions.
</p>

---

</div>

## 📑 Table of Contents
- [Overview](#-overview)
- [Key Features](#-key-features)
- [Technical Specifications](#-technical-specifications)
- [Commands & Permissions](#-commands--permissions)
- [Configuration](#-configuration)
- [Installation](#-installation)

---

## 🔭 Overview

**LSurvival Vault** replaces traditional physical storage (barricades) with a virtualized system. By not instantiating physical objects in the game world, it eliminates collision calculations and unnecessary rendering, significantly improving server performance while offering a modern user experience.

> **Core Philosophy:** Seamless integration, atomic data integrity, and combat-aware mechanics.

---

## ✨ Key Features

| Feature | Description |
| :--- | :--- |
| **🧠 In-Memory Operation** | Generates virtual containers in RAM via dependency injection. No physical barricades, no lag. |
| **🛡️ Combat Manager** | Monitors damage events. If a player takes PvP damage, access to the vault is locked for a configurable duration. |
| **💾 LiteDB Persistence** | Uses **LiteDB v5** (NoSQL). Replaces fragile flat files with transactional, atomic database operations. |
| **🔄 Auto-Save** | Asynchronous background saving every 60 seconds (configurable) to minimize data loss during crashes. |
| **👮 Admin Tools** | Real-time inspection. Admins can open offline players' vaults or "spy" on active vaults with shared memory updates. |
| **📈 Scalability** | Supports up to **18 independent vaults** per player, each with custom dimensions (Width x Height). |

---

## 📋 Technical Specifications

### Storage Architecture (Mock Storage)
* **Dynamic Positioning:** The virtual container dynamically positions itself relative to the player to satisfy server-side distance checks without physical instantiation.
* **Zero Collision:** Completely removes the physics overhead associated with standard metal lockers or crates.

### Data Integrity
* **Atomic Transactions:** Read/Write operations are transaction-safe.
* **Unified Storage:** All data is centralized in a single, portable file:  
    `.../Plugins/LSurvivalVault/Data/LSurvivalVault.db`

---

## ⌨️ Commands & Permissions

### 👤 User Commands

| Command | Syntax | Description | Permission Node |
| :--- | :--- | :--- | :--- |
| **Open Default** | `/vault` | Opens the default vault (ID 1). | `lsurvival.vault.1` |
| **Open Specific** | `/vault <id>` | Opens a specific vault number. | `lsurvival.vault.<id>` |

> **Note:** Permissions follow the specific ID format. Example: To grant access to vault #5, use `lsurvival.vault.5`.

### 🛡️ Admin Commands

| Command | Syntax | Description | Permission Node |
| :--- | :--- | :--- | :--- |
| **Inspect** | `/adminvault <player> <id>` | Force open any player's vault (Online/Offline). | `lsurvival.admin` |

---

## ⚙️ Configuration

The `LSurvivalVault.configuration.xml` controls physics, cooldowns, and capacity.

### 1. Vault Definitions
Define the size of each vault tier here.

```xml
<Vaults>
    <VaultDefinition>
        <Id>1</Id>
        <Width>5</Width>
        <Height>5</Height>
    </VaultDefinition>

    <VaultDefinition>
        <Id>2</Id>
        <Width>8</Width>
        <Height>8</Height>
    </VaultDefinition>
</Vaults>
