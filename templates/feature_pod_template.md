# Feature Pod & Dedicated Chat Launcher Template

When the Conductor decomposes a game into features (either at project start or on-demand), it creates a dedicated **Feature Pod** inside `features/[XX-feature-name]/`:

```text
features/
└── [01-core-mechanics]/
    ├── SPEC.md             # Detailed feature specification (from feature_spec_template.md)
    ├── TASKS.md            # Actionable engineering & art checklist
    ├── LAUNCH_CHAT.md      # 1-Click prompt to open a dedicated chat session with assigned agent
    └── INTEGRATION.md      # Engine hook points and cross-feature contracts
```

---

## Content of `LAUNCH_CHAT.md` (Example)

```markdown
# 🚀 Dedicated Chat Session: [Feature Name]

> **Instructions for User:**  
> Open a **new chat window** in Antigravity, Cursor, or Claude Code, copy-paste the block below, and send it. This isolates the context and assigns the specialist agent directly to this feature.

---

### Copy & Paste into New Chat:

I am working on feature **[Feature Name]** for project **[Project Name]**.

Please act as **@[Assigned Specialist Agent, e.g., agents/specialists/combat-designer.md]** assisted by **@[Second Agent, e.g., agents/game_feel_developer.md]**.

Context and tasks for this session:
1. Review feature specification: `features/[XX-feature-name]/SPEC.md`
2. Follow task checklist: `features/[XX-feature-name]/TASKS.md`
3. Engine integration rules: `features/[XX-feature-name]/INTEGRATION.md`

Let's begin with Task 1 from `TASKS.md`. Please propose your implementation plan.
```
