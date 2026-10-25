---
name: blender-mcp
description: Model Context Protocol (MCP) bridge for Blender. Enables real-time scene inspection, script execution, procedural asset generation, camera control, and viewport render captures over JSON-RPC.
---

# Blender MCP Connector Skill

The **Blender MCP Connector** provides an intelligent Model Context Protocol interface between AI agents and a live or headless instance of Blender.

---

## 1. Connector Architecture

```mermaid
flowchart LR
    Agent["🤖 Antigravity Agent"] <-->|MCP JSON-RPC (stdio/ws)| Server["🔌 Blender MCP Server\n(Python Daemon)"]
    Server <-->|Local IPC / Port 9876| Addon["🧩 Blender Add-on\n(bpy runtime loop)"]
    Addon <--> LiveScene["📦 Live Blender 4.x Scene"]
```

The bridge operates in two modes:
1. **Interactive Add-on**: Runs inside Blender's modal timer loop (`bpy.app.timers`), allowing non-blocking agent commands while the artist works in the GUI.
2. **Headless Microservice**: Spawns a background Blender process (`blender -b`) accepting JSON-RPC commands for batch asset generation in CI/CD pipelines.

---

## 2. Standard MCP Tools Exposed

### `blender_execute_code`
Executes an arbitrary Python code block within Blender's main thread.
- **Input**:
  ```json
  {
    "code": "import bpy\nbpy.ops.mesh.primitive_monkey_add(size=2, location=(0,0,1))"
  }
  ```
- **Output**:
  ```json
  {
    "status": "success",
    "stdout": "Created Suzanne at Vector((0.0, 0.0, 1.0))"
  }
  ```

### `blender_get_scene_info`
Returns a structural snapshot of the current blend file, including collections, objects, polycounts, and active materials.
- **Output**:
  ```json
  {
    "scene_name": "Scene",
    "object_count": 4,
    "active_object": "SM_Chest_Base",
    "objects": [
      {
        "name": "SM_Chest_Base",
        "type": "MESH",
        "vertices": 348,
        "polygons": 320,
        "materials": ["M_Chest_Wood"],
        "location": [0.0, 0.0, 0.0]
      }
    ]
  }
  ```

### `blender_render_viewport`
Renders an instant viewport snapshot and saves it as a temporary PNG for visual agent feedback.
- **Input**:
  ```json
  {
    "shading_mode": "MATERIAL",
    "camera": "Camera",
    "resolution": [512, 512]
  }
  ```
- **Output**:
  ```json
  {
    "image_path": "scratch/renders/render_preview_01.png"
  }
  ```

---

## 3. Launching the Blender MCP Server

### Method A: Direct Command Line (Headless)
```bash
blender -b --python-expr "
import bpy
# Launch embedded MCP listener on port 9876
from mcp_bridge import start_server
start_server(port=9876)
"
```

### Method B: GUI Addon Setup
1. Copy `bpy_mcp_addon.py` into your Blender addons directory (`%APPDATA%\Blender Foundation\Blender\4.x\scripts\addons\`).
2. Go to **Edit > Preferences > Add-ons** and enable **"AI Studio: Blender MCP Bridge"**.
3. Toggle the connector in the 3D Viewport N-Panel under the **Game Studio MCP** tab.
