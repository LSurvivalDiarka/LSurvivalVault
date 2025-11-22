# 📦 LSurvival Vault (RocketMod 4)

![LSurvival Vault Icon](https://i.imgur.com/tGFsdnA.png)


# 📦 LSurvival Vault (RocketMod 4)

**Sistema de Almacenamiento Virtual Persistente para Servidores Unturned.**

LSurvival Vault es un plugin de infraestructura diseñado para gestionar inventarios virtuales adicionales para los jugadores. Utiliza una arquitectura de inyección de memoria y bases de datos NoSQL para proporcionar un almacenamiento seguro, escalable y de alto rendimiento.

---

## 📋 Especificaciones Técnicas y Características

### 🛠️ Arquitectura de Almacenamiento (Mock Storage)
El sistema genera contenedores de almacenamiento virtuales directamente en la memoria RAM del servidor mediante inyección de dependencias.
* **Funcionamiento en Memoria:** No instancia objetos físicos (barricadas) en el mundo del juego, eliminando colisiones y renderizado innecesario.
* **Sincronización:** El contenedor virtual se posiciona dinámicamente sobre el jugador para cumplir con las validaciones de distancia del servidor.

### 💾 Persistencia de Datos (LiteDB)
El plugin sustituye el almacenamiento de archivos planos por **LiteDB v5**, una base de datos embebida transaccional.
* **Integridad de Datos:** Utiliza transacciones atómicas para operaciones de lectura/escritura.
* **Auto-Guardado (Auto-Save):** Ejecuta un ciclo de guardado automático cada 60 segundos (configurable) para volcar los datos de la memoria RAM al disco, minimizando la pérdida de datos ante interrupciones del servidor.
* **Estructura Unificada:** Todos los datos se centralizan en un único archivo de base de datos (`LSurvivalVault.db`) ubicado en la carpeta `Data` del plugin.

### ⚔️ Gestión de Combate (PvP Manager)
Integra un monitor de eventos de daño para regular el acceso al almacenamiento durante situaciones de combate.
* **Detección de Fuente:** Identifica específicamente el daño proveniente de otros jugadores (PvP), ignorando daños ambientales o de zombies.
* **Bloqueo Temporal:** Impide la ejecución del comando de apertura durante un periodo de tiempo configurable tras recibir daño.

### 👮 Administración en Tiempo Real
Herramientas para la gestión y auditoría de inventarios por parte del personal administrativo.
* **Inspección Remota:** Permite abrir el inventario de cualquier jugador, independientemente de si está conectado o desconectado.
* **Memoria Compartida:** Si un administrador abre el baúl de un jugador que está usándolo activamente, ambos clientes comparten la misma instancia de memoria, permitiendo ver modificaciones en tiempo real.

### 📈 Escalabilidad (Sistema Multi-Vault)
El sistema soporta la configuración de múltiples instancias de almacenamiento por jugador.
* **Capacidad Dinámica:** Permite definir hasta **18 baúles independientes**.
* **Dimensiones Personalizables:** Cada ID de baúl puede tener un tamaño de cuadrícula único (Ancho x Alto), configurado desde el archivo XML.

---

## 📜 Comandos y Permisos

### Usuario

| Comando | Sintaxis | Descripción | Permiso Requerido |
| :--- | :--- | :--- | :--- |
| **/vault** | `/vault` | Accede al baúl predeterminado (ID 1). | `lsurvival.vault.1` |
| **/vault** | `/vault [id]` | Accede a un baúl específico por su número. | `lsurvival.vault.[id]` |

> **Nota:** Los permisos siguen el formato `lsurvival.vault.<numero>`. Ejemplo: `lsurvival.vault.5`.

### Administración

| Comando | Sintaxis | Descripción | Permiso Requerido |
| :--- | :--- | :--- | :--- |
| **/adminvault** | `/adminvault [jugador] [id]` | Abre el baúl especificado del jugador objetivo para gestión. | `lsurvival.admin` |

---

## ⚙️ Configuración (XML)

El archivo `LSurvivalVault.configuration.xml` controla los parámetros operativos del plugin.

### Definición de Baúles
Lista que define las propiedades físicas de cada contenedor disponible.

```xml
<Vaults>
  <VaultDefinition>
    <Id>1</Id>
    <Width>5</Width>   <Height>5</Height>  </VaultDefinition>
  
  <VaultDefinition>
    <Id>2</Id>
    <Width>8</Width>
    <Height>8</Height>
  </VaultDefinition>
  
  </Vaults>
````

### Parámetros del Sistema

````xml
<BlockInCombat>true</BlockInCombat>             <CombatCooldownSeconds>30</CombatCooldownSeconds> <AutoSaveIntervalSeconds>60</AutoSaveIntervalSeconds> <StorageAssetId>1283</StorageAssetId> ```

---

## 📥 Guía de Despliegue

Para la correcta instalación en un entorno de producción RocketMod 4:

1.  **Dependencias:**
    * El plugin requiere la librería **`LiteDB.dll`** (Versión 5.0.x para .NET 4.6.1).
    * Ubicación requerida: Carpeta `/Libraries` de la instancia de Rocket.

2.  **Instalación del Plugin:**
    * Archivo `LSurvivalVault.dll` en la carpeta `/Plugins`.

3.  **Inicialización:**
    * Al iniciar, el plugin generará automáticamente la estructura de carpetas:
        * Configuración: `/Plugins/LSurvivalVault/`
        * Base de Datos: `/Plugins/LSurvivalVault/Data/LSurvivalVault.db`
````
