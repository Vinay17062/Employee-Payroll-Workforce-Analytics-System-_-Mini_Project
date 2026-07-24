import numpy as np
import os

# ---------------- Employee Class ----------------
class Employee:
    def __init__(self, emp_id, name, department, salary):
        self.emp_id = emp_id
        self.name = name
        self.department = department
        self.salary = salary

    def __str__(self):
        return f"{self.emp_id},{self.name},{self.department},{self.salary}"


# ---------------- Payroll System ----------------
class PayrollSystem:
    def __init__(self):
        self.employees = []

    # Add Employee
    def add_employee(self):
        try:
            emp_id = int(input("Enter Employee ID: "))
            name = input("Enter Name: ")
            department = input("Enter Department: ")
            salary = float(input("Enter Salary: "))

            if salary <= 0:
                raise ValueError("Salary must be greater than zero.")

            emp = Employee(emp_id, name, department, salary)
            self.employees.append(emp)
            print("Employee Added Successfully!\n")

        except ValueError as e:
            print("Error:", e)

    # Save to File
    def save_to_file(self):
        with open("employees.txt", "w") as file:
            for emp in self.employees:
                file.write(str(emp) + "\n")
        print("Data Saved Successfully!\n")

    # Load from File
    def load_from_file(self):
        if not os.path.exists("employees.txt"):
            print("No employee file found.\n")
            return

        self.employees.clear()

        with open("employees.txt", "r") as file:
            for line in file:
                data = line.strip().split(",")
                emp = Employee(int(data[0]), data[1], data[2], float(data[3]))
                self.employees.append(emp)

        print("Employee Data Loaded Successfully!\n")

    # Display Employees
    def display(self):
        if len(self.employees) == 0:
            print("No Employee Records.\n")
            return

        print("\nEmployee Records")
        print("-" * 60)
        for emp in self.employees:
            print(f"ID:{emp.emp_id}  Name:{emp.name}  Dept:{emp.department}  Salary:{emp.salary}")
        print()

    # Salary Statistics
    def salary_statistics(self):
        if len(self.employees) == 0:
            print("No Employee Data.\n")
            return

        salaries = np.array([emp.salary for emp in self.employees])

        print("\nSalary Statistics")
        print("----------------------")
        print("Mean Salary :", np.mean(salaries))
        print("Median Salary :", np.median(salaries))
        print("Standard Deviation :", np.std(salaries))
        print("Highest Salary :", np.max(salaries))
        print("Lowest Salary :", np.min(salaries))
        print()

    # Apply Increment
    def apply_increment(self):
        if len(self.employees) == 0:
            print("No Employee Data.\n")
            return

        percent = float(input("Enter Increment Percentage: "))

        salaries = np.array([emp.salary for emp in self.employees])

        updated = salaries + salaries * (percent / 100)

        for i in range(len(self.employees)):
            self.employees[i].salary = updated[i]

        print("Increment Applied Successfully!\n")

    # Department-wise Matrix
    def department_matrix(self):
        if len(self.employees) == 0:
            print("No Employee Data.\n")
            return

        departments = {}

        for emp in self.employees:
            if emp.department not in departments:
                departments[emp.department] = []
            departments[emp.department].append(emp.salary)

        print("\nDepartment Wise Salary Matrix")
        print("------------------------------")
        for dept, sal in departments.items():
            arr = np.array(sal)
            print(f"{dept} -> {arr} | Total Salary = {np.sum(arr)}")
        print()

    # Random Dataset
    def random_dataset(self):
        names = ["Amit", "Riya", "Rahul", "Sneha", "Neha",
                 "Karan", "Priya", "Arjun", "Pooja", "Vikas"]

        departments = ["HR", "IT", "Sales", "Finance"]

        self.employees.clear()

        for i in range(10):
            emp = Employee(
                i + 1,
                np.random.choice(names),
                np.random.choice(departments),
                np.random.randint(25000, 90000)
            )
            self.employees.append(emp)

        print("Random Employee Dataset Generated!\n")

    # Boolean Indexing
    def filter_salary(self):
        if len(self.employees) == 0:
            print("No Employee Data.\n")
            return

        limit = float(input("Show Employees with Salary Greater Than: "))

        salaries = np.array([emp.salary for emp in self.employees])

        mask = salaries > limit

        print("\nFiltered Employees")
        print("----------------------")
        for emp, m in zip(self.employees, mask):
            if m:
                print(emp.emp_id, emp.name, emp.department, emp.salary)
        print()


# ---------------- Main Program ----------------

system = PayrollSystem()

while True:
    print("========== Employee Payroll System ==========")
    print("1. Add Employee")
    print("2. Display Employees")
    print("3. Save to File")
    print("4. Load from File")
    print("5. Salary Statistics")
    print("6. Apply Increment")
    print("7. Department-wise Matrix")
    print("8. Generate Random Dataset")
    print("9. Filter Salary")
    print("10. Exit")

    choice = input("Enter Choice: ")

    if choice == "1":
        system.add_employee()

    elif choice == "2":
        system.display()

    elif choice == "3":
        system.save_to_file()

    elif choice == "4":
        system.load_from_file()

    elif choice == "5":
        system.salary_statistics()

    elif choice == "6":
        system.apply_increment()

    elif choice == "7":
        system.department_matrix()

    elif choice == "8":
        system.random_dataset()

    elif choice == "9":
        system.filter_salary()

    elif choice == "10":
        print("Thank You!")
        break

    else:
        print("Invalid Choice!\n")
