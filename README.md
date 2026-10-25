# Master Game Studio Hub 🎮🚀

[![Architecture](https://img.shields.io/badge/Architecture-Agentic%20Multi--Engine-blueviolet?style=for-the-badge)](#architecture)
[![MCP Powered](https://img.shields.io/badge/MCP-Model%20Context%20Protocol-emerald?style=for-the-badge)](#mcp-connectors)
[![Engines](https://img.shields.io/badge/Engines-Unity%20%7C%20Godot%20%7C%20Unreal%20%7C%20Blender-orange?style=for-the-badge)](#supported-engines)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

**Master Game Studio Hub** is a next-generation, production-ready AI development hub designed for modern game production. It unifies **Narrative Design**, **Procedural 3D Asset Pipelines**, and **Direct Engine Tooling** across Blender, Unity, Godot, and Unreal Engine through standardized **Model Context Protocol (MCP)** bridges.

---

## 🌟 Key Capabilities

- **Executive Agent Orchestration (`gamestudio-director`)**: High-level game direction, milestone planning, cross-engine task delegation, and production balance.
- **Narrative & Quest Engine (`narrative-quest-engine`)**: Branching quest graph generation, dialogue writing, character state bibles, and multi-format exports (Ink, Yarn Spinner, Godot Dialogue Manager, Unreal Gameplay Tags).
- **Automated 3D Asset Pipeline (`blender-3d-pipeline`)**: Procedural mesh generation, automated UV unwrapping, PBR material synthesis, LOD cascades, and batch glTF/FBX/USD exports.
- **Bi-Directional Engine MCP Connectors (`engine-mcp-connectors`)**:
  - 🎨 **Blender MCP**: Live scene inspection, headless Python automation, geometry node control.
  - ⚡ **Unity MCP**: Scene graph traversal, GameObject manipulation, AssetDatabase automation, and Playmode test execution.
  - 🤖 **Godot 4 MCP**: GDScript execution, Scene tree queries, node instantiation, and resource serialization.
  - 🌌 **Unreal Engine 5 MCP**: Python Remote Execution bridge, Blueprint node querying, actor placement, and Nanite/Lumen settings verification.

---

## 🏗️ Architecture

```mermaid
flowchart TD
    Director["🎯 Game Studio Director\n(Executive Agent)"]
    
    subgraph Core_Creation ["Creative & Narrative Core"]
        Narrative["📜 Narrative & Quest Engine\n(Dialogue, Graphs, Character Bibles)"]
        BlenderPipe["📐 Blender 3D Pipeline\n(Procedural Meshes, UVs, LODs, PBR)"]
    end
    
    subgraph MCP_Layer ["Model Context Protocol (MCP) Connectors"]
        MCP_Blender["🔌 Blender MCP Bridge"]
        MCP_Unity["🔌 Unity MCP Bridge"]
        MCP_Godot["🔌 Godot 4 MCP Bridge"]
        MCP_Unreal["🔌 Unreal Engine 5 MCP Bridge"]
    end
    
    subgraph Engines ["Target Engine Runtime & Editors"]
        BlenderRuntime["Blender 4.x\n(DCC Environment)"]
        UnityRuntime["Unity 2022/6000\n(C# Editor API)"]
        GodotRuntime["Godot 4.x\n(GDScript / C#)"]
        UnrealRuntime["Unreal Engine 5.x\n(Python Remote / C++)"]
    end

    Director --> Narrative
    Director --> BlenderPipe
    
    Narrative -->|Story Data & Quests| MCP_Unity
    Narrative -->|Dialogue Tables| MCP_Godot
    Narrative -->|Gameplay Tags & DataTables| MCP_Unreal
    
    BlenderPipe -->|Asset Generation| MCP_Blender
    MCP_Blender --> BlenderRuntime
    
    MCP_Blender -.->|glTF / FBX Export| MCP_Unity
    MCP_Blender -.->|glTF Export| MCP_Godot
    MCP_Blender -.->|FBX / USD Export| MCP_Unreal

    MCP_Unity --> UnityRuntime
    MCP_Godot --> GodotRuntime
    MCP_Unreal --> UnrealRuntime
```

---

## 📁 Repository Structure

```text
master-game-studio/
├── README.md                                  # Hub overview and getting started guide
├── AGENTS.md                                  # Multi-agent coordination protocols and role definitions
├── .gitignore                                 # Production ignore rules across engines & DCC tools
└── agent-skills/                              # Standardized agent operational skill manifests
    ├── gamestudio-director/                   # Production planning, feature scope, and orchestration
    │   └── SKILL.md
    ├── narrative-quest-engine/                # World-building, branching dialogue, quest logic
    │   └── SKILL.md
    ├── blender-3d-pipeline/                   # Python procedural modeling, UV unwrap, LOD export
    │   └── SKILL.md
    └── engine-mcp-connectors/                 # Engine communication protocols (MCP bridges)
        ├── blender-mcp/                       # Blender RPC & Python automation server
        │   └── SKILL.md
        ├── unity-mcp/                         # Unity Editor C# bridge & Playmode runner
        │   └── SKILL.md
        ├── godot-mcp/                         # Godot 4 EditorPlugin & TCP/WebSocket bridge
        │   └── SKILL.md
        └── unreal-mcp/                        # Unreal Engine 5 Python Remote Execution bridge
            └── SKILL.md
```

---

## 🚀 Getting Started

### Prerequisites
1. **DCC & Engine Installations**:
   - Blender 4.0+
   - Unity 2022.3 LTS or Unity 6+
   - Godot Engine 4.2+
   - Unreal Engine 5.3+
2. **AI Runtime**:
   - Antigravity IDE / Antigravity Agent Runtime
   - Node.js 20+ and Python 3.10+ for MCP bridge servers

### Initial Setup
```bash
# Clone the repository
git clone https://github.com/kursatsndgn/master-game-studio.git
cd master-game-studio

# Verify workspace configuration
git status
```

### Activating Skills
Each folder inside `agent-skills/` conforms to the standardized `SKILL.md` format. When you interact with an Antigravity agent:
1. **To plan a game feature**: `"Run @gamestudio-director to scope an open-world tavern quest."`
2. **To generate quest trees**: `"Invoke @narrative-quest-engine and output in Godot Dialogue Manager format."`
3. **To generate 3D assets**: `"Use @blender-3d-pipeline to generate a low-poly iron sword with 3 LODs."`
4. **To inspect live scenes**: `"Use @engine-mcp-connectors/godot-mcp to inspect active nodes in the main scene."`

---

## 🛠️ MCP Connector Protocols

| Engine | Primary Transport | Host Script / Plugin | Key Tools |
| :--- | :--- | :--- | :--- |
| **Blender** | stdio / Local Socket | `bpy_mcp_addon.py` | `blender_exec_script`, `blender_inspect_scene`, `blender_export_asset` |
| **Unity** | HTTP / WebSocket | `UnityMcpEditorBridge.cs` | `unity_get_scene_hierarchy`, `unity_instantiate_prefab`, `unity_run_playmode_tests` |
| **Godot 4** | WebSocket / TCP | `godot_mcp_plugin.gd` | `godot_get_tree`, `godot_run_gdscript`, `godot_instantiate_packed_scene` |
| **Unreal Engine 5** | UDP / Web Remote Control | UE Python Remote Execution | `unreal_execute_python`, `unreal_spawn_actor`, `unreal_query_assets` |

---

## 📜 Contributing & Governance
See [AGENTS.md](AGENTS.md) for multi-agent interaction rules, message payloads, error-handling conventions, and schema contracts.
