# Assignment-2.-software-design
ASSIGNMENT 2
Phase 1 - Analysis of Code Violations

This first step is to analyze the provided Student Management System code to identify where it violates the software design principles: SOLID, DRY, KISS, and YAGNI.
1. SOLID Principles Violations

    Single Responsibility Principle (SRP):
        Violation: The Student class has multiple responsibilities. It not only holds student data but also manages updating its own data and displaying itself.
        Example: Methods like update_student and display_student in the Student class violate SRP because a class should have only one reason to change. Here, if we change how a student is displayed or updated, it affects the Student class.

    Open/Closed Principle (OCP):
        Violation: The system is not easily extendable for new features without modifying existing code. For example, if we need to add new functionality like searching students or exporting data, we would have to modify the existing classes heavily.
        Example: Adding a new feature would require direct modifications to the StudentManagementSystem class or StudentDatabase class.

    Dependency Inversion Principle (DIP):
        Violation: The StudentManagementSystem class is directly dependent on the StudentDatabase and Student classes. This tight coupling makes it harder to manage changes or swap out components.
        Example: Instead of depending directly on concrete classes, the system could benefit from depending on abstractions (e.g., using interfaces or abstract classes).

2. DRY (Don't Repeat Yourself)

    Violation: The code contains repetitive patterns that could be refactored.
    Example: In update_student_info of StudentManagementSystem, the loop that finds a student based on student_id is redundant because the same kind of searching is likely needed elsewhere (e.g., when deleting a student). This logic could be abstracted into a helper function to reduce repetition.

3. KISS (Keep It Simple, Stupid)

    Violation: The current code has some complexity that could be simplified.
    Example: The Student class has a method to update itself, which adds unnecessary complexity. This could be managed externally by the StudentManagementSystem or StudentDatabase. Additionally, methods like display_student add complexity and could be simplified by moving the logic to a dedicated service or utility class responsible for formatting output.

4. YAGNI (You Ain't Gonna Need It)

    Violation: The code contains features or methods that may not be necessary for the current requirements.
    Example: The display_student method inside the Student class may be an example of YAGNI. If the requirements only specify managing student data, formatting and printing should not be a concern of the Student class itself.

Summary of Violations

    SOLID: Multiple principles are violated, particularly SRP, OCP, and DIP.
    DRY: Repetition in methods that manipulate or search for student data.
    KISS: Unnecessary complexity in class responsibilities and methods.
    YAGNI: Methods like display_student may be unnecessary and not required by current needs.

Phase 2 - Refactoring

Below is the refactored version of the Student Management System code to align with SOLID principles, eliminate redundancy (DRY), simplify the design (KISS), and avoid unnecessary features (YAGNI). Additionally, a simple text-based menu system is implemented for user interaction.

Each section of the code includes comments to explain the key parts and the rationale behind the implementation.

# Student.py

class Student:
    """
    Represents a student with an ID, name, age, and major.
    Responsible for holding student data and providing update functionality.
    """

    def __init__(self, student_id, name, age, major):
        self.student_id = student_id
        self.name = name
        self.age = age
        self.major = major

    def update(self, name=None, age=None, major=None):
        """
        Updates the student's information if new data is provided.
        """
        if name is not None:
            self.name = name
        if age is not None:
            self.age = age
        if major is not None:
            self.major = major


# StudentDatabase.py

class StudentDatabase:
    """
    Manages the collection of students.
    Responsible for adding, removing, and retrieving students.
    """

    def __init__(self):
        self.students = {}  # Use a dictionary for fast lookup by student ID

    def add_student(self, student):
        """Adds a new student to the database."""
        self.students[student.student_id] = student

    def remove_student(self, student_id):
        """Removes a student from the database by ID."""
        if student_id in self.students:
            del self.students[student_id]

    def find_student(self, student_id):
        """Finds and returns a student by ID, or None if not found."""
        return self.students.get(student_id, None)

    def get_all_students(self):
        """Returns a list of all students in the database."""
        return list(self.students.values())


# StudentManagementSystem.py

class StudentManagementSystem:
    """
    High-level interface for managing student data.
    Responsible for handling the interaction between the database and the user interface.
    """

    def __init__(self):
        self.database = StudentDatabase()

    def add_new_student(self, student_id, name, age, major):
        """
        Adds a new student to the management system.
        """
        student = Student(student_id, name, age, major)
        self.database.add_student(student)

    def delete_student(self, student_id):
        """
        Deletes a student from the management system by ID.
        """
        self.database.remove_student(student_id)

    def update_student_info(self, student_id, name=None, age=None, major=None):
        """
        Updates a student's information in the management system.
        """
        student = self.database.find_student(student_id)
        if student:
            student.update(name, age, major)

    def display_all_students(self):
        """
        Displays all students in the management system.
        """
        students = self.database.get_all_students()
        for student in students:
            print(f"ID: {student.student_id}, Name: {student.name}, Age: {student.age}, Major: {student.major}")


# MenuSystem.py

class MenuSystem:
    """
    Provides a text-based menu for user interaction with the student management system.
    """

    def __init__(self):
        self.system = StudentManagementSystem()

    def display_menu(self):
        """Displays the available menu options to the user."""
        print("Student Management System Menu")
        print("1. Add Student")
        print("2. Delete Student")
        print("3. Update Student Information")
        print("4. View All Students")
        print("5. Exit")

    def add_student(self):
        """Prompts user for student details and adds a student."""
        student_id = input("Enter Student ID: ")
        name = input("Enter Student Name: ")
        age = input("Enter Student Age: ")
        major = input("Enter Student Major: ")
        self.system.add_new_student(student_id, name, int(age), major)
        print("Student added successfully!")

    def delete_student(self):
        """Prompts user for student ID and deletes the student."""
        student_id = input("Enter Student ID to delete: ")
        self.system.delete_student(student_id)
        print("Student deleted successfully!")

    def update_student_info(self):
        """Prompts user for student details and updates the student."""
        student_id = input("Enter Student ID to update: ")
        name = input("Enter new name (leave blank to keep unchanged): ")
        age = input("Enter new age (leave blank to keep unchanged): ")
        major = input("Enter new major (leave blank to keep unchanged): ")

        self.system.update_student_info(
            student_id,
            name if name else None,
            int(age) if age else None,
            major if major else None
        )
        print("Student information updated successfully!")

    def view_all_students(self):
        """Displays all students."""
        print("All Students:")
        self.system.display_all_students()

    def run(self):
        """Runs the menu system, allowing user interaction."""
        while True:
            self.display_menu()
            choice = input("Choose an option: ")

            if choice == '1':
                self.add_student()
            elif choice == '2':
                self.delete_student()
            elif choice == '3':
                self.update_student_info()
            elif choice == '4':
                self.view_all_students()
            elif choice == '5':
                print("Exiting the system.")
                break
            else:
                print("Invalid option. Please choose a valid option.")


# Main.py

if __name__ == "__main__":
    """
    Entry point for the application. Initializes and runs the menu system.
    """
    menu_system = MenuSystem()
    menu_system.run()

Key Changes and Improvements

SOLID Principles:

SRP (Single Responsibility Principle): Each class has a clear, single responsibility. Student handles only student data. StudentDatabase manages student storage and retrieval. StudentManagementSystem is the service layer handling operations between the UI and database. MenuSystem manages user interactions.

OCP (Open/Closed Principle): The code is open for extension (e.g., adding new features) but closed for modification (existing code doesn't need to change).

DIP (Dependency Inversion Principle): High-level modules (like MenuSystem) do not depend on low-level modules; they depend on abstractions (StudentManagementSystem).

DRY (Don't Repeat Yourself): Removed repetitive code (e.g., searching for students in multiple places).

KISS (Keep It Simple, Stupid): Simplified classes and methods to focus on a single responsibility.

YAGNI (You Ain't Gonna Need It): Removed unnecessary features (e.g., display_student method in Student class).

Menu System: Implemented a simple text-based menu for user interaction, making the system more user-friendly and interactive.
About

