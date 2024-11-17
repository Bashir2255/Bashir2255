import tkinter as tk
from tkinter import ttk

# Function to calculate the "Present On-Site"
def calculate_present_onsite():
    try:
        total_members = int(entry_total_members.get())
        present_country = int(entry_present_country.get())
        result = total_members - present_country
        entry_present_onsite.delete(0, tk.END)
        entry_present_onsite.insert(0, str(result))
    except ValueError:
        entry_present_onsite.delete(0, tk.END)
        entry_present_onsite.insert(0, "Error")

# Function to calculate the totals in the second page
def calculate_totals_page2():
    try:
        for row in range(1, 11):
            deposit = float(deposit_entries[row].get() or 0)
            marketing = float(marketing_entries[row].get() or 0)
            total_per_person = deposit + marketing
            total_entries[row].delete(0, tk.END)
            total_entries[row].insert(0, str(total_per_person))

        # Calculate column totals
        total_deposit = sum(float(deposit_entries[row].get() or 0) for row in range(1, 11))
        total_marketing = sum(float(marketing_entries[row].get() or 0) for row in range(1, 11))
        total_final = sum(float(total_entries[row].get() or 0) for row in range(1, 11))

        deposit_entries[11].delete(0, tk.END)
        deposit_entries[11].insert(0, str(total_deposit))

        marketing_entries[11].delete(0, tk.END)
        marketing_entries[11].insert(0, str(total_marketing))

        total_entries[11].delete(0, tk.END)
        total_entries[11].insert(0, str(total_final))
    except ValueError:
        pass

# Function to calculate the total food expenses
def calculate_food_expenses():
    try:
        total = sum(float(food_amounts[row].get() or 0) for row in range(2, 14))
        food_amounts[14].delete(0, tk.END)
        food_amounts[14].insert(0, str(total))
    except ValueError:
        pass

# Create the main application window
app = tk.Tk()
app.title("Management App")
app.geometry("800x600")

# Create tabs for each page
notebook = ttk.Notebook(app)
page1 = ttk.Frame(notebook)
page2 = ttk.Frame(notebook)
page3 = ttk.Frame(notebook)

notebook.add(page1, text="Page 1")
notebook.add(page2, text="Page 2")
notebook.add(page3, text="Page 3")
notebook.pack(expand=True, fill="both")

# Page 1: Total Members
tk.Label(page1, text="Total Members").grid(row=0, column=0, padx=10, pady=10)
entry_total_members = tk.Entry(page1)
entry_total_members.grid(row=0, column=1, padx=10, pady=10)

tk.Label(page1, text="Present in the Country").grid(row=1, column=0, padx=10, pady=10)
entry_present_country = tk.Entry(page1)
entry_present_country.grid(row=1, column=1, padx=10, pady=10)

tk.Label(page1, text="Present On-Site").grid(row=2, column=0, padx=10, pady=10)
entry_present_onsite = tk.Entry(page1)
entry_present_onsite.grid(row=2, column=1, padx=10, pady=10)

tk.Button(page1, text="Calculate", command=calculate_present_onsite).grid(row=3, column=0, columnspan=2, pady=20)

# Page 2: Deposit, Marketing, and Total Per Person
headers = ["Name", "Deposit", "Marketing", "Total Per Person"]
for col, header in enumerate(headers):
    tk.Label(page2, text=header).grid(row=0, column=col, padx=10, pady=5)

deposit_entries = {}
marketing_entries = {}
total_entries = {}

for row in range(1, 12):
    tk.Entry(page2).grid(row=row, column=0, padx=10, pady=5)  # Name column
    deposit_entries[row] = tk.Entry(page2)
    deposit_entries[row].grid(row=row, column=1, padx=10, pady=5)
    marketing_entries[row] = tk.Entry(page2)
    marketing_entries[row].grid(row=row, column=2, padx=10, pady=5)
    total_entries[row] = tk.Entry(page2)
    total_entries[row].grid(row=row, column=3, padx=10, pady=5)

tk.Button(page2, text="Calculate Totals", command=calculate_totals_page2).grid(row=12, column=0, columnspan=4, pady=20)

# Page 3: Food Expenses
tk.Label(page3, text="FOOD EXPENSES", font=("Arial", 14, "bold")).grid(row=0, column=0, columnspan=2, pady=10)
tk.Label(page3, text="Regerdings").grid(row=1, column=0, padx=10, pady=5)
tk.Label(page3, text="Amount").grid(row=1, column=1, padx=10, pady=5)

food_amounts = {}

for row in range(2, 15):
    tk.Entry(page3).grid(row=row, column=0, padx=10, pady=5)  # Regerdings column
    food_amounts[row] = tk.Entry(page3)
    food_amounts[row].grid(row=row, column=1, padx=10, pady=5)

tk.Button(page3, text="Calculate Total", command=calculate_food_expenses).grid(row=15, column=0, columnspan=2, pady=20)

# Run the app
app.mainloop()
