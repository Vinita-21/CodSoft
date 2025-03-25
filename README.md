# CodSoft
Python Projects 
import tkinter as tk
from tkinter import messagebox
from datetime import datetime

# Initialize task list
tasks = []

# Function to add a task
def add_task():
    task = task_entry.get()
    due_date = date_entry.get()
    priority = priority_var.get()

    if task and due_date and priority:
        try:
            datetime.strptime(due_date, "%Y-%m-%d")
            tasks.append({"task": task, "due_date": due_date, "priority": priority, "status": "Pending"})
            update_task_list()
            task_entry.delete(0, tk.END)
            date_entry.delete(0, tk.END)
            priority_var.set("Medium")
            messagebox.showinfo("Success", "Task added successfully!")
        except ValueError:
            messagebox.showerror("Error", "Invalid date format. Use YYYY-MM-DD.")
    else:
        messagebox.showwarning("Warning", "Please fill all fields.")

# Function to delete a selected task
def delete_task():
    selected_task = task_listbox.curselection()
    if selected_task:
        tasks.pop(selected_task[0])
        update_task_list()
        messagebox.showinfo("Success", "Task deleted successfully!")
    else:
        messagebox.showwarning("Warning", "Please select a task to delete.")

# Function to mark a task as completed
def mark_completed():
    selected_task = task_listbox.curselection()
    if selected_task:
        tasks[selected_task[0]]["status"] = "Completed"
        update_task_list()
    else:
        messagebox.showwarning("Warning", "Please select a task to mark as completed.")

# Function to update the task list display
def update_task_list():
    task_listbox.delete(0, tk.END)
    for task in tasks:
        task_text = f"{task['task']} | Due: {task['due_date']} | Priority: {task['priority']} | Status: {task['status']}"
        task_listbox.insert(tk.END, task_text)

# Create the main window
root = tk.Tk()
root.title("To-Do List App")
root.geometry("500x450")
root.config(bg="#FFD3D3")  # Pastel Pink background

# Title label with pastel blue text
title_label = tk.Label(root, text="To-Do List", font=("Arial", 18, "bold"), bg="#FFD3D3", fg="#A4D8FF")  # Pastel Blue text
title_label.pack(pady=10)

# Task entry section
task_frame = tk.Frame(root, bg="#FFD3D3")
task_frame.pack(pady=5)
task_label = tk.Label(task_frame, text="Task:", font=("Arial", 12), bg="#FFD3D3")
task_label.pack(side=tk.LEFT)
task_entry = tk.Entry(task_frame, width=30)
task_entry.pack(side=tk.LEFT, padx=5)

# Due date entry section
date_frame = tk.Frame(root, bg="#FFD3D3")
date_frame.pack(pady=5)
date_label = tk.Label(date_frame, text="Due Date (YYYY-MM-DD):", font=("Arial", 12), bg="#FFD3D3")
date_label.pack(side=tk.LEFT)
date_entry = tk.Entry(date_frame, width=15)
date_entry.pack(side=tk.LEFT, padx=5)

# Priority dropdown menu
priority_frame = tk.Frame(root, bg="#FFD3D3")
priority_frame.pack(pady=5)
priority_label = tk.Label(priority_frame, text="Priority:", font=("Arial", 12), bg="#FFD3D3")
priority_label.pack(side=tk.LEFT)
priority_var = tk.StringVar(value="Medium")
priority_menu = tk.OptionMenu(priority_frame, priority_var, "High", "Medium", "Low")
priority_menu.config(bg="#A4D8FF", fg="black", width=10)  # Pastel blue dropdown
priority_menu.pack(side=tk.LEFT, padx=5)

# Buttons with pastel-themed colors
button_frame = tk.Frame(root, bg="#FFD3D3")
button_frame.pack(pady=10)
add_button = tk.Button(button_frame, text="Add Task", command=add_task, width=10, bg="#A4D8FF", fg="black")
add_button.pack(side=tk.LEFT, padx=5)
delete_button = tk.Button(button_frame, text="Delete Task", command=delete_task, width=10, bg="#FFA4A4", fg="black")  # Soft pink
delete_button.pack(side=tk.LEFT, padx=5)
complete_button = tk.Button(button_frame, text="Mark Completed", command=mark_completed, width=15, bg="#A4D8FF", fg="black")
complete_button.pack(side=tk.LEFT, padx=5)

# Task list display
task_listbox = tk.Listbox(root, width=60, height=10, bg="#FFE4E4", fg="black")  # Soft pastel pink box
task_listbox.pack(pady=10)

# Run the GUI event loop
root.mainloop()
