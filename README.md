# proyectofinal
aqui lo que hice fue combinar ambos codif¿gos ahora asi se pueden registrar retardos, asistencias, faltas y vvacaciones de los trabajadores del hospital en un mismo codigo c

import tkinter as tk
from tkinter import messagebox
from datetime import datetime, time

trabajadores = {
    "Luis": "Recepcionista",
    "Maria": "Doctora",
    "Pedro": "Camillero",
    "Ana": "Enfermera",
    "Carlos": "Cirujano"
}

asistencia_registro = {
    nombre: {
        "Asistencias": 0,
        "Faltas": 0,
        "Retardos": 0,
        "Vacaciones": 0
    }
    for nombre in trabajadores
}

hora_entrada_estandar = time(8, 0)

def registrar(tipo):
    nombre = lista_trabajadores.get()
    if not nombre:
        messagebox.showwarning("Advertencia", "Selecciona un trabajador.")
        return

    if tipo == "Asistencia":
        hora_llegada_str = entrada_hora.get()
        if not hora_llegada_str:
            messagebox.showwarning("Advertencia", "Ingresa la hora de llegada (formato HH:MM).")
            return
        try:
            hora_llegada = datetime.strptime(hora_llegada_str, "%H:%M").time()
        except ValueError:
            messagebox.showerror("Error", "Formato de hora inválido. Usa HH:MM (24 horas).")
            return

        minutos_tarde = (
            datetime.combine(datetime.today(), hora_llegada) -
            datetime.combine(datetime.today(), hora_entrada_estandar)
        ).total_seconds() / 60

        if minutos_tarde > 10:
            asistencia_registro[nombre]["Retardos"] += 1
            messagebox.showinfo("Retardo", f"{nombre} llegó con {int(minutos_tarde)} minutos de retraso.")
        else:
            asistencia_registro[nombre]["Asistencias"] += 1
            messagebox.showinfo("Asistencia", f"Asistencia registrada para {nombre}.")
    
    elif tipo == "Falta":
        asistencia_registro[nombre]["Faltas"] += 1
        messagebox.showinfo("Falta", f"Falta registrada para {nombre}.")

    elif tipo == "Vacaciones":
        asistencia_registro[nombre]["Vacaciones"] += 1
        messagebox.showinfo("Vacaciones", f"{nombre} está de vacaciones hoy.")

    actualizar_datos()

def actualizar_datos():
    texto_resultado.delete(1.0, tk.END)
    for nombre, datos in asistencia_registro.items():
        puesto = trabajadores[nombre]
        resumen = (
            f"{nombre} ({puesto}) - "
            f"Asistencias: {datos['Asistencias']}, "
            f"Faltas: {datos['Faltas']}, "
            f"Retardos: {datos['Retardos']}, "
            f"Vacaciones: {datos['Vacaciones']}\n"
        )
        texto_resultado.insert(tk.END, resumen)

ventana = tk.Tk()
ventana.title("Registro de Asistencias - Hospital")
ventana.geometry("500x500")

tk.Label(ventana, text="Selecciona un trabajador:").pack()
lista_trabajadores = tk.StringVar(ventana)
lista_trabajadores.set(list(trabajadores.keys())[0])
tk.OptionMenu(ventana, lista_trabajadores, *trabajadores.keys()).pack(pady=5)

tk.Label(ventana, text="Hora de llegada (HH:MM):").pack()
entrada_hora = tk.Entry(ventana)
entrada_hora.pack(pady=5)

tk.Button(ventana, text="Registrar Asistencia", command=lambda: registrar("Asistencia")).pack(pady=2)
tk.Button(ventana, text="Registrar Retardo o Falta", command=lambda: registrar("Falta")).pack(pady=2)
tk.Button(ventana, text="Registrar Vacaciones", command=lambda: registrar("Vacaciones")).pack(pady=2)

tk.Label(ventana, text="Resumen general:").pack(pady=5)
texto_resultado = tk.Text(ventana, height=15, width=60)
texto_resultado.pack()

actualizar_datos()

ventana.mainloop()




