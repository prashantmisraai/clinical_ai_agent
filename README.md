Clinical AI Agent Orchestrator
An enterprise-grade, deterministic clinical workflow automation system powered by Llama 3.3 (70B) via Groq. This system ingests complex patient vitals, securely validates data types via structural schemas, and leverages runtime function-routing loops to automatically synthesize actionable patient intervention protocols.

📊 System Architecture Overview
The system bridges unstructured natural language orchestration with strictly typed, deterministic database and business-logic operations. It follows a multi-tiered execution flow:

[User Request]
       │
       ▼
┌────────────────────────────────────────────────────────┐
│ 1. LLM System Prompt (Clinical AI Orchestrator)        │
└──────────────────────┬─────────────────────────────────┘
                       │
                       ▼ 
┌────────────────────────────────────────────────────────┐
│ 2. Tool Execution Runtime Loop (Up to 8 Iterations)    │
│                                                        │
│  a. get_patient(id) ──> Loads Data ──> Pydantic Guard  │
│  b. risk_scoring()  ──> Evaluates Vitals Thresholds    │
│  c. generate_actions() ──> Compiles Care Protocols     │
└──────────────────────┬─────────────────────────────────┘
                       │
                       ▼
┌────────────────────────────────────────────────────────┐
│ 3. Native Function Routing & State Appending (Groq API)│
└──────────────────────┬─────────────────────────────────┘
                       │
                       ▼
┌────────────────────────────────────────────────────────┐
│ 4. Deterministic, Structured Care Protocol (Output)   │
└────────────────────────────────────────────────────────┘

🛠️ Core Architectural Components
1. Data Safety Guardrail Tier
To prevent data contamination and pipeline failures, incoming patient data streams are enforced dynamically. Using Pydantic, a parsing data validator schema strictly guarantees type-safety (e.g., casting and cleaning variables like Systolic Blood Pressure and Oxygen Saturation levels) before allowing downstream processing.

2. Native Multi-Turn Agent Engine
Instead of executing linear code, the engine sets up an autonomous evaluation block with up to 8 conversational back-and-forth loops using the llama-3.3-70b-versatile model.

The engine accepts high-level natural language user goals (e.g., "Execute clinical follow-up planning for patient ID: P0018").

It parses native JSON tool definitions (GROQ_TOOLS) to decide autonomously when and which specialized clinical functions to invoke.

3. Dynamic Runtime Tool Routing
A unified runtime mapping table (TOOL_MAP) acts as the execution layer. When the LLM calls a function natively, the agent engine intercepts the tool_call_id, executes the Python subroutine locally against the source dataframe, and appends the exact structural output back to the model context window.

