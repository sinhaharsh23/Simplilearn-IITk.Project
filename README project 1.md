Add an expense:


expenses = []

def add_expense():

    date = input("Enter the date (YYYY-MM-DD): ")
    category = input("Enter the category (e.g., Food, Travel): ")
    amount = float(input("Enter the amount spent: "))
    description = input("Enter a brief description: ")
    
    # Create dictionary for this expense
    expense = {
        'date': date,
        'category': category,
        'amount': amount,
        'description': description
    }
    
    # Add to list
    expenses.append(expense)
    print("\n Expense added successfully!\n")

  

  View expenses:  ( STEP 2 )

  def add_expense():

    date = input("Enter the date (YYYY-MM-DD): ")
    category = input("Enter the category (e.g., Food, Travel): ")
    amount = float(input("Enter the amount spent: "))
    description = input("Enter a brief description: ")

    # Create dictionary for this expense
    expense = {
        'date': date,
        'category': category,
        'amount': amount,
        'description': description
    }


    expenses.append(expense)
    print("\n Expense added successfully!\n")


def view_expenses():
    """Function to display all stored expenses with validation"""
    if not expenses:
        print("\n No expenses found.\n")
        return

    print("\n Expense Records:")
    for i, exp in enumerate(expenses, start=1):
        if all(exp.get(key) for key in ['date', 'category', 'amount', 'description']):
            print(f"\nExpense {i}:")
            print(f" Date        : {exp['date']}")
            print(f" Category    : {exp['category']}")
            print(f" Amount      : ₹{exp['amount']}")
            print(f" Description : {exp['description']}")
        else:
            print(f"\n Expense {i} skipped due to missing details.")
    print()


def main():
    """Main menu for the expense tracker"""
    while True:
        print("===== Personal Expense Tracker =====")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Exit")

        choice = input("Enter your choice (1-3): ").strip()

        if choice == '1':
            add_expense()
        elif choice == '2':
            view_expenses()
        elif choice == '3':
            print("Exiting... Goodbye!")
            break
        else:
            print("Invalid choice. Please enter 1, 2, or 3.\n")


# Run the program
if __name__ == "__main__":
    main()

add_expense()
print("Current Expenses:", expenses)




  Set and track the budget:  ( step 3)

  # Personal Expense Tracker with Budget Feature

expenses = []       # Global list to store all expenses
monthly_budget = 0  # Global variable to store budget


def add_expense():
    """Function to add an expense to the expenses list"""
    date = input("Enter the date (YYYY-MM-DD): ").strip()
    category = input("Enter the category (e.g., Food, Travel): ").strip()
    
    try:
        amount = float(input("Enter the amount spent: "))
    except ValueError:
        print("Invalid amount. Please enter a number.")
        return
    
    description = input("Enter a brief description: ").strip()
    
    expense = {
        'date': date,
        'category': category,
        'amount': amount,
        'description': description
    }
    
    expenses.append(expense)
    print("\n Expense added successfully!\n")


def view_expenses():
    """Function to display all stored expenses with validation"""
    if not expenses:
        print("\n No expenses found.\n")
        return
    
    print("\n Expense Records:")
    for i, exp in enumerate(expenses, start=1):
        if all(exp.get(key) for key in ['date', 'category', 'amount', 'description']):
            print(f"\nExpense {i}:")
            print(f" Date        : {exp['date']}")
            print(f" Category    : {exp['category']}")
            print(f" Amount      : ₹{exp['amount']}")
            print(f" Description : {exp['description']}")
        else:
            print(f"\n Expense {i} skipped due to missing details.")
    print()


def set_budget():
    """Function to set the monthly budget"""
    global monthly_budget
    try:
        monthly_budget = float(input("Enter your monthly budget: "))
        print(f"\n Monthly budget set to ₹{monthly_budget}\n")
    except ValueError:
        print("Invalid input. Please enter a number.\n")


def track_budget():
    """Function to track expenses against the monthly budget"""
    if monthly_budget == 0:
        print("\n No budget set. Please set a budget first.\n")
        return
    
    total_expenses = sum(exp['amount'] for exp in expenses if exp.get('amount'))
    
    print(f"\n Total Expenses so far: ₹{total_expenses}")
    print(f"Monthly Budget: ₹{monthly_budget}")
    
    if total_expenses > monthly_budget:
        print("You have exceeded your budget!")
    else:
        remaining = monthly_budget - total_expenses
        print(f"You have ₹{remaining} left for the month.\n")


def main():
    """Main menu for the expense tracker"""
    while True:
        print("===== Personal Expense Tracker =====")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Set Budget")
        print("4. Track Budget")
        print("5. Exit")
        
        choice = input("Enter your choice (1-5): ").strip()
        
        if choice == '1':
            add_expense()
        elif choice == '2':
            view_expenses()
        elif choice == '3':
            set_budget()
        elif choice == '4':
            track_budget()
        elif choice == '5':
            print("Exiting... Goodbye!")
            break
        else:
            print("Invalid choice. Please enter 1-5.\n")


# Run the program
if __name__ == "__main__":
    main()




Save and load expenses: ( step 4)


import csv
import os

# File name for storing expenses
CSV_FILE = "expenses.csv"

# Global variables
expenses = []
monthly_budget = 0


def load_expenses():
    """Load expenses from CSV file"""
    global expenses
    if os.path.exists(CSV_FILE):
        with open(CSV_FILE, "r", newline="", encoding="utf-8") as file:
            reader = csv.DictReader(file)
            expenses = []
            for row in reader:
                try:
                    expense = {
                        'date': row['date'],
                        'category': row['category'],
                        'amount': float(row['amount']),
                        'description': row['description']
                    }
                    expenses.append(expense)
                except ValueError:
                    continue  # Skip invalid rows


def save_expenses():
    """Save expenses to CSV file"""
    with open(CSV_FILE, "w", newline="", encoding="utf-8") as file:
        fieldnames = ['date', 'category', 'amount', 'description']
        writer = csv.DictWriter(file, fieldnames=fieldnames)
        writer.writeheader()
        for exp in expenses:
            writer.writerow(exp)


def add_expense():
    """Add a new expense entry"""
    date = input("Enter the date (YYYY-MM-DD): ").strip()
    category = input("Enter the category (e.g., Food, Travel): ").strip()
    
    try:
        amount = float(input("Enter the amount spent: "))
    except ValueError:
        print("Invalid amount. Please enter a number.")
        return
    
    description = input("Enter a brief description: ").strip()
    
    expense = {
        'date': date,
        'category': category,
        'amount': amount,
        'description': description
    }
    
    expenses.append(expense)
    save_expenses()
    print("\n Expense added successfully!\n")


def view_expenses():
    """Display all stored expenses"""
    if not expenses:
        print("\n No expenses found.\n")
        return
    
    print("\n Expense Records:")
    for i, exp in enumerate(expenses, start=1):
        print(f"\nExpense {i}:")
        print(f" Date        : {exp['date']}")
        print(f" Category    : {exp['category']}")
        print(f" Amount      : ₹{exp['amount']}")
        print(f" Description : {exp['description']}")
    print()


def set_budget():
    """Set the monthly budget"""
    global monthly_budget
    try:
        monthly_budget = float(input("Enter your monthly budget: "))
        print(f"\n Monthly budget set to ₹{monthly_budget}\n")
    except ValueError:
        print(" Invalid input. Please enter a number.\n")


def track_budget():
    """Track expenses against the monthly budget"""
    if monthly_budget == 0:
        print("\n No budget set. Please set a budget first.\n")
        return
    
    total_expenses = sum(exp['amount'] for exp in expenses)
    
    print(f"\n Total Expenses so far: ₹{total_expenses}")
    print(f" Monthly Budget: ₹{monthly_budget}")
    
    if total_expenses > monthly_budget:
        print("You have exceeded your budget!")
    else:
        remaining = monthly_budget - total_expenses
        print(f"You have ₹{remaining} left for the month.\n")


def main():
    """Main menu for the expense tracker"""
    load_expenses()  # Load previous data at startup
    
    while True:
        print("===== Personal Expense Tracker =====")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Set Budget")
        print("4. Track Budget")
        print("5. Exit")
        
        choice = input("Enter your choice (1-5): ").strip()
        
        if choice == '1':
            add_expense()
        elif choice == '2':
            view_expenses()
        elif choice == '3':
            set_budget()
        elif choice == '4':
            track_budget()
        elif choice == '5':
            save_expenses()
            print("Exiting... Expenses saved to file. Goodbye!")
            break
        else:
            print("Invalid choice. Please enter 1-5.\n")


# Run program
if __name__ == "__main__":
    main()



Create an interactive menu:  ( step 5 )


import csv
import os

# File to store expenses
CSV_FILE = "expenses.csv"

# Global data
expenses = []
monthly_budget = 0


# --------------------- File Handling ---------------------
def load_data():
    """Load budget and expenses from CSV file"""
    global expenses, monthly_budget
    if os.path.exists(CSV_FILE):
        with open(CSV_FILE, "r", newline="", encoding="utf-8") as file:
            reader = csv.reader(file)
            rows = list(reader)

            expenses = []
            monthly_budget = 0

            for row in rows:
                if not row:
                    continue
                if row[0] == "BUDGET":
                    try:
                        monthly_budget = float(row[1])
                    except ValueError:
                        monthly_budget = 0
                elif row[0] != "date":  # skip header
                    try:
                        expense = {
                            'date': row[0],
                            'category': row[1],
                            'amount': float(row[2]),
                            'description': row[3]
                        }
                        expenses.append(expense)
                    except (ValueError, IndexError):
                        continue


def save_data():
    """Save budget and expenses to CSV file"""
    with open(CSV_FILE, "w", newline="", encoding="utf-8") as file:
        writer = csv.writer(file)

        # Save budget
        writer.writerow(["BUDGET", monthly_budget])

        # Save expenses header
        writer.writerow(["date", "category", "amount", "description"])

        # Save all expenses
        for exp in expenses:
            writer.writerow([exp['date'], exp['category'], exp['amount'], exp['description']])


# --------------------- Core Functionalities ---------------------
def add_expense():
    """Add a new expense"""
    date = input("Enter the date (YYYY-MM-DD): ").strip()
    category = input("Enter the category (e.g., Food, Travel): ").strip()

    try:
        amount = float(input("Enter the amount spent: "))
    except ValueError:
        print("Invalid amount. Please enter a number.")
        return

    description = input("Enter a brief description: ").strip()

    expense = {
        'date': date,
        'category': category,
        'amount': amount,
        'description': description
    }

    expenses.append(expense)
    print("\n✅ Expense added successfully!\n")


def view_expenses():
    """View all expenses"""
    if not expenses:
        print("\n No expenses found.\n")
        return

    print("\n Expense Records:")
    for i, exp in enumerate(expenses, start=1):
        print(f"\nExpense {i}:")
        print(f" Date        : {exp['date']}")
        print(f" Category    : {exp['category']}")
        print(f" Amount      : ₹{exp['amount']}")
        print(f" Description : {exp['description']}")
    print()


def track_budget():
    """Check expenses against the budget"""
    if monthly_budget == 0:
        print("\n No budget set. Please set one by editing the CSV file.\n")
        return

    total_expenses = sum(exp['amount'] for exp in expenses)

    print(f"\n Total Expenses so far: ₹{total_expenses}")
    print(f"Monthly Budget: ₹{monthly_budget}")

    if total_expenses > monthly_budget:
        print("You have exceeded your budget!")
    else:
        remaining = monthly_budget - total_expenses
        print(f"You have ₹{remaining} left for the month.\n")


# --------------------- Interactive Menu ---------------------
def menu():
    """Display menu and handle choices"""
    load_data()  # Load saved data

    while True:
        print("===== Personal Expense Tracker =====")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Track Budget")
        print("4. Save Expenses")
        print("5. Exit")

        choice = input("Enter your choice (1-5): ").strip()

        if choice == '1':
            add_expense()
        elif choice == '2':
            view_expenses()
        elif choice == '3':
            track_budget()
        elif choice == '4':
            save_data()
            print("Expenses saved successfully!\n")
        elif choice == '5':
            save_data()
            print("Exiting... Data saved. Goodbye!")
            break
        else:
            print("Invalid choice. Please enter 1-5.\n")


# --------------------- Run Program ---------------------
if __name__ == "__main__":
    menu()


