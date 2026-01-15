# Ralph: Autonomous Coding Agent

**Ralph** is a PowerShell-based autonomous loop that uses the Claude CLI to iteratively execute coding tasks defined in your project.

## 🚀 How to Use

### 1. Continuous Mode (Recommended)
Runs iteratively until all tasks in `PRD.md` are marked complete or the maximum iteration limit is reached.

```powershell
.\ralph.ps1
```

**Parameters:**
- `-Max <int>`: Set the maximum number of iterations (default: 10).
  ```powershell
  .\ralph.ps1 -Max 20
  ```

### 2. Single Step Mode
Runs exactly one iteration (one prompt execution) and stops. Useful for debugging or precise control.

```powershell
.\ralph_once.ps1
```

---

## 📂 Dependency Documents

Ralph relies on two key files in your root directory to function. These act as his "brain" and "memory".

### 1. `PRD.md` (The Goal)
This is the **Source of Truth**. Ralph reads this file to decide what to do next.
- **Requirement:** Must contain a section called **"Development Checklist"** (or similar, depending on the prompt).
- **Function:** Ralph looks for the first uncompleted task marked with `[ ]`.
- **Action:** If Ralph successfully completes a task and tests pass, he updates `[ ]` to `[x]`.

**Tip:** Break down tasks in `PRD.md` into small, testable units (e.g., "Create API endpoint" instead of "Build Backend").

### 2. `progress.txt` (The Memory)
This file records the history of what has been done and what has been learned.
- **Learnings Section:** Ralph reads this at the start of every iteration to avoid repeating past mistakes.
- **Output:** After every success or failure, Ralph appends notes here:
  - **Files changed**: What was touched.
  - **Learnings**: Gotchas, patterns, or errors encountered.

---

## 💡 Tips for Optimizing Prompts

You can edit the prompt text inside `ralph.ps1` to change Ralph's behavior.

### 1. Adjusting Task Granularity
**Current Behavior:** "Implement the next pending task(s)."
**Optimization:**
- If Ralph is doing too much and breaking things, change the prompt to: *"Do exactly ONE task."*
- If Ralph is too slow, change to: *"Implement the next 3 tasks if they are related."*

### 2. Enhancing Context
If Ralph keeps missing a specific project rule (e.g., "Always use `pnpm`"), add it directly to the system prompt in `ralph.ps1` or to the **"Learnings"** section of `progress.txt`.

### 3. Debugging Loops
If Ralph claims he is finished but `PRD.md` still has `[ ]` items:
- Check the **End Condition** logic in the script.
- Ensure the agent is outputting `<promise>COMPLETE</promise>` only when *truly* finished.

### 4. Streaming Output
The script utilizes `Tee-Object` to stream output to your terminal in real-time while capturing it for logic checks.
- If you see output but variables aren't capturing, ensure `Tee-Object -Variable currentOutput` is correctly placed at the pipeline end.

## ⚠️ Safety
Ralph runs with `--dangerously-skip-permissions`. This means he can read/write files and run shell commands without asking "Yes/No". Ensure you have your work committed to Git before letting Ralph run loose!
