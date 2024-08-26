#### **README File**

Below is the content of the README file for this GitHub repository.

```markdown
# Student Management System

A simple Python-based Student Management System that allows users to add, delete, update, and view student information. The system has been refactored to adhere to key software design principles: SOLID, DRY, KISS, and YAGNI.

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Design Principles](#design-principles)
- [Changes Made](#changes-made)
- [Future Improvements](#future-improvements)

## Features

- Add a new student
- Delete an existing student
- Update student information
- View all students
- Simple text-based menu for easy user interaction

## Requirements

- Python 3.x

## Installation

1. Clone the repository:

    ```bash
    git clone https://github.com/yourusername/student-management-system.git
    ```

2. Navigate to the project directory:

    ```bash
    cd student-management-system
    ```

## Usage

Run the `Main.py` script to start the system:

```bash
python Main.py
```

Follow the on-screen prompts to interact with the system.

## Design Principles

This system was refactored to follow these key software design principles:

- **SOLID Principles**: Each class has a single responsibility, the code is open for extension but closed for modification, and high-level modules are not dependent on low-level modules.
- **DRY (Don't Repeat Yourself)**: Removed redundant code to make the system more maintainable.
- **KISS (Keep It Simple, Stupid)**: Simplified class responsibilities and reduced unnecessary complexity.
- **YAGNI (You Ain't Gonna Need It)**: Removed unnecessary features to keep the codebase clean and focused on current requirements.

## Changes Made

1. **Refactored Classes**: Split responsibilities across multiple classes (`Student`, `StudentDatabase`, `StudentManagementSystem`, and `MenuSystem`) to adhere to SOLID principles.
2. **Removed Redundancy**: Combined duplicate logic (such as searching for students) into single methods.
3. **Simplified Methods**: Reduced the complexity of methods and removed unnecessary features.
4. **Implemented a Menu System**: Added a simple text-based menu for improved user interaction.

## Future Improvements

- Add persistent storage (e.g., a database or file system) to save student data.
- Enhance the user interface with a graphical UI (e.g., using Tkinter or PyQt).
- Implement more advanced search and sorting features for student records.
- Add user authentication to enhance security.

