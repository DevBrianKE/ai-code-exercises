# Code Understanding Journal
Task Manager Exploration

## Project Overview

Language: Python

Main Files Identified:

- cli.py → likely handles user commands
- models.py → defines task structure
- storage.py → handles saving/loading tasks
- task_manager.py → main logic controller
- task_parser.py → converts text into tasks
- task_priority.py → handles priority logic

Initial Thoughts:

This looks like a command-line task manager that:
- Creates tasks
- Updates status
- Stores tasks
- Handles priorities

# Part 1: DUnderstanding a Specific Feature

### Task Class (models.py)

Purpose:
Represents a single task in the system.

Key Attributes:
- id → unique task identifier
- title → short task name
- description → details
- priority → importance level
- status → current state (TODO by default)
- created_at → creation time
- updated_at → last update time
- completed_at → completion time
- due_date → optional deadline
- tags → list of labels

Key Observations:
- Each task automatically gets a unique ID.
- Status starts as TODO.
- Timestamps are tracked automatically.
- Priority and status likely use enums.


# Part 2: Deepening Understanding of Task Prioritization System

## Initial Understanding

Before closely analyzing the code, I thought task prioritization was based mainly on a simple rule like:

- High priority tasks always come first  
- Due dates might slightly affect ordering  
- Tasks were likely sorted using a single attribute like priority level  

I also assumed the system might be using basic sorting logic rather than a calculated scoring system.

---

## What I Discovered After Examining the Code

After analyzing `task_priority.py`, I discovered that prioritization is not based on a single attribute.

Instead:

- Each task is assigned a numerical score  
- The score is calculated using multiple factors:
  - Priority level (LOW → URGENT)
  - Due date proximity (overdue, due today, soon)
  - Status (DONE reduces score, REVIEW reduces score slightly)
  - Tags (e.g. blocker, critical increase score)
  - Recent updates (recently updated tasks get a boost)

Then:

- Tasks are sorted using this score (highest first)
- The system dynamically ranks tasks based on combined logic

---

## Key Insights from Guided Questions

Through deeper questioning and analysis, I learned that:

- Priority is not absolute — it is weighted with other factors  
- A task with lower priority can outrank a higher one if it is overdue or tagged as critical  
- The system is designed to reflect “real-world urgency,” not just labels  
- Sorting happens after score calculation, not during task creation  
- The logic is centralized in one scoring function, making it reusable  

---

## Misconceptions I Had (Corrected)

- I initially thought priority alone determined order → Incorrect  
- I thought sorting was simple attribute-based sorting → Incorrect  
- I assumed due date was the main factor → Incorrect  
- I did not realize status (DONE/REVIEW) actively reduces priority score  

---

## Final Understanding

Task prioritization in this system is multi-factor scoring-based ranking, not simple sorting.

It combines multiple real-world signals into a single score, then sorts tasks based on that computed importance.

---

## Key Learning Outcome

I learned that real-world systems rarely rely on a single variable for decision-making. Instead, they combine multiple signals into a scoring system to make more intelligent decisions.


# Part 3: Mapping Data Flow and State Management

## Feature: Marking a Task as Complete

### Entry Point (User Action)

The process starts in `cli.py` when the user runs:

status <task_id> done


This triggers:

task_manager.update_task_status(task_id, "done")


---

## Application Flow (Step-by-step)

- User enters command in CLI  
- `cli.py` parses command using `argparse`  
- `TaskManager.update_task_status()` is called  
- `TaskStorage.get_task(task_id)` retrieves task from memory  
- If status is DONE:
  - `task.mark_as_done()` is executed  
- Task state is updated:
  - `status → DONE`
  - `completed_at → current time`
  - `updated_at → current time`
- `TaskStorage.save()` writes updated data to JSON file  

---

## State Changes During Completion

When a task is marked as DONE:

- `status → TaskStatus.DONE`
- `completed_at → datetime.now()`
- `updated_at → datetime.now()`

---

## Data Flow Diagram (Text Version)
User CLI Command
→ CLI Parser (argparse)
→ TaskManager.update_task_status()
→ TaskStorage.get_task()
→ Task.mark_as_done()
→ TaskStorage.save()
→ JSON file updated


---

## Persistence Layer

- Tasks are stored in `tasks.json`
- `TaskEncoder` converts Task objects → JSON
- `TaskDecoder` converts JSON → Task objects  

This ensures tasks persist between program runs.

---

## Potential Points of Failure

- Invalid task ID → task not found  
- Wrong status input → enum conversion error  
- File read/write errors in storage  
- Corrupted JSON file  
- Incorrect date handling  

---

## Key Insight

Task completion is not just a status change — it is a state mutation, timestamp update, and persistence operation.

---

## Final Understanding

The system follows this architecture:

CLI → Manager → Storage → Model → Storage (save)




