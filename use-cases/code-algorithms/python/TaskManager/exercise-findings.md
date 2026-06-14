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

The most valuable insight was learning that cli.py is the entry point — the file where everything begins. I also learned that a well-structured project gives each file exactly one responsibility. The AI corrected my misconception about numpy and pillow and helped me see how all the files connect to each other like a chain.

How the files connect:
- cli.py — takes the user's input (like a waiter taking an order)
- task_manager.py — coordinates everything (like a head chef)
- models.py — defines what a task looks like (like a recipe book)
- storage.py — saves and loads tasks (like a fridge)
- task_priority.py — ranks tasks by importance
- task_parser.py — converts plain text into structured tasks
- task_list_merge.py — combines task lists and resolves conflicts

Data flow: User input → cli.py → task_manager.py → models.py + storage.py → displayed back to user



**Prompt 2 — Finding a Specific Feature**

I investigated the "creating a new task" feature. My initial guess about which files were involved turned out to be exactly right. The feature flows through four files in this order: cli.py → task_manager.py → models.py → storage.py.

I also learned more effective search terms for navigating code:
- `create_task`, `add_task`, `Task(`, `tasks.append`, `storage.save`

Questions to ask when exploring any feature:
- Where is the command defined?
- What function actually builds the object?
- Where does it get saved?



**Prompt 3 — Understanding the Domain Model**

This prompt helped me understand the business logic behind the code — not just what the code does but why it is designed that way.

Core concepts I learned:
- Task — the main work item a user acts on
- TaskPriority — urgency level with four levels: LOW, MEDIUM, HIGH, URGENT
- TaskStatus — the lifecycle stage of a task: TODO → IN_PROGRESS → REVIEW → DONE
- created_at — never changes, records when the task was first created
- updated_at — changes every time any detail of the task is edited
- completed_at — only set when the task is marked as DONE
- Tags — labels used to group and filter tasks

Think of it like a sticky note at work:
- TaskPriority is the colour of the note (urgent = red, low = green)
- TaskStatus is which column on the board it sits in
- Timestamps are the dates written on the note

My answers to the AI's domain model questions:

1. A task is overdue if it has a due date, that date has passed, and it has not been marked DONE yet.
2. If you edit a task title, only updated_at changes. created_at stays the same and completed_at stays empty.
3. The correct order of status changes is: TODO → IN_PROGRESS → REVIEW → DONE.
4. completed_at is separate from updated_at because updated_at changes for any edit, while completed_at records only the exact moment the task was finished.
5. URGENT tasks should appear at the top of the list, followed by HIGH, MEDIUM, and LOW. URGENT tasks might also trigger notifications.



## 3. Approach to Implementing a New Business Rule

If I were asked to add a new feature to this codebase, I would follow this process:

1. Start at cli.py to understand how user commands are structured
2. Follow the call chain to task_manager.py to see where the business logic lives
3. Check models.py to understand what data structures are available
4. Check storage.py to understand how data is saved and loaded
5. Ask the AI to help me locate similar existing features so I can follow the same pattern
6. Make changes in the file that matches the responsibility of my new feature



## 4. Strategies for Approaching Unfamiliar Code in Future

- Read the README file first before looking at any code
- Look at all the file names and guess what each one does before asking AI
- Use AI prompts in order: structure first, then specific features, then business logic
- Ask the AI to correct your understanding rather than just explain everything from scratch — that way you learn more
- Run the app first if possible to see what it does from a user's perspective before touching the code
- Use specific search terms like function names rather than broad words like "create" or "add"



## Reflection

**Which prompt was most helpful?**
Prompt 1 was the most helpful because understanding the overall structure first made everything else easier to follow. Once I knew what each file was responsible for and how they connected, the other prompts made much more sense.

**What would I do differently next time?**
I would read the README first and try to form stronger initial guesses before sending the prompt. The better my starting guess, the more useful the AI's corrections and additions were.

**What additional tools would complement AI prompting?**
Actually running the app to see what it does from a user's perspective, reading any diagrams the team has made, and asking a team member for a quick walkthrough would all complement AI prompting well.

`github.com/lestokolo026j-eng/ai-code-exercises-wtc`

Ready to paste it in?
