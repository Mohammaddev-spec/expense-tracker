# expense-tracker
[Expense_tracker.py](https://github.com/user-attachments/files/32307796/Expense_tracker.py)
import os
from datetime import datetime


def load_expenses():
    expenses = []

    if os.path.exists("expenses.txt"):
        file = open("expenses.txt", "r")

        for line in file:
            line = line.strip()

            if line == "":
                continue

            parts = line.split(" - ")

            try:
                if len(parts) == 4:
                    expense = {
                        "name": parts[0],
                        "category": parts[1],
                        "price": int(parts[2]),
                        "datetime": parts[3]
                    }

                elif len(parts) == 2:
                    expense = {
                        "name": parts[0],
                        "category": "other",
                        "price": int(parts[1]),
                        "datetime": "unknown"
                    }

                else:
                    print("Skipped invalid line:", line)
                    continue

                expenses.append(expense)

            except ValueError:
                print("Skipped invalid price:", line)

        file.close()

    return expenses


def save_expenses(expenses):
    file = open("expenses.txt", "w")

    for expense in expenses:
        file.write(
            expense["name"]
            + " - "
            + expense["category"]
            + " - "
            + str(expense["price"])
            + " - "
            + expense["datetime"]
            + "\n"
        )

    file.close()


def load_income():
    if os.path.exists("income.txt"):
        file = open("income.txt", "r")
        content = file.read().strip()
        file.close()

        if content != "":
            try:
                return int(content)

            except ValueError:
                print("Invalid income data.")

    return 0


def save_income(income):
    file = open("income.txt", "w")
    file.write(str(income))
    file.close()


def set_income():
    while True:
        try:
            income = int(
                input("Enter your monthly income: ")
            )

            if income <= 0:
                print("Income must be greater than zero!")
                continue

            break

        except ValueError:
            print("Please enter a valid number!")

    save_income(income)

    print("Monthly income saved!")

    return income


def add_expense(expenses):
    name = input("Expense name: ").strip()

    if name == "":
        print("Name cannot be empty!")
        return

    category = input("Category: ").strip()

    if category == "":
        category = "other"

    while True:
        try:
            price = int(
                input("Expense price: ")
            )

            if price <= 0:
                print("Price must be greater than zero!")
                continue

            break

        except ValueError:
            print("Please enter a valid number!")

    now = datetime.now()
    date_and_time = now.strftime("%Y-%m-%d %H:%M")

    expense = {
        "name": name,
        "category": category,
        "price": price,
        "datetime": date_and_time
    }

    expenses.append(expense)

    save_expenses(expenses)

    print("Expense saved!")


def show_expenses(expenses):
    print("\nAll expenses:")

    if len(expenses) == 0:
        print("No expenses yet.")

    else:
        number = 1

        for expense in expenses:
            print(
                number,
                "-",
                expense["name"],
                "-",
                expense["category"],
                "-",
                expense["price"],
                "-",
                expense["datetime"]
            )

            number = number + 1


def show_total(expenses):
    total = 0

    for expense in expenses:
        total = total + expense["price"]

    print("\nTotal:", total)


def delete_expense(expenses):
    show_expenses(expenses)

    if len(expenses) == 0:
        return

    while True:
        choice = input(
            "Enter expense number to delete: "
        )

        try:
            number = int(choice)

            if number < 1 or number > len(expenses):
                print("Invalid expense number!")

            else:
                break

        except ValueError:
            print("Please enter a valid number!")

    deleted_expense = expenses.pop(number - 1)

    save_expenses(expenses)

    print(
        "Deleted:",
        deleted_expense["name"],
        "-",
        deleted_expense["price"]
    )


def edit_expense(expenses):
    show_expenses(expenses)

    if len(expenses) == 0:
        return

    while True:
        choice = input(
            "Enter expense number to edit: "
        )

        try:
            number = int(choice)

            if number < 1 or number > len(expenses):
                print("Invalid expense number!")

            else:
                break

        except ValueError:
            print("Please enter a valid number!")

    expense = expenses[number - 1]

    new_name = input(
        "Enter new name: "
    ).strip()

    if new_name == "":
        print("Name cannot be empty!")
        return

    new_category = input(
        "Enter new category: "
    ).strip()

    if new_category == "":
        new_category = "other"

    while True:
        try:
            new_price = int(
                input("Enter new price: ")
            )

            if new_price <= 0:
                print("Price must be greater than zero!")
                continue

            break

        except ValueError:
            print("Please enter a valid number!")

    expense["name"] = new_name
    expense["category"] = new_category
    expense["price"] = new_price

    save_expenses(expenses)

    print("Expense updated!")


def search_expenses(expenses):
    keyword = input(
        "Search by name or category: "
    ).strip().lower()

    results = []

    for expense in expenses:
        if (
            keyword in expense["name"].lower()
            or keyword in expense["category"].lower()
        ):
            results.append(expense)

    print("\nSearch results:")

    if len(results) == 0:
        print("No matching expenses found.")

    else:
        number = 1

        for expense in results:
            print(
                number,
                "-",
                expense["name"],
                "-",
                expense["category"],
                "-",
                expense["price"],
                "-",
                expense["datetime"]
            )

            number = number + 1


def calculate_goal(expenses, income):
    item_name = input(
        "What do you want to buy? "
    ).strip()

    if item_name == "":
        print("Item name cannot be empty!")
        return

    while True:
        try:
            item_price = int(
                input("Item price: ")
            )

            if item_price <= 0:
                print("Price must be greater than zero!")
                continue

            break

        except ValueError:
            print("Please enter a valid number!")

    total_expenses = 0

    for expense in expenses:
        total_expenses = total_expenses + expense["price"]

    monthly_saving = income - total_expenses

    print("\n--- Purchase Goal ---")
    print("Item:", item_name)
    print("Price:", item_price)
    print("Your monthly saving:", monthly_saving)

    if income == 0:
        print(
            "You haven't set your monthly income yet! "
            "Please set it first."
        )

    elif monthly_saving <= 0:
        print(
            "You are not saving any money right now, "
            "so this goal isn't reachable yet."
        )

    else:
        months_needed = item_price / monthly_saving
        months_needed = round(months_needed, 1)

        print(
            "You need about",
            months_needed,
            "months to afford this."
        )


expenses = load_expenses()
income = load_income()


while True:
    print("\n===== Expense Tracker =====")
    print("1. Add expense")
    print("2. Show expenses")
    print("3. Show total")
    print("4. Delete expense")
    print("5. Edit expense")
    print("6. Search expenses")
    print("7. Set/Update monthly income")
    print("8. Purchase goal calculator")
    print("9. Exit")

    choice = input("Choose an option: ")

    if choice == "1":
        add_expense(expenses)

    elif choice == "2":
        show_expenses(expenses)

    elif choice == "3":
        show_total(expenses)

    elif choice == "4":
        delete_expense(expenses)

    elif choice == "5":
        edit_expense(expenses)

    elif choice == "6":
        search_expenses(expenses)

    elif choice == "7":
        income = set_income()

    elif choice == "8":
        calculate_goal(expenses, income)

    elif choice == "9":
        print("Goodbye!")
        break

    else:
        print("Invalid option!")
