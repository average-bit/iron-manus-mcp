# Manus FSM Flow Diagram

## 🔄 Complete Agent Loop Flow

```
USER GOAL: "Build a React app with authentication"
    │
    ▼
┌─────────────────────────────────────────────────────────────────┐
│                     CLAUDE CODE CLI                            │
│  Sequential Thinking Mode: Must call tool after each turn      │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                  manus_orchestrator                             │
│  Input: { session_id: "123", initial_objective: "..." }        │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                   FSM CORE                              │    │
│  │                                                         │    │
│  │  INIT ──→ QUERY ──→ ENHANCE ──→ KNOWLEDGE ──→ PLAN     │    │
│  │    │        │         │           │           │        │    │
│  │    │        │         │           │           ▼        │    │
│  │    │        │         │           │        EXECUTE     │    │
│  │    │        │         │           │           │        │    │
│  │    │        │         │           │           ▼        │    │
│  │    └────────┴─────────┴───────────┴────── VERIFY       │    │
│  │                                           │            │    │
│  │                                           ▼            │    │
│  │                                         DONE           │    │
│  └─────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                    FSM OUTPUT                                   │
│  {                                                              │
│    next_phase: "QUERY",                                         │
│    system_prompt: "You are interpreting the user's goal...",    │
│    allowed_next_tools: ["manus_orchestrator"],                  │
│    status: "IN_PROGRESS"                                        │
│  }                                                              │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                     CLAUDE BRAIN                               │
│  System Prompt: "You are interpreting the user's goal..."      │
│  Allowed Tools: ["manus_orchestrator"]                         │
│                                                                 │
│  Claude thinks: "User wants React app with auth. Need to       │
│  clarify authentication method, styling preferences..."        │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                  manus_orchestrator                             │
│  Input: {                                                       │
│    session_id: "123",                                           │
│    phase_completed: "QUERY",                                    │
│    payload: { interpreted_goal: "React app with JWT auth..." }  │
│  }                                                              │
│                                                                 │
│  FSM: QUERY → ENHANCE                                           │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
                      [Continue through all phases...]
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                   FINAL OUTPUT                                 │
│  Phase: DONE                                                    │
│  Status: Task completed successfully!                           │
│  Deliverable: React app with authentication system             │
└─────────────────────────────────────────────────────────────────┘
```

## 🎭 Phase-by-Phase Breakdown

```
Phase 1: QUERY
┌─────────────────────────┐    ┌──────────────────────────┐
│ Input: Raw user goal    │───▶│ Output: Interpreted goal │
│ Tools: [orchestrator]   │    │ Next: ENHANCE           │
└─────────────────────────┘    └──────────────────────────┘

Phase 2: ENHANCE  
┌─────────────────────────┐    ┌──────────────────────────┐
│ Input: Interpreted goal │───▶│ Output: Enhanced goal    │
│ Tools: [orchestrator]   │    │ Next: KNOWLEDGE         │
└─────────────────────────┘    └──────────────────────────┘

Phase 3: KNOWLEDGE
┌─────────────────────────┐    ┌──────────────────────────┐
│ Input: Enhanced goal    │───▶│ Output: Knowledge gather │
│ Tools: [orchestrator]   │    │ Next: PLAN              │
└─────────────────────────┘    └──────────────────────────┘

Phase 4: PLAN
┌─────────────────────────┐    ┌──────────────────────────┐
│ Input: Knowledge        │───▶│ Output: TodoWrite plan   │
│ Tools: [TodoWrite, orch]│    │ Next: EXECUTE           │
└─────────────────────────┘    └──────────────────────────┘

Phase 5: EXECUTE
┌─────────────────────────┐    ┌──────────────────────────┐
│ Input: Plan created     │───▶│ Output: Implementation   │
│ Tools: [ALL TOOLS]      │    │ Next: VERIFY            │
└─────────────────────────┘    └──────────────────────────┘

Phase 6: VERIFY
┌─────────────────────────┐    ┌──────────────────────────┐
│ Input: Implementation   │───▶│ Output: Quality check    │
│ Tools: [Read, orch]     │    │ Next: DONE              │
└─────────────────────────┘    └──────────────────────────┘

Phase 7: DONE
┌─────────────────────────┐    ┌──────────────────────────┐
│ Input: Verified work    │───▶│ Output: Success summary  │
│ Tools: []               │    │ Next: (end)             │
└─────────────────────────┘    └──────────────────────────┘
```

## 🔐 Tool Gating Security

```
                    PHASE-BASED TOOL ACCESS CONTROL
                              
╔═══════════════════════════════════════════════════════════════╗
║                           SECURITY WALL                      ║
╠═══════════════════════════════════════════════════════════════╣
║ QUERY:     [manus_orchestrator]                    ✅ SAFE   ║
║ ENHANCE:   [manus_orchestrator]                    ✅ SAFE   ║  
║ KNOWLEDGE: [manus_orchestrator]                    ✅ SAFE   ║
║ PLAN:      [TodoWrite, manus_orchestrator]         ✅ SAFE   ║
║ EXECUTE:   [Bash, Edit, Task, *, manus_orchestrator] ⚠️ POWER ║
║ VERIFY:    [Read, manus_orchestrator]              ✅ SAFE   ║
║ DONE:      []                                      ✅ SAFE   ║
╚═══════════════════════════════════════════════════════════════╝

Key Insight: Claude can only use powerful tools (Bash, Edit, Task) 
during EXECUTE phase, ensuring controlled and auditable execution.
```

## 🧠 Hijacking Sequential Thinking

```
NORMAL Sequential Thinking:
┌─────────┐    ┌─────────┐    ┌─────────┐
│ Think 1 │───▶│ Think 2 │───▶│ Think 3 │ ... (free-form)
└─────────┘    └─────────┘    └─────────┘

MANUS FSM Sequential Thinking:
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│ QUERY   │───▶│ ENHANCE │───▶│ PLAN    │───▶│ EXECUTE │ (deterministic)
│ prompt  │    │ prompt  │    │ prompt  │    │ prompt  │
└─────────┘    └─────────┘    └─────────┘    └─────────┘
     │              │              │              │
     ▼              ▼              ▼              ▼
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│ manus_  │    │ manus_  │    │ manus_  │    │ manus_  │
│ orch    │    │ orch    │    │ orch    │    │ orch    │
└─────────┘    └─────────┘    └─────────┘    └─────────┘

Result: Claude MUST follow the FSM - no drift, no confusion!
```