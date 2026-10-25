---
name: unreal-mcp
description: Model Context Protocol (MCP) bridge for Unreal Engine 5. Integrates with UE5 Python Remote Execution and Web Remote Control to inspect levels, spawn Actors, configure Nanite/Lumen, and manage Blueprints.
---

# Unreal Engine 5 MCP Connector Skill

The **Unreal Engine 5 MCP Connector** provides deep integration with the UE5 Editor environment using the built-in **Python Remote Execution** protocol and the **Web Remote Control** subsystem.

---

## 1. Architecture & Protocol

```mermaid
flowchart LR
    Agent["🤖 Antigravity Agent"] <-->|MCP JSON-RPC| Server["🔌 UE5 MCP Server Host"]
    Server <-->|Remote Python / Web Remote Control (HTTP: 30010)| UE5["🌌 Unreal Engine 5 Editor\n(Editor Subsystems)"]
    UE5 <--> UELandscape["🌍 Level, Actors, Nanite/Lumen, Asset Registry"]
```

The bridge interacts with the `unreal` Python module inside the Unreal Editor, providing access to `unreal.EditorLevelLibrary`, `unreal.EditorAssetLibrary`, and `unreal.Subsystem`.

---

## 2. Standard MCP Tools Exposed

### `unreal_execute_python`
Executes an arbitrary Python automation script inside the live Unreal Editor context.
- **Input**:
  ```json
  {
    "script": "import unreal\nactors = unreal.EditorLevelLibrary.get_all_level_actors()\nprint(f'Total actors: {len(actors)}')"
  }
  ```
- **Output**:
  ```json
  {
    "status": "success",
    "output": "Total actors: 142"
  }
  ```

### `unreal_get_actors_in_level`
Returns all actors in the currently loaded world, optionally filtered by class or tag.
- **Input**:
  ```json
  {
    "class_filter": "StaticMeshActor",
    "tag_filter": "Interactable"
  }
  ```
- **Output**:
  ```json
  {
    "world_name": "Main_Persist",
    "actors": [
      {
        "label": "SM_Chest_01",
        "class": "StaticMeshActor",
        "location": [120.0, -450.0, 32.0],
        "rotation": [0.0, 0.0, 45.0],
        "static_mesh": "/Game/MasterGameStudio/Props/SM_Chest.SM_Chest"
      }
    ]
  }
  ```

### `unreal_spawn_actor`
Spawns a new Actor, Blueprint instance, or StaticMesh in the persistent level.
- **Input**:
  ```json
  {
    "asset_path": "/Game/MasterGameStudio/Props/SM_Chest",
    "location": [200.0, 100.0, 0.0],
    "rotation": [0.0, 0.0, 0.0],
    "enable_nanite": true
  }
  ```
- **Output**:
  ```json
  {
    "status": "success",
    "actor_label": "SM_Chest_02",
    "guid": "A3E45F20-B812-4C10-9111-AA760124FEA1"
  }
  ```

### `unreal_query_asset_registry`
Searches the Unreal Asset Registry for assets matching paths, classes, or metadata tags.
- **Input**:
  ```json
  {
    "package_path": "/Game/MasterGameStudio",
    "recursive": true
  }
  ```
- **Output**:
  ```json
  {
    "asset_count": 8,
    "assets": [
      {
        "name": "SM_Chest",
        "class": "StaticMesh",
        "package_name": "/Game/MasterGameStudio/Props/SM_Chest"
      }
    ]
  }
  ```

---

## 3. UE5 Project Configuration

1. In your Unreal Engine 5 project, open **Edit > Plugins**.
2. Enable the following built-in plugins:
   - **Python Editor Script Plugin** (`PythonScriptPlugin`)
   - **Editor Scripting Utilities** (`EditorScriptingUtilities`)
   - **Remote Control API** (`RemoteControl`)
3. In **Project Settings > Python**:
   - Check **Enable Remote Execution**.
   - Ensure the multicast group is configured (default: `239.0.0.1:6766` or HTTP `30010`).
