# Task Magic: Automagically create tasks for vibe coding projects

This system helps you manage your software projects by defining plans, breaking them down into actionable tasks, and keeping a memory of what's been done, all while working seamlessly with AI agents like Cursor and Windsurf.

One of the key goals is to avoid the "AI loop of death." ☠️

By breaking down work into smaller, focused tasks (each with a clear start, end, and test strategy), AI agents can tackle them effectively without losing context and making dumb mistakes.

(Note: they will still sometimes make dumb mistakes but it is 10x less likely using a task-based approach.)

The system provides permanent context through the `memory/` folder (located at the root of your main project). This allows AI coding agents to see what has been built before and understand the how and why, helping them to ship new tasks more effectively.

## Video walkthrough

To get an idea of how to use Task Magic in your own projects, check out the video walkthrough below (and also subscribe to the channel, obvs).

[![I built a task manager that makes Cursor "JUST WORK"](https://img.youtube.com/vi/XBu8KeWuQbM/0.jpg)](https://www.youtube.com/watch?v=XBu8KeWuQbM)

## How it works

Task Magic is a file-based system. This means all your project plans, tasks, and history are stored in plain text files (mostly Markdown) right in your project's root directory (e.g., `plans/`, `tasks/`, `memory/`). This makes it easy to version control, track changes, and for AI agents to read and understand your project.

There are three main parts to Task Magic:

1.  **Plans (`plans/`)**:
    *   **Purpose**: This is where you define the "what" and "why" of your project or specific features. Think of these as your Product Requirements Documents (PRDs). These files are expected at the root of your main project (e.g., `main-repo/plans/`).
    *   **Key files**:
        *   `plans/PLAN.md`: A global overview of your entire project. It should be a concise summary and index, linking to more detailed feature plans.
        *   `plans/{your-feature}-plan.md`: Detailed PRDs for each specific feature you're building. This is where the AI will look for specifics when generating tasks.
    *   **AI interaction**: AI agents use these plans to understand the scope and requirements, helping to generate tasks.

2.  **Tasks (`tasks/` & `TASKS.md`)**:
    *   **Purpose**: This is where the actual work items live. AI agents (or you) can break down plans into individual, manageable tasks. The `tasks/` directory and `TASKS.md` file are expected at the root of your main project (e.g., `main-repo/tasks/` and `main-repo/TASKS.md`).
    *   **Key files & structure**:
        *   `tasks/task{id}_description.md`: Each task gets its own Markdown file. It includes details like status (pending, inprogress, completed), priority, dependencies, a description, and how to test it.
        *   `TASKS.md`: This is your master checklist. It's a human-friendly overview of all tasks in the `tasks/` directory, showing their status at a glance. **This file and the individual task files are kept in sync by the AI.**
    *   **AI interaction**: AI agents can create tasks from plans, update their status as they work on them, and help you manage dependencies.

3.  **Memory (`memory/`)**:
    *   **Purpose**: Completed and failed tasks, as well as old plans, are archived here. This provides a valuable history for the AI to learn from and for you to reference. This directory is expected at the root of your main project (e.g., `main-repo/memory/`).
    *   **Key files**:
        *   `memory/tasks/`: Archived task files.
        *   `memory/TASKS_LOG.md`: A log of when tasks were archived.
        *   `memory/plans/`: Archived plan files.
        *   `memory/PLANS_LOG.md`: A log for archived plans.
    *   **AI interaction**: The AI can consult the memory to understand how similar things were done in the past, or why a certain approach was taken.

## Working with AI agents (Cursor & Windsurf)

Task Magic (these rules and associated scripts, typically located in `your-main-repo/.cursor/tasks/`) is designed to work closely with AI agents. Here's how rules and context are handled:

*   **Automatic context (`_index.md` files)**:
    *   Files named `_index.md` (like the one in `tasks/_index.mdc`) provide a high-level overview of a system or a set of rules.
    *   These `_index.md` files are **automatically included in the AI's context** when you're working within a project that uses them. This gives the AI a foundational understanding without you needing to do anything extra.

*   **On-demand rules (other `.md` or `.mdc` rule files)**:
    *   Other rule files (e.g., `tasks.mdc`, `plans.mdc`) are located within this `task-magic` system directory (e.g., `your-main-repo/.cursor/rules/tasks/`).
    *   Each of these rule files has a `description` in its header. The AI agent (Cursor/Windsurf) can read these descriptions and **decide dynamically whether a specific rule is relevant** to your current request or the task it's performing.
    *   If the AI deems a rule relevant, it will "fetch" and use that rule.

*   **Your role: Guiding the AI with @-tags**:
    *   While the agent is usually pretty good at figuring out which rules to use, you can manually tag the rules you want to use.
    *   **For best results, @-tag specific rule files or directories in your prompts.** For example:
        *   `@rules/tasks.mdc create tasks for this feature` (refers to a rule file within `your-main-repo/.cursor/rules/tasks/tasks.mdc`)
        *   `@tasks/plans.mdc generate a plan for X` (refers to `your-main-repo/.cursor/rules/tasks/plans.mdc`)
        *   `@TASKS.md what is the status of my project?` (refers to `your-main-repo/TASKS.md`)
        *   `@plans/my-cool-feature-plan.md can you review this plan?` (refers to `your-main-repo/plans/my-cool-feature-plan.md`)
    *   This helps ensure the AI looks at the exact information you want it to.

## Getting started

1.  **Initialize project structure**: The Task Magic system expects specific directories (`plans/`, `tasks/`, `memory/`) and files (`TASKS.md`, `plans/PLAN.md`, `memory/TASKS_LOG.md`) to be present at the root of your main project repository.
    *   If these directories and files don't exist, instruct the AI to create them. For example: "Set up the Task Magic directory structure in my project root."
    *   The AI should create:
        *   `plans/` directory, with a `plans/PLAN.md` file (start with a simple project title and overview).
        *   `tasks/` directory.
        *   `TASKS.md` file in the project root (can start with just `# Project Tasks`).
        *   `memory/` directory, with `memory/TASKS_LOG.md` and `memory/PLANS_LOG.md` files (can start with `# Task Archive Log` and `# Plan Archive Log` respectively).
        *   `memory/tasks/` and `memory/plans/` subdirectories.
2.  **Create a plan**: Ask your AI assistant to create a new feature plan using the planning rule (e.g., `@tasks/plans.mdc create a plan for user authentication`). The plan will be saved in `your-main-repo/plans/`.
3.  **Generate tasks**: Once a plan is ready, ask the AI to generate tasks from it (e.g., `@rules/tasks.mdc generate tasks for the user-authentication-plan.md`). Tasks will be created in `your-main-repo/tasks/` and listed in `your-main-repo/TASKS.md`.
4.  **Work on tasks**: Tell the AI to start working on tasks. It will update `TASKS.md` and the individual task files in the `tasks/` directory as it progresses.
5.  **Archive**: Periodically, ask the AI to archive completed or failed tasks to keep your active task list clean. These will be moved to `memory/tasks/` and `memory/plans/`.

By using Task Magic, you get a structured, AI-friendly way to manage your projects, ensuring both you and your AI assistants are always on the same page.

## Customizing Task Magic

I built this for myself to fit into my workflow. It might not work as well for you.

But since this is file based rules and we're all vibe coding in Cursor/Windsurf, you can easily customize it to fit your workflow.

If something doesn't work for you, tag the rule(s) you want to change, explain what doesn't work (and what you think might) and **have the agent update them for you.**

For example:

```
@tasks/plans.mdc I want my plans to be more concise and simple. Update the rule and make the PRD template shorter and to the point.
```

## Contributing & feedback

This system is designed to evolve. If you have ideas for improvements, find a bug, or want to make Task Magic even better:

*   **Open an issue**: If you encounter problems or have suggestions, please open an issue.
*   **Submit Pull Requests**: If you've made enhancements to the rules or the system structure that you think would benefit others, feel free to submit a pull request.