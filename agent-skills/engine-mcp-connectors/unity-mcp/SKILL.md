---
name: unity-mcp
description: Model Context Protocol (MCP) bridge for Unity Editor. Enables scene hierarchy inspection, GameObject and Component manipulation, Prefab instantiation, AssetDatabase management, and automated Playmode test execution.
---

# Unity MCP Connector Skill

The **Unity MCP Connector** bridges AI agent capabilities with the Unity Editor runtime. It enables real-time scene inspection, procedural scene assembly, automated component configuration, and Playmode test execution.

---

## 1. Architecture & Communication Protocol

```mermaid
flowchart LR
    Agent["🤖 Antigravity Agent"] <-->|MCP JSON-RPC (stdio / HTTP)| Bridge["🔌 Unity MCP Node Host"]
    Bridge <-->|WebSocket: localhost:8080| UnityEditor["🎮 Unity Editor\n(EditorWindow / EditorApplication)"]
    UnityEditor <--> UnityScene["🌳 Active Scene & AssetDatabase"]
```

The Unity Editor integration runs on the editor thread via `EditorApplication.update` callbacks, ensuring that all operations are thread-safe and properly recorded within Unity's `Undo` system.

---

## 2. Standard MCP Tools Exposed

### `unity_get_scene_hierarchy`
Retrieves the complete or scoped GameObject hierarchy of the open scene.
- **Input**:
  ```json
  {
    "root_only": true,
    "include_components": true
  }
  ```
- **Output**:
  ```json
  {
    "active_scene": "MainLevel",
    "root_count": 2,
    "game_objects": [
      {
        "instance_id": 1024,
        "name": "Directional Light",
        "tag": "Untagged",
        "components": ["Transform", "Light"]
      },
      {
        "instance_id": 1028,
        "name": "PlayerArmature",
        "tag": "Player",
        "components": ["Transform", "CharacterController", "PlayerInput", "PlayerMovement"]
      }
    ]
  }
  ```

### `unity_instantiate_prefab`
Loads an asset from `Assets/` and instantiates it into the active scene.
- **Input**:
  ```json
  {
    "prefab_path": "Assets/MasterGameStudio/Props/SM_Chest.prefab",
    "position": [0.0, 1.5, -4.0],
    "rotation_euler": [0.0, 45.0, 0.0],
    "parent_name": "PropsContainer"
  }
  ```
- **Output**:
  ```json
  {
    "status": "success",
    "instance_id": 2048,
    "name": "SM_Chest",
    "world_position": [0.0, 1.5, -4.0]
  }
  ```

### `unity_refresh_asset_database`
Forces Unity to re-import newly created external files (glTF, textures, scripts).
- **Output**:
  ```json
  {
    "status": "success",
    "imported_count": 3
  }
  ```

### `unity_run_playmode_test`
Enters Playmode, executes designated unit or integration test suites, and returns results.
- **Input**:
  ```json
  {
    "test_suite": "MasterStudio.Tests.QuestSmokeTests",
    "timeout_seconds": 30
  }
  ```
- **Output**:
  ```json
  {
    "passed": true,
    "total_tests": 4,
    "duration_ms": 1240,
    "failures": []
  }
  ```

---

## 3. Unity Project Integration Steps

1. In your Unity project, ensure the package `com.unity.nuget.newtonsoft-json` is installed.
2. Place `UnityMcpBridge.cs` in `Assets/Editor/MasterGameStudio/`.
3. Open Unity: Select **Window > Game Studio > Start MCP Server**.
4. The server begins listening on `http://127.0.0.1:8080/mcp`.
