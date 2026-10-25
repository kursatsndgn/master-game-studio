---
name: narrative-quest-engine
description: Comprehensive narrative design, world-building, quest graph architecture, branching dialogue, and multi-engine export engine (Yarn Spinner, Ink, Godot Dialogue Manager, and Unreal Gameplay Tag DataTables).
---

# Narrative & Quest Engine Skill

The **Narrative & Quest Engine** is specialized in designing rich branching narratives, quest dependency graphs, non-linear dialogues, and character personality states, automatically serializing them into engine-ready formats.

---

## 1. Supported Export Formats

| Engine | Tooling / Plugin | Output File Extension | Export Format Description |
| :--- | :--- | :--- | :--- |
| **Unity** | Yarn Spinner | `.yarn` | Node-based interactive dialogue script with variables and commands. |
| **Godot 4** | Dialogue Manager (Nathan Hoad) | `.dialogue` | Indented text format supporting Godot signals, state checks, and mutations. |
| **Universal** | Ink by Inkle | `.ink` | Industry standard branching narrative scripting language. |
| **Unreal Engine 5** | Gameplay Tags & DataTables | `.json` / `.csv` | Schema-validated tables for UE DataAsset / DataTable serialization. |

---

## 2. Format Generation Templates

### A. Godot Dialogue Manager (`.dialogue`)
```dialogue
~ start
Elder Matthew: The fog has crept past the cemetery gates.
if has_lantern == false:
    Elder Matthew: You cannot step into the pines without a flame.
    - Where can I find a lantern? => ask_lantern
    - I do not fear the fog. => stubborn_path
else:
    Elder Matthew: I see the flame in your hands. May the light keep you.
    do trigger_quest_step("enter_forest")
    => END

~ ask_lantern
Elder Matthew: Check the shed behind the blacksmith's forge.
do set_quest_marker("blacksmith_shed")
=> END

~ stubborn_path
Elder Matthew: Then may the ancestors pity your soul.
do apply_curse("mist_blindness")
=> END
```

### B. Yarn Spinner Format (`.yarn`)
```yarn
title: StartNode
tags: quest, forest
---
Elder Matthew: The fog has crept past the cemetery gates.
<<if $has_lantern is false>>
    Elder Matthew: You cannot step into the pines without a flame.
    -> Where can I find a lantern?
        <<jump AskLantern>>
    -> I do not fear the fog.
        <<jump StubbornPath>>
<<else>>
    Elder Matthew: I see the flame in your hands.
    <<set $forest_unlocked = true>>
<<endif>>
===
title: AskLantern
---
Elder Matthew: Check the shed behind the blacksmith's forge.
<<set_objective "FindLantern">>
===
```

### C. Unreal Engine 5 Quest DataTable Schema (`.json`)
```json
[
  {
    "QuestID": "Quest.DarkForest.Pines",
    "Stage": 1,
    "ObjectiveText": "Acquire a lantern from the blacksmith's shed",
    "RequiredTags": ["Gameplay.Item.Lantern"],
    "CompletedTags": ["Quest.DarkForest.Stage1.Complete"],
    "OnCompletionEvents": ["Event.SpawnMistWanderer"],
    "ExperienceReward": 250
  }
]
```

---

## 3. Quest Graph Integrity Verification

Before emitting quest trees, verify the following graph invariant rules:

1. **Deadlock & Orphan Prevention**: Every node must have at least one valid path from `root` to an terminal/end state.
2. **Variable Determinism**: Any variable read in a branch condition (e.g. `has_key`, `trust_level`) must have an initial declaration and state mutation path.
3. **Circular Loop Detection**: Endless conversational loops must have explicit exit conditions or exhaustion counts.
4. **Localization Keys**: String literals should automatically include deterministic localization tags (e.g., `#line:elder_matthew_01`).

---

## 4. Multi-Agent Integration Directives

- When cooperating with `@blender-3d-pipeline`: Extract any prop or environmental item mentioned in narrative text (e.g., `"rusted_key"`, `"iron_lantern"`) and output a list of required 3D asset briefs.
- When cooperating with `@engine-mcp-connectors`: Emit the finished script directly into the project's dialogue directory and instruct the engine connector to re-index the asset database.
