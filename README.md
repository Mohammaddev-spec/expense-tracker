# Expense Tracker (Python)

A simple command-line expense tracker built in Python. Tracks expenses by category, calculates savings, and estimates how long it will take to afford a purchase goal based on your income and spending.

## Features

- Add, edit, delete, and search expenses
- Categorize expenses (e.g. food, transport, bills)
- Automatic date & time tracking for each expense
- Monthly income tracking
- Purchase goal calculator: enter an item and its price, and the app estimates how many months you need to save for it
- Total spending summary
- Data persistence using local text files (no database required)

## How to run

    python main.py

## How it works

1. Set your monthly income (option 7)
2. Add your expenses with a name, category, and price (option 1)
3. Use option 8 to see how long it will take to save up for something you want to buy

## Example

    Choose an option: 8
    What do you want to buy? PS5
    Item price: 9000000

    --- Purchase Goal ---
    Item: PS5
    Price: 9000000
    Your monthly saving: 3000000
    You need about 3.0 months to afford this.

## Tech used

- Python 3
- Built-in modules only: os, datetime

## Planned improvements

- Move data storage to JSON for better structure
- Split code into multiple files (main, storage, expense logic)
- Add automated tests
- Optional GUI version
