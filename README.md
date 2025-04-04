import tkinter as tk
from tkinter import messagebox

class TaskManager:
    def __init__(self, root):
        self.root = root
        self.root.title("Gestor de Tareas")
        self.root.geometry("400x400")

        # Campo de entrada
        self.entry = tk.Entry(self.root, font=('Arial', 14))
        self.entry.pack(pady=10)
        self.entry.focus()

        # Lista de tareas
        self.listbox = tk.Listbox(self.root, font=('Arial', 14), selectmode=tk.SINGLE)
        self.listbox.pack(pady=10, fill=tk.BOTH, expand=True)

        # Botones
        self.btn_frame = tk.Frame(self.root)
        self.btn_frame.pack(pady=5)

        self.add_btn = tk.Button(self.btn_frame, text="Añadir Tarea", command=self.add_task)
        self.add_btn.grid(row=0, column=0, padx=5)

        self.complete_btn = tk.Button(self.btn_frame, text="Completar", command=self.complete_task)
        self.complete_btn.grid(row=0, column=1, padx=5)

        self.delete_btn = tk.Button(self.btn_frame, text="Eliminar", command=self.delete_task)
        self.delete_btn.grid(row=0, column=2, padx=5)

        # Atajos de teclado
        self.root.bind('<Return>', lambda event: self.add_task())
        self.root.bind('<c>', lambda event: self.complete_task())
        self.root.bind('<C>', lambda event: self.complete_task())
        self.root.bind('<d>', lambda event: self.delete_task())
        self.root.bind('<D>', lambda event: self.delete_task())
        self.root.bind('<Delete>', lambda event: self.delete_task())
        self.root.bind('<Escape>', lambda event: self.root.quit())

        # Diccionario para tareas completadas
        self.completed = {}

    def add_task(self):
        task = self.entry.get().strip()
        if task:
            self.listbox.insert(tk.END, task)
            self.completed[task] = False
            self.entry.delete(0, tk.END)
        else:
            messagebox.showwarning("Entrada Vacía", "Por favor ingrese una tarea")

    def complete_task(self):
        try:
            index = self.listbox.curselection()[0]
            task = self.listbox.get(index)
            if not self.completed[task]:
                self.listbox.delete(index)
                task = "✔️ " + task
                self.listbox.insert(index, task)
                self.completed[task] = True
        except IndexError:
            messagebox.showinfo("Sin Selección", "Seleccione una tarea para completar")

    def delete_task(self):
        try:
            index = self.listbox.curselection()[0]
            task = self.listbox.get(index)
            self.listbox.delete(index)
            self.completed.pop(task, None)
        except IndexError:
            messagebox.showinfo("Sin Selección", "Seleccione una tarea para eliminar")


if __name__ == "__main__":
    root = tk.Tk()
    app = TaskManager(root)
    root.mainloop()
