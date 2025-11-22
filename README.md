# LSurvival Vault - Plugin Simple para Unturned

![LSurvival Vault Icon](https://i.imgur.com/tGFsdnA.png)

Plugin minimalista de **vault personal único** para Unturned 3 con OpenMod 3.8.x.

## 🎯 Características

- ✅ **Un vault por jugador**: Inventario personal persistente
- ✅ **Comando `/vault`**: Abre/cierra el vault (toggle)
- ✅ **UI nativa de Unturned**: Inventario tipo almacén (storage)
- ✅ **Persistencia JSON segura**: Guardado atómico sin duplicación
- ✅ **Anti-dupe**: Sesión única, escritura atómica (.tmp → .json)
- ✅ **Cierre automático**: Al desconectar se guarda y cierra
- ✅ **Sin placeholders**: Implementación completa y funcional

## 📦 Instalación

### Compilación

```bash
cd "c:\Users\sauld\Desktop\Plugins\LSurvivalvault"
dotnet restore
dotnet build -c Release
```

### Despliegue

1. Copiar `bin/Release/netstandard2.1/` a `<Servidor>/OpenMod/plugins/LSurvivalVault/`
2. Reiniciar el servidor
3. Verificar en logs: `LSurvival Vault v1.0.0`

## 🎮 Uso

### Comando

```
/vault
```

**Comportamiento**:
- **Primera ejecución**: Crea y abre vault vacío
- **Con vault abierto**: Cierra y reabre (toggle)
- **Al cerrar inventario**: Se guarda automáticamente

### Permiso

```
lsurvivalvault.vault
```

Por defecto: habilitado para todos los jugadores.

## ⚙️ Configuración

Editar `appsettings.json`:

```json
{
  "LSurvivalVault": {
    "DataFolderName": "data/vaults",
    "VaultWidth": 6,
    "VaultHeight": 4,
    "PollingIntervalSeconds": 1,
    "Messages": {
      "VaultOpened": "Has abierto tu vault.",
      "VaultClosed": "Vault cerrado y guardado.",
      "VaultReloaded": "Vault recargado.",
      "VaultAlreadyOpen": "Ya tienes el vault abierto.",
      "VaultError": "No se pudo abrir tu vault. Contacta con un administrador.",
      "NotPlayer": "Este comando solo puede ser usado por jugadores."
    }
  }
}
```

### Opciones

| Opción | Descripción | Default |
|--------|-------------|---------|
| `DataFolderName` | Carpeta de datos | `data/vaults` |
| `VaultWidth` | Columnas del vault | `6` |
| `VaultHeight` | Filas del vault | `4` |
| `PollingIntervalSeconds` | Intervalo de verificación de cierre | `1` |

## 🔒 Sistema Anti-Dupe

### Protecciones Implementadas

1. **Sesión única por jugador**:
   - Dictionary en memoria (`VaultSessionManager`)
   - Solo una sesión activa a la vez
   - Al abrir con sesión existente → cierra automáticamente

2. **Guardado atómico**:
   ```
   Serializar → Escribir a .tmp → File.Move(tmp, json, overwrite:true)
   ```
   - Si falla: archivo original intacto
   - Operación atómica en Windows

3. **Cierre automático al desconectar**:
   - `PlayerDisconnectListener` → evento `UnturnedPlayerDisconnectedEvent`
   - Guarda y destruye barricada virtual
   - Elimina sesión de memoria

4. **Cleanup de barricadas huérfanas**:
   - Al cargar el plugin, destruye barricadas en coordenadas (9999, 9999, 9999)
   - Evita acumulación tras crashes

## 📁 Estructura de Archivos

```
LSurvivalVault/
├── LSurvivalVault.csproj
├── plugin.yaml
├── appsettings.json
├── LSurvivalVaultPlugin.cs
│
├── Commands/
│   └── VaultCommand.cs
│
├── Models/
│   ├── VaultItemRecord.cs
│   ├── VaultRecord.cs
│   └── VaultSession.cs
│
├── Persistence/
│   ├── IVaultRepository.cs
│   └── JsonVaultRepository.cs
│
├── Services/
│   ├── IVaultService.cs
│   ├── VaultService.cs
│   ├── IVaultSessionManager.cs
│   └── VaultSessionManager.cs
│
└── Listeners/
    └── PlayerDisconnectListener.cs
```

**Total: 16 archivos**

## 🗂️ Formato de Datos

### Archivo JSON

```
OpenMod/plugins/LSurvivalVault/data/vaults/vault_{steamId}.json
```

**Ejemplo**:

```json
{
  "playerId": "76561198123456789",
  "items": [
    {
      "itemId": 363,
      "amount": 1,
      "quality": 100,
      "state": "..."
    }
  ],
  "lastSavedUtc": "2025-01-17T12:34:56.789Z"
}
```

## 🛠️ Implementación Técnica

### Creación de Vault Virtual

1. Spawneo de barricada en coordenadas ocultas (9999, 9999, 9999)
2. Uso de `ItemStorageAsset` (caja de madera, ID 328)
3. Configuración de dimensiones customizadas
4. Población con items cargados del JSON

### Sincronización de Items

**Al abrir**:
```csharp
foreach (var itemRecord in vault.Items)
{
    var item = new Item(itemRecord.ItemId, itemRecord.Amount,
                       itemRecord.Quality, itemRecord.State);
    storage.items.tryAddItem(item);
}
```

**Al cerrar**:
```csharp
for (byte page = 0; page < storage.items.getItemCount(); page++)
{
    var item = storage.items.getItem(page);
    vaultRecord.Items.Add(new VaultItemRecord(...));
}
```

### Detección de Cierre

Polling cada segundo:
```csharp
while (hasSession)
{
    await UniTask.Delay(1s);
    if (!player.inventory.isStoring)
        await CloseVaultAsync();
}
```

## ⚠️ Solución de Problemas

### "INTERNAL ERROR" al ejecutar /vault

1. Revisar logs en `OpenMod/logs/latest.log`
2. Verificar que existe un `ItemStorageAsset` (ID 328)
3. Comprobar permisos de la carpeta `data/vaults`

### Items no se guardan

1. Verificar que el jugador cierra el inventario (ESC)
2. Revisar logs para errores de escritura
3. Comprobar espacio en disco

### Barricadas visibles en el mapa

El plugin usa coordenadas (9999, 9999, 9999) que están fuera del mapa. Si son visibles, verificar:
1. Tamaño del mapa (`Regions.WORLD_SIZE`)
2. Ajustar coordenadas si es necesario

## 📊 Estadísticas del Proyecto

| Métrica | Valor |
|---------|-------|
| Total archivos | 16 |
| Líneas de código | ~800 |
| Clases | 13 |
| Interfaces | 3 |
| Dependencias NuGet | 5 |

## 🔄 Roadmap

- [ ] Comando admin para ver/editar vaults de otros jugadores
- [ ] Límite de peso/items configurable
- [ ] Cooldown entre aperturas
- [ ] Integración con economía (costo por apertura)
- [ ] Logs de auditoría (quién abrió qué)

## 📝 Licencia

MISHUMORE License - Solo para uso personal 

---

**Desarrollado por LSurvival** | v1.0.0
Plugin minimalista sin placeholders ni código incompleto.
