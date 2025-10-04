# Reaction-time-game
import tkinter as tk
import time
import random

class ReactionTester:
    def __init__(self, master):
        self.master = master
        self.master.title("Reaction Time Tester")
        self.master.geometry("400x300")
        self.master.configure(bg='gray')

        self.label = tk.Label(master, text="Click to start", font=("Arial", 20), bg='gray')
        self.label.pack(expand=True)

        self.master.bind("<Button-1>", self.start_test)

        self.waiting = False
        self.start_time = 0

    def start_test(self, event):
        if not self.waiting:
            self.label.config(text="Wait for green...", bg="red")
            self.master.configure(bg='red')
            self.master.after(random.randint(2000, 5000), self.change_color)
            self.waiting = True
            self.master.bind("<Button-1>", self.too_soon)
        else:
            self.end_test()

    def change_color(self):
        self.start_time = time.time()
        self.label.config(text="CLICK!", bg="green")
        self.master.configure(bg="green")
        self.master.bind("<Button-1>", self.end_test)

    def too_soon(self, event):
        if self.waiting:
            self.label.config(text="Too soon! Click to try again.", bg="gray")
            self.master.configure(bg="gray")
            self.master.bind("<Button-1>", self.start_test)
            self.waiting = False

    def end_test(self, event=None):
        if self.waiting and self.start_time > 0:
            reaction_time = (time.time() - self.start_time) * 1000  # milliseconds
            self.label.config(text=f"Reaction Time: {int(reaction_time)} ms\nClick to try again.", bg="gray")
            self.master.configure(bg="gray")
            self.waiting = False
            self.start_time = 0
            self.master.bind("<Button-1>", self.start_test)

# Run the app
root = tk.Tk()
app = ReactionTester(root)
root.mainloop()

