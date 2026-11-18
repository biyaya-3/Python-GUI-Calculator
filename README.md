#Blessel Mae F Mondoy, BSS 3-A, CT5, Python Calculator in TKinter
#This Tkinter calculator features a deep navy blue background and custom color-coded buttons, complete with precise cursor navigation (< and ►), an integrated backspace function, and a high-visibility blinking entry cursor to make editing complex mathematical expressions seamless.


<img width="1366" height="768" alt="Calculator_CT5_Mondoy" src="https://github.com/user-attachments/assets/f223eead-daa8-4605-95e5-c43a71dfd9f5" />

# Python-GUI-Calculator
#This Repository include my Projects for CT5.

# Importing Tkinter library
import tkinter as tk 

# Creating the Main Window
window = tk.Tk()
window.title("Modern Calculator")
window.geometry("400x620")
window.resizable(False, False)
window.configure(bg="#081947")

# Creating a Variable for the Expression
expression = ""

# Creating the Calculator Display
display = tk.Entry(
    window,
    font=("Arial", 28),
    bg="#1e1e1e",
    fg="white",
    insertbackground="white",  
    insertwidth=3,           
    justify="right",
    bd=0,
    relief="flat"
)

# Positioning the Display
display.pack(
    fill="x",
    padx=20,
    pady=25,
    ipady=20
)

# Creating a Function for Keyboard Input Validation
def validate_input(event):
    if event.char.isdigit() or event.keysym in (
        "BackSpace", "Delete", "Left", "Right", "period", "plus", "minus", "asterisk", "slash"
    ):
        return None
    return "break"

# Synchronizing the Display and Expression Variable
def sync_expression(event=None):
    global expression
    expression = display.get()

# Connecting Keyboard Events Using bind()
display.bind("<Key>", validate_input)
display.bind("<KeyRelease>", sync_expression)

# Creating the Button Click Function (Inserts at current cursor position & keeps focus)
def button_click(value):
    display.focus_set()  # Keeps the blinking cursor active in the display
    display.insert(tk.INSERT, str(value))
    sync_expression()

# Creating the Clear Function
def clear():
    display.focus_set()
    global expression
    expression = ""
    display.delete(0, tk.END)

# Creating the Backspace Function (Deletes character just before cursor & keeps focus)
def backspace():
    display.focus_set()
    current_pos = display.index(tk.INSERT)
    if current_pos > 0:
        display.delete(current_pos - 1, current_pos)
        sync_expression()

# Creating Left and Right Navigation Functions (Keeps focus so blinking line moves and stays visible)
def move_cursor(direction):
    try:
        display.focus_set()
        current_pos = display.index(tk.INSERT)
        new_pos = current_pos + direction
        if 0 <= new_pos <= len(display.get()):
            display.icursor(new_pos)
    except Exception:
        pass

# Creating the Calculate Function
def calculate():
    display.focus_set()
    global expression
    try:
        expr = display.get()
        # Evaluating the mathematical expression safely
        result = eval(expr)
        display.delete(0, tk.END)
        display.insert(tk.END, str(result))
        expression = str(result)
    except Exception:
        display.delete(0, tk.END)
        display.insert(tk.END, "ERROR")
        expression = ""

# Creating a Two-Dimensional List for the Buttons 
buttons = [
    ["C", "⌫", "<", ">"],
    ["7", "8", "9", "/"],
    ["4", "5", "6", "*"],
    ["1", "2", "3", "-"],
    ["0", ".", "=", "+"]
]

# Creating a Frame for Buttons
button_frame = tk.Frame(
    window, 
    bg="#121212"
)

# Positioning the Frame
button_frame.pack(
    expand=True,
    fill="both",
    padx=15,
    pady=10
)

# Generating Buttons Using Nested Loops
for row_index, row in enumerate(buttons):
    for col_index, value in enumerate(row):
        if value == "=":
            command = calculate
            color = "#00e1ff"  
            text_color = "#121212"
        elif value == "C":
            command = clear
            color = "#9fb8e7"
            text_color = "white"
        elif value == "⌫":
            command = backspace
            color = "#4a4a4a" 
            text_color = "white"
        elif value == "<":
            command = lambda: move_cursor(-1)
            color = "#97bdcf" 
            text_color = "white"
        elif value == ">":
            command = lambda: move_cursor(1)
            color = "#97bdcf" 
            text_color = "white"
        elif value in ["+", "-", "*", "/"]:
            command = lambda v=value: button_click(v)
            color = "#3d7fe2" 
            text_color = "white"
        else:
            command = lambda v=value: button_click(v)
            color = "#2a2a2a" 
            text_color = "white"

# Creating Each Calculator Button
btn = tk.Button(
  button_frame,
  text=value,
  font=("Arial", 18, "bold"),
  bg=color,
  fg=text_color,
  activebackground="#4a4a4a",
  activeforeground="white",
  bd=0,
  relief="flat",
  command=command,
  takefocus=False  # Prevents buttons from trapping keyboard focus
  )

# Positioning the Generated Buttons
btn.grid(
  row=row_index,
  column=col_index,
  sticky="nsew",
  padx=5,
  pady=5)

# Configuring Grid Rows and Columns to scale smoothly
for i in range(len(buttons)):
    button_frame.grid_rowconfigure(i, weight=1)

for i in range(4):
    button_frame.grid_columnconfigure(i, weight=1)

# Running the application
window.mainloop()
