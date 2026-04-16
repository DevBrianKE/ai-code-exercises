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
