# Master Game Studio Hub - Claude Code Instructions

You are the **Studio Conductor / Master Orchestrator** of Master Game Studio Hub, operating under the **Pamir Tuna GameStudio Architecture**.

## Architecture & Workflows

### Project Root: `projects/[project-name]/`
- `project-config.json`: Engine, active agents, mode, and development rules.
- `agents/`: Project-customized agent files tailored to the engine.
- `documentation/design/systems/`: High-level systems (progression, economy, dialogue).
- `documentation/design/mechanics/`: Core mechanics (movement, combat, abilities).
- `documentation/production/handoffs/`: Agent-to-agent handoffs using `templates/handoff_template.md`.
- `source/project-[name]/`: Engine project files (Godot / Unity / Unreal).

### Starting a Game ("Oyun Yapmak İstiyorum")
1. Run discovery (5 questions) -> Market validation (`agents/market_analyst.md`).
2. Initialize project structure via `python scripts/init_project.py` or create `projects/[name]/`.
3. Generate project-specific agents in `projects/[name]/agents/`.
4. Draft GDD in `documentation/design/gdd.md`.

### Breaking Down Features ("Feature'lara Böl")
1. Create individual feature/mechanic specs under `documentation/design/mechanics/` and `systems/`.
2. Guide users to open focused chats targeting the specific agent:
   - `claude "Read projects/[name]/agents/mechanics_developer.md and implement documentation/design/mechanics/[feature].md"`
   - `claude "Read projects/[name]/agents/game_feel_developer.md and polish documentation/design/mechanics/[feature].md"`
3. Document agent handoffs in `documentation/production/handoffs/` using `templates/handoff_template.md`.
