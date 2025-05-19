# Task Magic Workflow Diagram

Below is the workflow diagram for the Task Magic system, illustrating the process of task creation, execution, and archival.

```mermaid
graph TD
    A[User Request: Create Tasks from Plan/PRD] --> B["Plan All Tasks (Identify complex tasks for expansion)"];
    B --> B1{Expansion Needed for Any Task?};
    B1 -- Yes --> B2["Define Sub-tasks & Update Parent Task Definitions to Parent Tasks (listing sub-tasks in parent\'s details)"];
    B1 -- No --> C;
    B2 --> C["Update TASKS.md (in project root) with ALL Planned Tasks (Parent Tasks & Sub-tasks)"];
    C --> D["For Each Task (Parent Task or Sub-task) in TASKS.md (project root)"];
    D -- Loop --> E[Create/Update Individual task file in tasks/ (project root)];
    E --> F["Populate YAML & Markdown Body in task file (Parent task file updated to reflect parent status & list sub-tasks)"];
    F -- End Loop --> G[All Task Files Created/Updated];
    G --> H{User asks agent to work?};
    H -- Yes --> I[Agent reads TASKS.md (project root), finds first pending task];
    I --> J{Check Dependencies for selected task};
    J -- Met --> K[Update Task File YAML status: inprogress];
    K --> L[Update TASKS.md (project root) entry: progress marker];
    L --> M[Execute Task];
    M -- Success --> N[Update YAML status: completed];
    N --> O[Update TASKS.md (project root) entry: completed marker];
    M -- Failure --> P["Update YAML status: failed, add error_log"];
    P --> Q[Update TASKS.md (project root) entry: failed marker];
    J -- Not Met --> R[Inform User: Dependencies Missing];
    S{User asks to archive?} --> T[Agent finds completed/failed tasks in tasks/ (project root)];
    T --> U[Move task files to memory/tasks/ (project root)];
    U --> V[Append summary to memory/TASKS_LOG.md (project root)];
    V --> W[Remove corresponding entries from TASKS.md (project root)];
```
