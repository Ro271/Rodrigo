import tkinter as tk
from tkinter import ttk, messagebox, filedialog
import csv

# Clase para manejar los datos personales
class Persona:
    def __init__(self, nombre, edad, profesion, experiencia, sexo):
        self.nombre = nombre
        self.edad = edad
        self.profesion = profesion
        self.experiencia = experiencia
        self.sexo = sexo

    def to_list(self):
        return [self.nombre, self.edad, self.profesion, self.experiencia, self.sexo]

# Función para validar el inicio de sesión
def validar_login():
    usuario = usuario_entry.get().strip()
    contraseña = contraseña_entry.get().strip()
    
    if usuario == "rodrigo" and contraseña == "casa":
        login_window.destroy()  # Cierra la ventana de inicio de sesión
        mostrar_ventana_principal()  # Muestra la ventana principal
    else:
        messagebox.showerror("Error", "Usuario o contraseña incorrectos. Intente de nuevo.")

# Función para mostrar la ventana principal de la aplicación
def mostrar_ventana_principal():
    root = tk.Tk()
    root.title("Registro de Información Personal")
    root.geometry("800x600")  # Ajustar el tamaño de la ventana
    personas = []  # Lista para almacenar objetos Persona

    # Crear frame principal
    main_frame = ttk.Frame(root)
    main_frame.pack(fill=tk.BOTH, expand=True, padx=10, pady=10)

    # Frame para la entrada de datos
    entry_frame = ttk.LabelFrame(main_frame, text="Ingrese los datos")
    entry_frame.grid(row=0, column=0, padx=10, pady=10, sticky="n")

    # Frame para la lista de datos
    lista_frame = ttk.LabelFrame(main_frame, text="Datos registrados")
    lista_frame.grid(row=0, column=1, padx=10, pady=10, sticky="n")

    # Entradas para los datos
    ttk.Label(entry_frame, text="Nombre:").grid(row=0, column=0, padx=5, pady=5)
    nombre_entry = ttk.Entry(entry_frame, width=30)
    nombre_entry.grid(row=0, column=1, padx=5, pady=5)

    ttk.Label(entry_frame, text="Edad:").grid(row=1, column=0, padx=5, pady=5)
    edad_entry = ttk.Entry(entry_frame, width=10)
    edad_entry.grid(row=1, column=1, padx=5, pady=5)

    ttk.Label(entry_frame, text="Profesión:").grid(row=2, column=0, padx=5, pady=5)
    profesion_entry = ttk.Entry(entry_frame, width=30)
    profesion_entry.grid(row=2, column=1, padx=5, pady=5)

    ttk.Label(entry_frame, text="Años de Experiencia:").grid(row=3, column=0, padx=5, pady=5)
    experiencia_entry = ttk.Entry(entry_frame, width=10)
    experiencia_entry.grid(row=3, column=1, padx=5, pady=5)

    ttk.Label(entry_frame, text="Sexo:").grid(row=4, column=0, padx=5, pady=5)
    sexo_combobox = ttk.Combobox(entry_frame, values=["Hombre", "Mujer", "Otro"], state="readonly")
    sexo_combobox.grid(row=4, column=1, padx=5, pady=5)

    # Función para procesar la información y agregar a la lista personas
    def procesar_informacion():
        nombre = nombre_entry.get().strip()
        edad = edad_entry.get().strip()
        profesion = profesion_entry.get().strip()
        experiencia = experiencia_entry.get().strip()
        sexo = sexo_combobox.get()

        # Verificar si hay datos para procesar
        if nombre and edad.isdigit() and profesion and experiencia.isdigit() and sexo:
            persona = Persona(nombre, int(edad), profesion, int(experiencia), sexo)
            personas.append(persona)
            actualizar_lista()
            limpiar_entradas()
        else:
            messagebox.showerror("Error", "Por favor, ingrese datos válidos.")

    # Función para actualizar la lista de datos registrados
    def actualizar_lista():
        for widget in lista_frame.winfo_children():
            widget.destroy()
        
        for index, persona in enumerate(personas):
            datos = persona.to_list()
            for col, dato in enumerate(datos):
                entry = ttk.Entry(lista_frame, width=20)
                entry.grid(row=index, column=col, padx=5, pady=5)
                entry.insert(0, dato)

    # Función para limpiar las entradas
    def limpiar_entradas():
        nombre_entry.delete(0, tk.END)
        edad_entry.delete(0, tk.END)
        profesion_entry.delete(0, tk.END)
        experiencia_entry.delete(0, tk.END)
        sexo_combobox.set("")

    # Botón para procesar la información
    procesar_button = ttk.Button(entry_frame, text="Procesar Información", command=procesar_informacion)
    procesar_button.grid(row=5, column=0, columnspan=2, pady=10)

    root.mainloop()

# Crear la ventana de inicio de sesión
login_window = tk.Tk()
login_window.title("Inicio de Sesión")

# Etiquetas y entradas para el usuario y la contraseña
ttk.Label(login_window, text="Usuario:").grid(row=0, column=0, padx=10, pady=10)
usuario_entry = ttk.Entry(login_window)
usuario_entry.grid(row=0, column=1, padx=10, pady=10)

ttk.Label(login_window, text="Contraseña:").grid(row=1, column=0, padx=10, pady=10)
contraseña_entry = ttk.Entry(login_window, show="*")  # Asegúrate de que contraseña_entry esté definida aquí
contraseña_entry.grid(row=1, column=1, padx=10, pady=10)

# Botón para validar el inicio de sesión
login_button = ttk.Button(login_window, text="Iniciar Sesión", command=validar_login)
login_button.grid(row=2, column=0, columnspan=2, pady=20)

# Ejecutar el bucle principal de la ventana de inicio de sesión
login_window.mainloop()

