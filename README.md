# 📦 LSurvival Vault (RocketMod 4)

![LSurvival Vault Icon](https://i.imgur.com/tGFsdnA.png)



### La Solución Definitiva de Almacenamiento Virtual para Servidores de Alto Rendimiento.

**LSurvival Vault** no es otro plugin de `/vault` más. Es una reingeniería total del concepto de almacenamiento personal, diseñado específicamente para servidores **Survival, PvP y Hardcore** que no pueden permitirse fallos, lag ni dupeos.

Olvídate de las barricadas físicas bajo el mapa y los glitches de duplicación. Este sistema es **100% Virtual, Seguro y Escalable**.

-----

## 🔥 Características Principales

  * 🚀 **Tecnología "Zero-Lag":** A diferencia de otros plugins, este **NO spawnea cajas físicas** en el mapa. Todo ocurre en la memoria, lo que significa **0 impacto en los FPS** del servidor, sin importar cuántos jugadores lo usen a la vez.
  * 🔒 **Blindado contra Dupeos:** Sistema de persistencia inteligente con **Auto-Guardado** cada 60 segundos. Si tu servidor crashea, los items de tus jugadores están seguros.
  * 🎒 **Sistema Multi-Vault Escalable (1-18):** Configura hasta **18 baúles diferentes** con tamaños personalizados. Desde una mochila pequeña (5x5) hasta un almacén gigante (12x14).
  * ⚔️ **Protección PvP "Fair Play":** Evita el *Combat Stashing*. Si un jugador recibe daño de otro usuario, el baúl se bloquea temporalmente. ¡Se acabaron los cobardes que guardan el loot antes de morir\!
  * 👮 **Herramientas de Administración:** Inspecciona el baúl de cualquier jugador (online u offline) en tiempo real con un solo comando. Ideal para detectar robos o dar soporte.
  * 🎨 **Totalmente Personalizable:** Configura mensajes, colores, iconos del chat, cooldowns y tamaños a tu gusto.

-----

## 📜 Comandos y Permisos

### 👤 Para Jugadores

| Comando | Sintaxis | Descripción | Permiso Requerido |
| :--- | :--- | :--- | :--- |
| **/vault** | `/vault` | Abre tu Baúl Principal (ID 1). | `lsurvival.vault.1` |
| **/vault** | `/vault [id]` | Abre un baúl específico (Ej: `/vault 2`, `/vault 5`). | `lsurvival.vault.[id]` |

> **Nota:** El sistema de permisos es dinámico. Si quieres que un VIP tenga acceso al Baúl 5, solo dale el permiso `lsurvival.vault.5`.

### 🛡️ Para Administradores

| Comando | Sintaxis | Descripción | Permiso Requerido |
| :--- | :--- | :--- | :--- |
| **/adminvault** | `/adminvault [jugador] [id]` | Abre el baúl de otro jugador para inspeccionarlo o modificarlo. Funciona incluso si el jugador está desconectado. | `lsurvival.admin` |

-----

## ⚙️ Configuración Avanzada

El archivo `LSurvivalVault.configuration.xml` te da control total sobre la experiencia:

### 📏 Tamaños de Baúles (Progresión)

Puedes definir el tamaño exacto (Ancho x Alto) para cada uno de los 18 baúles.

  * *Ejemplo:* Haz que el **Vault 1** sea pequeño (5x5) para usuarios gratis y el **Vault 2** sea grande (10x10) para VIPs.

<!-- end list -->

```xml
<Vaults>
  <VaultDefinition><Id>1</Id><Width>5</Width><Height>5</Height></VaultDefinition>
  <VaultDefinition><Id>2</Id><Width>10</Width><Height>10</Height></VaultDefinition>
  ...
</Vaults>
```

### ⚔️ Sistema de Combate

  * **BlockInCombat:** `true` / `false` (Activa el bloqueo en PvP).
  * **CombatCooldownSeconds:** Tiempo en segundos que el jugador debe esperar tras recibir un golpe (Ej: `30`).

### 💾 Seguridad de Datos

  * **AutoSaveIntervalSeconds:** Frecuencia con la que el servidor guarda los baúles abiertos para prevenir pérdida de datos por crashes (Recomendado: `60`).

-----

## 📥 Instalación

1.  Apaga tu servidor.
2.  Copia **`LSurvivalVault.dll`** en la carpeta `.../Rocket/Plugins/`.
3.  Copia la librería **`LiteDB.dll`** en la carpeta `.../Rocket/Libraries/`.
4.  Enciende el servidor.
5.  Configura los permisos en `Permissions.config.xml` y ¡listo\!

-----

**LSurvival Vault** — *Calidad profesional para servidores que se toman el juego en serio.*
