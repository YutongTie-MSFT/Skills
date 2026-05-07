# Copilot Skills

A collection of reusable Copilot CLI skills for engineering workflows in Dynamics 365 / Service Intelligence projects.

## Skills

| Skill | Description |
|---|---|
| `CCA-co-dev-spec-and-build` | End-to-end CCA feature development workflow: PM ask → codebase review → tech plan → tech spec → code change |

## How to Use

### Option 1 — Add to your repo (Recommended)
Copy the skill file into your repo's `.github/instructions/` folder:
```
your-repo/
└── .github/
    └── instructions/
        └── CCA-co-dev-spec-and-build.instructions.md
```
Copilot CLI will automatically load it every session for that repo. No extra steps needed.

### Option 2 — Reference at the start of a session
At the start of a Copilot CLI session, paste the skill content or tell Copilot:
> "Follow the CCA-co-dev-spec-and-build workflow for this task."

Then describe your PM ask and Copilot will follow the 12-step workflow automatically.

### Tips
- The skill works best when you have access to all three repos: `c4232cae-cea8-4841-9952-5f50e9d80d56`, `CRM.CS.ModernAdmin`, and `CRM.Solutions.ServiceIntelligence`
- Always start with a clear PM ask — the skill will prompt you to clarify ambiguities before diving into code
- Session history is automatically searched at Step 2 — prior investigations are reused, not rediscovered
