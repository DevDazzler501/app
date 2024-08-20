# app
test
import json
import os

class Task:
    def __init__(self, description, completed=False):
        self.description = description
        self.completed = completed

    def __str__(self):
        status = "✓" if self.completed else "✗"
        return f"{self.description} [{status}]"

class TaskManager:
    def __init__(self, filename="tasks.json"):
        self.filename = filename
        self.tasks = []
        self.load_tasks()

    def load_tasks(self):
        if os.path.exists(self.filename):
            with open(self.filename, "r") as file:
                tasks_data = json.load(file)
                self.tasks = [Task(**data) for data in tasks_data]
        else:
            self.tasks = []

    def save_tasks(self):
        with open(self.filename, "w") as file:
            json.dump([task.__dict__ for task in self.tasks], file, indent=4)

    def add_task(self, description):
        new_task = Task(description)
        self.tasks.append(new_task)
        self.save_tasks()

    def remove_task(self, index):
        if 0 <= index < len(self.tasks):
            del self.tasks[index]
            self.save_tasks()

    def complete_task(self, index):
        if 0 <= index < len(self.tasks):
            self.tasks[index].completed = True
            self.save_tasks()

    def display_tasks(self):
        if not self.tasks:
            print("No tasks found.")
        else:
            for i, task in enumerate(self.tasks, 1):
                print(f"{i}. {task}")

def main():
    task_manager = TaskManager()

    while True:
        print("\nTask Manager Menu")
        print("1. Add task")
        print("2. Remove task")
        print("3. Mark task as completed")
        print("4. View tasks")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            description = input("Enter task description: ")
            task_manager.add_task(description)
        elif choice == "2":
            task_manager.display_tasks()
            index = int(input("Enter the number of the task to remove: ")) - 1
            task_manager.remove_task(index)
        elif choice == "3":
            task_manager.display_tasks()
            index = int(input("Enter the number of the task to mark as completed: ")) - 1
            task_manager.complete_task(index)
        elif choice == "4":
            task_manager.display_tasks()
        elif choice == "5":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()

