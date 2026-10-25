---
name: godot-mcp
description: Model Context Protocol (MCP) bridge for Godot 4. Enables SceneTree queries, node creation, GDScript execution, Resource serialization, and debug console inspection.
---

# Godot 4 MCP Connector Skill

The **Godot 4 MCP Connector** links AI agents directly to the Godot 4 Editor and running game instances via an asynchronous TCP/WebSocket bridge implemented as a Godot `EditorPlugin`.

---

## 1. Architecture & Protocol

```mermaid
flowchart LR
    Agent["🤖 Antigravity Agent"] <-->|MCP JSON-RPC| Server["🔌 Godot MCP Proxy Host"]
    Server <-->|TCP / WebSocket (Port 9080)| Plugin["🤖 Godot 4 EditorPlugin\n(addons/godot_mcp/)"]
    Plugin <--> GodotTree["🌳 Godot 4 SceneTree & ResourceFileSystem"]
```

The plugin utilizes `EditorInterface` to ensure that node additions and property modifications register with the Godot Undo/Redo history stack and show immediate visual feedback in the 2D/3D viewports.

---

## 2. Standard MCP Tools Exposed

### `godot_get_scene_tree`
Queries the open scene's node tree hierarchy.
- **Input**:
  ```json
  {
    "filter_type": "Node3D",
    "include_groups": true
  }
  ```
- **Output**:
  ```json
  {
    "root_node": "LevelMain",
    "nodes": [
      {
        "name": "DirectionalLight3D",
        "type": "DirectionalLight3D",
        "path": "/root/LevelMain/DirectionalLight3D",
        "groups": ["sun"]
      },
      {
        "name": "Player",
        "type": "CharacterBody3D",
        "path": "/root/LevelMain/Player",
        "groups": ["player", "saveable"]
      }
    ]
  }
  ```

### `godot_instantiate_scene`
Instantiates a packed scene (`.tscn` or imported `.glb`) into the scene tree.
- **Input**:
  ```json
  {
    "scene_path": "res://assets/props/chest.tscn",
    "parent_node_path": "/root/LevelMain/Props",
    "transform": {
      "position": [2.5, 0.0, -1.0],
      "rotation_degrees": [0.0, 90.0, 0.0],
      "scale": [1.0, 1.0, 1.0]
    }
  }
  ```
- **Output**:
  ```json
  {
    "status": "success",
    "node_name": "Chest",
    "node_path": "/root/LevelMain/Props/Chest"
  }
  ```

### `godot_run_gdscript`
Evaluates a GDScript snippet dynamically inside the editor context.
- **Input**:
  ```json
  {
    "script": "return EditorInterface.get_edited_scene_root().name"
  }
  ```
- **Output**:
  ```json
  {
    "result": "LevelMain",
    "status": "success"
  }
  ```

### `godot_scan_filesystem`
Signals the Godot FileSystem dock to scan for external file changes and reimport modified resources.
- **Output**:
  ```json
  {
    "status": "success",
    "message": "FileSystem scan completed."
  }
  ```

---

## 3. Installation in a Godot Project

1. Copy the `addons/godot_mcp` folder into your Godot project's `res://addons/` directory.
2. In Godot 4, open **Project > Project Settings > Plugins**.
3. Locate **"Godot MCP Bridge"** and set its status to **Enabled**.
4. The MCP bridge will start listening on `ws://127.0.0.1:9080`.
