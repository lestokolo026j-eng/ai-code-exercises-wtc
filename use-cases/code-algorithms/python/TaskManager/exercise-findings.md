# Task Manager Codebase Exploration — Submission Document

**Student:** Lesego Stokolo
**Date:** 14 June 2026
**Exercise:** Knowing Where to Start — Using AI to Comprehend Existing Codebases


## 1. Initial vs Final Understanding of the Task Manager Codebase

**My initial understanding (before AI):**
- I thought it was a simple to-do list app
- I thought numpy and pillow were part of the project
- I did not know which file ran first or how the files connected to each other
- I could see each file had a different name but did not understand why

**My final understanding (after AI):**
- It is a fully structured task manager application built in pure Python with no external frameworks
- numpy and pillow in requirements.txt are not actually used by this project
- The app starts at cli.py — this is the entry point
- Each file has one specific job (this is called separation of concerns)
- The files connect in this order: cli.py → task_manager.py → models.py + storage.py
  

## 2. Most Valuable Insights From Each Prompt

**Prompt 1 — Understanding Project Structure**

The most valuable insight was learning that cli.py is the entry point where everything begins. I also learned that a well-structured project gives each file exactly one responsibility. The AI corrected my misconception about numpy and pillow and helped me see how all the files connect like a chain.

How the files connect:
- cli.py — takes the user's input (like a waiter taking an order)
- task_manager.py — coordinates everything (like a head chef)
- models.py — defines what a task looks like (like a recipe book)
- storage.py — saves and loads tasks (like a fridge)
- task_priority.py — ranks tasks by importance
- task_parser.py — converts plain text into structured tasks
- task_list_merge.py — combines task lists and resolves conflicts

Data flow: User input → cli.py → task_manager.py → models.py + storage.py → displayed back to user


**Prompt 2 — Finding a Specific Feature (Create a Task)**

I investigated the "creating a new task" feature. My initial guess about which files were involved turned out to be exactly right. The feature flows through four files in this order: cli.py → task_manager.py → models.py → storage.py.

I also learned more effective search terms for navigating code:
- `create_task`, `add_task`, `Task(`, `tasks.append`, `storage.save`

Questions to ask when exploring any feature:
- Where is the command defined?
- What function actually builds the object?
- Where does it get saved?


**Prompt 2 — Finding a Specific Feature (CSV Export)**

I investigated where a new "Task Export to CSV" feature would live in the codebase.

Files I searched: storage.py, cli.py, task_manager.py
Search terms I used: "export", "file", "save", "write", "csv"

My hypothesis was that the feature would belong in storage.py since it already handles saving data, and cli.py would need a new export command. The AI confirmed this was correct.

Files that need to be modified:
- models.py — add a `to_dict()` method to convert tasks to plain data
- storage.py — add the actual CSV writing function
- cli.py — add a new export command for the user

Implementation order:
1. Add `to_dict()` to models.py
2. Add `export_tasks_to_csv()` to storage.py
3. Add export command to cli.py
4. Test that the CSV file is created correctly


**Prompt 3 — Understanding the Domain Model**

This prompt helped me understand the business logic behind the code — not just what the code does but why it is designed that way.

Core concepts I learned:
- Task — the main work item a user acts on
- TaskPriority — urgency level: LOW, MEDIUM, HIGH, URGENT
- TaskStatus — lifecycle stage: TODO → IN_PROGRESS → REVIEW → DONE
- created_at — never changes, records when the task was first created
- updated_at — changes every time any detail of the task is edited
- completed_at — only set when the task is marked as DONE
- Tags — labels used to group and filter tasks

My answers to the AI's domain model questions:

1. A task is overdue if it has a due date, that date has passed, and it has not been marked DONE yet.
2. If you edit a task title, only updated_at changes. created_at stays the same and completed_at stays empty.
3. The correct order of status changes is: TODO → IN_PROGRESS → REVIEW → DONE.
4. completed_at is separate from updated_at because updated_at changes for any edit, while completed_at records only the exact moment the task was finished.
5. URGENT tasks should appear at the top of the list, followed by HIGH, MEDIUM, and LOW.


## 3. Practical Application — Implementing a New Business Rule

**Scenario:** Tasks that are overdue for more than 7 days should be automatically marked as abandoned unless they are marked as high priority.

**Files I would modify:**
- models.py — add ABANDONED as a new TaskStatus option
- task_manager.py — add the business rule logic that checks overdue tasks and marks them abandoned
- cli.py — possibly add a command that triggers the check

**Changes I would make:**
1. Add ABANDONED to TaskStatus in models.py
2. Write a new function in task_manager.py that loops through all tasks, checks if they are overdue by more than 7 days, and marks them abandoned if their priority is LOW or MEDIUM
3. Reuse the existing `is_overdue()` method in models.py as a starting point

**Questions I would ask my team before implementing:**
- Should MEDIUM priority tasks also be protected, or only HIGH and URGENT?
- Should the user be notified when a task gets marked abandoned?
- Should this check run automatically or only when the user triggers it?


## 4. Reflection

**Which prompt was most helpful?**
Prompt 1 was the most helpful because understanding the overall structure first made everything else easier to follow. Once I knew what each file was responsible for and how they connected, the other prompts made much more sense.

**How did the AI prompts help with implementation?**
The AI prompts helped me understand that each file has one specific responsibility, so I knew exactly where to look. Because of Prompt 1 I knew the overall structure, because of Prompt 2 I knew how to trace a feature through the files, and because of Prompt 3 I understood the business logic well enough to know that `is_overdue()` already exists and could be reused.

**What aspects of the codebase am I still unsure about?**
I am still unsure about how task_manager.py coordinates everything internally. I understand what it does but I have not read through all its functions in detail. I am also unsure about how to trigger an automatic check without the user having to type a command.

**What would my next steps be?**
My next steps would be to open task_manager.py and read through it function by function. I would also run the app in VS Code to see how it behaves from a user's perspective, and ask a senior developer to walk me through how the files communicate with each other in real time.

**What would I do differently next time?**
I would read the README first and try to form stronger initial guesses before sending the prompt. The better my starting guess, the more useful the AI's corrections and additions were.

**What additional tools would complement AI prompting?**
Actually running the app to see what it does from a user's perspective, reading any diagrams the team has made, and asking a team member for a quick walkthrough would all complement AI prompting well.

