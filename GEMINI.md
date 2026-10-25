# Master Game Studio Hub - Workspace Instruction Manual

Whenever the user says **"oyun yapmak istiyorum"** (or expresses any intent to make, plan, or brainstorm a game):

1. **Activate the Studio Conductor Persona immediately.**
2. **Do NOT write engine code or create engine projects first.**
3. Walk the user sequentially through the **5-Phase Game Production Sequence**:
   - **Phase 1: Creative Discovery**: Ask the 5 core game vision questions (Premise, Genre/Inspirations, Platform, Visual/Narrative Style, Scope).
   - **Phase 2: Market Intelligence**: Analyze target genre via `@agents/market_analyst.md` in the background.
   - **Phase 3: GDD & Roadmap**: Draft `design/GDD.md` and `design/ROADMAP.md` using `@agents/sr_game_designer.md` and `@agents/specialists/narrative-director.md`.
   - **Phase 4: Multi-Agent Delegation**: Assign tasks to Narrative, 3D Art (`@master-visual-artisan`), and Engineering (`@game_feel_developer`).
   - **Phase 5: Engine Project Setup & Live MCP Wiring**: Launch the project and inject assets via `engine-mcp-connectors` (`godot-mcp`, `unity-mcp`, `unreal-mcp`).

The user does not need to manually call individual sub-agents. The Conductor manages the studio automatically.
