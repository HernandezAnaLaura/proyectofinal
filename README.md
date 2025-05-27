# proyectofinal
REGISTRO DE FALTAS Y DE ASISTENCIAS MOSTRANDO EL PUESTO DE TRABAJO DE CADA PERSONA 

import tkinter as tk
from tkinter import messagebox

trabajadores = {
    "Luis": "recepcionista",
    "Maria": "Doctora",
    "Pedro": "camillero",
    "Ana": "enfermera",
    "Carlos": "cirujano"
}

asistencia_registro = {nombre: {"Asistencias": 0, "Faltas": 0} for nombre in trabajadores}

def registrar(asistencia=True):
    nombre = lista_trabajadores.get()
    if not nombre:
        messagebox.showwarning("Advertencia", "Selecciona un trabajador.")
        return
    
    if asistencia:
        asistencia_registro[nombre]["Asistencias"] += 1
    else:
        asistencia_registro[nombre]["Faltas"] += 1

    actualizar_datos()
    messagebox.showinfo("Registrado", f"{'Asistencia' if asistencia else 'Falta'} registrada para {nombre}.")

def actualizar_datos():
    texto_resultado.delete(1.0, tk.END)
    for nombre, datos in asistencia_registro.items():
        puesto = trabajadores[nombre]
        asistencias = datos["Asistencias"]
        faltas = datos["Faltas"]
        texto_resultado.insert(tk.END, f"{nombre} ({puesto}) - Asistencias: {asistencias}, Faltas: {faltas}\n")


ventana = tk.Tk()
ventana.title("Registro de Asistencias - Hospital")


tk.Label(ventana, text="Selecciona un trabajador:").pack()

lista_trabajadores = tk.StringVar(ventana)
lista_trabajadores.set(list(trabajadores.keys())[0])  

menu_desplegable = tk.OptionMenu(ventana, lista_trabajadores, *trabajadores.keys())
menu_desplegable.pack(pady=5)

tk.Button(ventana, text="Registrar Asistencia", command=lambda: registrar(True)).pack(pady=2)
tk.Button(ventana, text="Registrar Falta", command=lambda: registrar(False)).pack(pady=2)

tk.Label(ventana, text="Resumen de Asistencias y Faltas:").pack(pady=5)
texto_resultado = tk.Text(ventana, height=10, width=50)
texto_resultado.pack()

actualizar_datos()

ventana.mainloop()

