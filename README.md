# proyectofinal
PERIODO VACACIONAL 
import tkinter as tk
from tkinter import messagebox

trabajadores = {
    "Luis": {"vacaciones": "20/12-02/01", "asistencia": []},
    "María": {"vacaciones": "14/08-28/08", "asistencia": []},
    "Pedro": {"vacaciones": "01/06-15/06", "asistencia": []},
    "Ana": {"vacaciones": "10/09-24/09", "asistencia": []},
    "Carlos": {"vacaciones": "05/11-19/11", "asistencia": []}
}

def mostrar_vacaciones():
    trabajador = entrada_nombre.get()
    if trabajador in trabajadores:
        periodo = trabajadores[trabajador]["vacaciones"]
        messagebox.showinfo("Periodo Vacacional", f"{trabajador}: {periodo}")
    else:
        messagebox.showwarning("Trabajador no encontrado", "El trabajador no está registrado.")

def registrar_asistencia():
    trabajador = entrada_nombre.get()
    if trabajador in trabajadores:
        trabajadores[trabajador]["asistencia"].append("Presente")
        messagebox.showinfo("Asistencia registrada", f"Asistencia de {trabajador} registrada.")
    else:
        messagebox.showwarning("Trabajador no encontrado", "El trabajador no está registrado."

ventana = tk.Tk()
ventana.title("Registro de Asistencia")

etiqueta_nombre = tk.Label(ventana, text="Nombre del trabajador:")
etiqueta_nombre.grid(row=0, column=0, padx=10, pady=10)
entrada_nombre = tk.Entry(ventana)
entrada_nombre.grid(row=0, column=1, padx=10, pady=10)

boton_vacaciones = tk.Button(ventana, text="Ver periodo vacacional", command=mostrar_vacaciones)
boton_vacaciones.grid(row=1, column=0, padx=10, pady=10)
boton_asistencia = tk.Button(ventana, text="Registrar asistencia", command=registrar_asistencia)
boton_asistencia.grid(row=1, column=1, padx=10, pady=10)


ventana.mainloop()
