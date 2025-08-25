project 2  source code

import json
import hashlib
import os

# ------------------ Utility Functions ------------------

USERS_FILE = "users.json"
TASKS_FILE = "tasks.json"

def load_data(file):
    if not os.path.exists(file):
        return {}
    with open(file, "r") as f:
        return json.load(f)

def save_data(file, data):
    with open(file, "w") as f:
        json.dump(data, f, indent=4)

def hash_password(password):
    return hashlib.sha256(password.encode()).hexdigest()

# ------------------ Authentication ------------------

def register():
    users = load_data(USERS_FILE)
    username = input("Enter a username: ").strip()

    if username in users:
        print("Username already exists. Try another one.")
        return None

    password = input("Enter a password: ").strip()
    users[username] = hash_password(password)
    save_data(USERS_FILE, users)
    print("Registration successful! Please login.")
    return None

def login():
    users = load_data(USERS_FILE)
    username = input("Enter username: ").strip()
    password = input("Enter password: ").strip()

    if username in users and users[username] == hash_password(password):
        print(f"Welcome {username}!")
        return username
    else:
        print("Invalid credentials. Try again.")
        return None

# ------------------ Task Functions ------------------

def add_task(username):
    tasks = load_data(TASKS_FILE)
    task_desc = input("Enter task description: ").strip()
    task_id = str(len(tasks.get(username, [])) + 1)

    new_task = {"id": task_id, "description": task_desc, "status": "Pending"}
    tasks.setdefault(username, []).append(new_task)

    save_data(TASKS_FILE, tasks)
    print(f"Task '{task_desc}' added with ID {task_id}.")

def view_tasks(username):
    tasks = load_data(TASKS_FILE).get(username, [])
    if not tasks:
        print("No tasks found.")
        return
    print("\nYour Tasks:")
    for task in tasks:
        print(f"[{task['id']}] {task['description']} - {task['status']}")

def mark_task_completed(username):
    tasks = load_data(TASKS_FILE)
    user_tasks = tasks.get(username, [])

    if not user_tasks:
        print("No tasks to update.")
        return

    task_id = input("Enter task ID to mark as completed: ").strip()
    for task in user_tasks:
        if task["id"] == task_id:
            task["status"] = "Completed"
            save_data(TASKS_FILE, tasks)
            print(f"Task {task_id} marked as Completed.")
            return

    print("Task ID not found.")

def delete_task(username):
    tasks = load_data(TASKS_FILE)
    user_tasks = tasks.get(username, [])

    if not user_tasks:
        print("📭 No tasks to delete.")
        return

    task_id = input("Enter task ID to delete: ").strip()
    for task in user_tasks:
        if task["id"] == task_id:
            user_tasks.remove(task)
            save_data(TASKS_FILE, tasks)
            print(f"🗑️ Task {task_id} deleted.")
            return

    print("Task ID not found.")

# ------------------ Main Menu ------------------

def menu(username):
    while True:
        print("\n--- Task Manager ---")
        print("1. Add a Task")
        print("2. View Tasks")
        print("3. Mark Task as Completed")
        print("4. Delete a Task")
        print("5. Logout")

        choice = input("Choose an option: ").strip()

        if choice == "1":
            add_task(username)
        elif choice == "2":
            view_tasks(username)
        elif choice == "3":
            mark_task_completed(username)
        elif choice == "4":
            delete_task(username)
        elif choice == "5":
            print("👋 Logged out.")
            break
        else:
            print("Invalid option. Try again.")

# ------------------ Entry Point ------------------

def main():
    while True:
        print("\n--- Welcome to Task Manager ---")
        print("1. Register")
        print("2. Login")
        print("3. Exit")

        choice = input("Choose an option: ").strip()
        if choice == "1":
            register()
        elif choice == "2":
            user = login()
            if user:
                menu(user)
        elif choice == "3":
            print("Goodbye!")
            break
        else:
            print("Invalid option. Try again.")

if __name__ == "__main__":
    main()
