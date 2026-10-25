# Multi-Agent Coordination Protocol & System Architecture (`AGENTS.md`)

This document defines the operating model, hierarchy, interaction contracts, and execution pipelines for AI agents operating within the **Master Game Studio Hub**.

---

## 1. Agent Roles & Hierarchy

```mermaid
graph TD
    User([👤 User / Producer]) --> Director["🎯 gamestudio-director (Lead Orchestrator)"]
    
    Director --> Narrative["📜 narrative-quest-engine"]
    Director --> BlenderPipe["📐 blender-3d-pipeline"]
    Director --> EngineRouter{"🔀 Engine Connector Router"}
    
    EngineRouter --> MCP_Blender["🔌 blender-mcp"]
    EngineRouter --> MCP_Unity["🔌 unity-mcp"]
    EngineRouter --> MCP_Godot["🔌 godot-mcp"]
    EngineRouter --> MCP_Unreal["🔌 unreal-mcp"]
    
    Narrative -.->|Dialogue / Quest Payloads| EngineRouter
    BlenderPipe -.->|Export Artifacts (.gltf, .fbx)| EngineRouter
```

### Role Matrix

| Agent | Responsibility | Primary Inputs | Primary Outputs |
| :--- | :--- | :--- | :--- |
| **`gamestudio-director`** | High-level production lead, milestone planning, delegation, quality gates. | High-level game pitch, feature briefs, user constraints. | Production plans, assigned sub-agent tasks, integration reviews. |
| **`narrative-quest-engine`** | Narrative branching, character bibles, quest graphs, dialogue scripting. | Quest briefs, character profiles, lore bible. | Ink scripts, Yarn files, Godot dialogue resources, Unreal DataTables. |
| **`blender-3d-pipeline`** | Procedural geometry, rigging, UV unwrapping, material shaders, LOD exports. | Asset specifications, polycount budgets, style guides. | Headless Blender scripts, `.blend` files, `.gltf` / `.fbx` bundles. |
| **`engine-mcp-connectors`** | Bi-directional engine control, live scene manipulation, playtest telemetry. | Formatted engine tasks, asset paths, script snippets. | Scene updates, test reports, console logs, error diagnostics. |

---

## 2. Cross-Agent Data Exchange Contracts

All agents communicate using structured JSON schema payloads to ensure reliable multi-agent execution.

### Quest Export Payload Contract
```json
{
  "$schema": "https://master-game-studio.local/schemas/quest-v1.json",
  "quest_id": "quest_dark_forest_01",
  "title": "Whispers in the Pines",
  "target_engine": "godot",
  "state_variables": {
    "has_lantern": false,
    "herbalist_trust": 0
  },
  "nodes": [
    {
      "node_id": "start",
      "type": "dialogue",
      "speaker": "Elder Matthew",
      "text": "The fog has thickened. Do not enter without a flame.",
      "choices": [
        { "label": "I have no fear of the dark.", "next_node": "refuse_lantern" },
        { "label": "Where can I find a lantern?", "next_node": "ask_lantern" }
      ]
    }
  ],
  "rewards": {
    "exp": 150,
    "items": ["rusted_key"]
  }
}
```

### 3D Asset Manifest Contract
```json
{
  "$schema": "https://master-game-studio.local/schemas/asset-3d-v1.json",
  "asset_id": "prop_iron_lantern",
  "category": "prop",
  "lod_levels": [
    { "level": 0, "target_triangles": 1200, "file": "prop_iron_lantern_lod0.gltf" },
    { "level": 1, "target_triangles": 450, "file": "prop_iron_lantern_lod1.gltf" },
    { "level": 2, "target_triangles": 120, "file": "prop_iron_lantern_lod2.gltf" }
  ],
  "collision_type": "convex_hull",
  "pbr_textures": {
    "albedo": "textures/lantern_albedo.png",
    "roughness_metallic": "textures/lantern_arm.png",
    "normal": "textures/lantern_normal.png"
  }
}
```

---

## 3. Standard Operational Workflows

### Workflow A: End-to-End Asset Creation & Engine Placement
1. **User asks**: *"Create a medieval chest prop and place it in my Godot level."*
2. **`gamestudio-director`**:
   - Decomposes the task into asset generation and engine import steps.
   - Invokes `blender-3d-pipeline` with dimensions, polycount limits (under 2,000 tris), and PBR material setup.
3. **`blender-3d-pipeline`**:
   - Generates procedural Blender Python script using `bpy`.
   - Executes Blender in headless background mode: `blender -b -P generate_chest.py`.
   - Exports `medieval_chest.gltf` and collision hull into target project assets.
4. **`godot-mcp`**:
   - Receives asset import notification via MCP.
   - Triggers `EditorInterface.get_resource_filesystem().reimport_files()`.
   - Instantiates the chest scene at designated coordinates `(Vector3(0, 0, 0))` in the active 3D scene.
5. **Director**:
   - Verifies placement and returns confirmation summary with visual diff / screenshot to user.

---

## 4. Safety & Production Guardrails

1. **Non-Destructive Modifications**:
   - Agents MUST NOT run recursive file deletions (`rmdir /s`, `rm -rf`) on asset or scene folders.
   - Scene mutations must be reversible or wrapped in engine Undo blocks (`UndoRedo` in Godot, `Undo.RegisterCreatedObjectUndo` in Unity).
2. **Deterministic File Paths**:
   - All exported assets must reside within well-defined project directories:
     - Unity: `Assets/MasterGameStudio/<Category>/`
     - Godot: `res://master_game_studio/<category>/`
     - Unreal: `/Game/MasterGameStudio/<Category>/`
3. **Headless Timeout Limits**:
   - Blender headless scripts and engine automation calls must enforce a maximum timeout of 60 seconds to prevent hanging processes.
