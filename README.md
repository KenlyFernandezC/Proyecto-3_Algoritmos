# Proyecto 3
### Nombre: Kenly Augusto Fernandez Barbuis
### Carné: 23-6317 
### Sección: C

## Ejercicio en Py

---
```Py
import tkinter as tk
from tkinter import filedialog
from tkinter import messagebox
import webbrowser

class TextEditorApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Editor de Texto")
        self.file_path = None

        # Barra de menú
        menubar = tk.Menu(root)
        file_menu = tk.Menu(menubar, tearoff=0)
        edit_menu = tk.Menu(menubar, tearoff=0)
        help_menu = tk.Menu(menubar, tearoff=0)

        menubar.add_cascade(label="Archivo", menu=file_menu)
        menubar.add_cascade(label="Editar", menu=edit_menu)
        menubar.add_cascade(label="Ayuda", menu=help_menu)

        file_menu.add_command(label="Abrir", command=self.open_file)
        file_menu.add_command(label="Guardar", command=self.save_file)
        file_menu.add_command(label="Guardar como", command=self.save_file_as)

        edit_menu.add_command(label="Deshacer", command=self.undo)
        edit_menu.add_command(label="Rehacer", command=self.redo)

        help_menu.add_command(label="Información", command=self.show_info)
        help_menu.add_command(label="Manual de usuario", command=self.open_user_manual)
        help_menu.add_command(label="Integrantes", command=self.show_integrantes)

        root.config(menu=menubar)

        # Área de texto
        self.text_widget = tk.Text(root)
        self.text_widget.pack(fill="both", expand=True)

        # Historial de acciones de edición
        self.edit_history = []

        # Índice actual en el historial de acciones
        self.current_edit_index = -1

    def open_file(self):
        # Función para abrir un archivo y cargar su contenido en el área de texto
        file_path = filedialog.askopenfilename(filetypes=[("Archivos de texto", "*.txt *.cpp *.cs *.py")])
        if file_path:
            with open(file_path, "r") as file:
                content = file.read()
                self.text_widget.delete("1.0", "end")
                self.text_widget.insert("1.0", content)
            self.file_path = file_path

    def save_file(self):
        # Función para guardar el contenido del área de texto en el archivo actual
        if self.file_path:
            content = self.text_widget.get("1.0", "end")
            with open(self.file_path, "w") as file:
                file.write(content)
        else:
            messagebox.showerror("Error", "Ningún archivo abierto. Use 'Guardar como'.")

    def save_file_as(self):
        # Función para guardar el contenido del área de texto en un nuevo archivo
        file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Archivos de texto", "*.txt *.cpp *.cs *.py")])
        if file_path:
            content = self.text_widget.get("1.0", "end")
            with open(file_path, "w") as file:
                file.write(content)
            self.file_path = file_path

    def show_info(self):
        # Función para mostrar información sobre la aplicación
        info_text = """Editor de Texto v1.0

        Esta aplicación es un editor de texto simple desarrollado en Python con la biblioteca tkinter.

        Licencia: MIT License

        Puede usar, modificar y distribuir este software de acuerdo con los términos de la licencia.

        Autor: Kenly
        Contacto: kfernandezg@gmail.com"""
        messagebox.showinfo("Información", info_text)

    def open_user_manual(self):
        # Función para abrir un manual de usuario en línea en un navegador web
        manual_url = "https://github.com/KenlyFernandezC/Proyecto-3_Algoritmos.git"
        webbrowser.open(manual_url)

    def show_integrantes(self):
        # Función para mostrar información sobre los integrantes del grupo
        messagebox.showinfo("Integrantes", "Integrante del grupo:\nKenly Augusto Fernández Barbuis\nCarné: 7690-23-6317")

    def undo(self):
        # Función para deshacer la última acción de edición en el área de texto
        if self.current_edit_index > 0:
            self.current_edit_index -= 1
            self.text_widget.delete("1.0", "end")
            self.text_widget.insert("1.0", self.edit_history[self.current_edit_index])

    def redo(self):
        # Función para rehacer la última acción de edición deshecha
        if self.current_edit_index < len(self.edit_history) - 1:
            self.current_edit_index += 1
            self.text_widget.delete("1.0", "end")
            self.text_widget.insert("1.0", self.edit_history[self.current_edit_index])

    def add_edit_history(self):
        # Función para agregar la acción de edición actual al historial de edición
        current_content = self.text_widget.get("1.0", "end")
        
        # Borra las acciones posteriores al índice actual en el historial
        del self.edit_history[self.current_edit_index + 1:]
        
        self.edit_history.append(current_content)
        self.current_edit_index = len(self.edit_history) - 1

if __name__ == "__main__":
    root = tk.Tk()
    app = TextEditorApp(root)
    
    # Configura la función para agregar una acción de edición en cada modificación del texto
    app.text_widget.bind("<KeyRelease>", lambda event: app.add_edit_history())
    
    root.mainloop()

```

