# proyectofinal
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
    nombre: {"Asistencias": 0, "Faltas": 0, "Retardos": 0}
    for nombre in trabajadores
}

hora_entrada_estandar = time(8, 0)  

def registrar(asistencia=True):
    nombre = lista_trabajadores.get()
    if not nombre:
        messagebox.showwarning("Advertencia", "Selecciona un trabajador.")
        return

    if asistencia:
        hora_llegada_str = entrada_hora.get()
        if not hora_llegada_str:
            messagebox.showwarning("Advertencia", "Ingresa la hora de llegada (formato HH:MM).")
            return
        try:
            hora_llegada = datetime.strptime(hora_llegada_str, "%H:%M").time()
        except ValueError:
            messagebox.showerror("Error", "Formato de hora inválido. Usa HH:MM (24 horas).")
            return

        diferencia = (
            datetime.combine(datetime.today(), hora_llegada) -
            datetime.combine(datetime.today(), hora_entrada_estandar)
        ).total_seconds() / 60

        if diferencia > 10:
            asistencia_registro[nombre]["Retardos"] += 1
            messagebox.showinfo("Retardo", f"{nombre} llegó con {int(diferencia)} minutos de retraso.")
        else:
            asistencia_registro[nombre]["Asistencias"] += 1
            messagebox.showinfo("Asistencia", f"Asistencia registrada para {nombre}.")
    else:
        asistencia_registro[nombre]["Faltas"] += 1
        messagebox.showinfo("Falta", f"Falta registrada para {nombre}.")

    actualizar_datos()

def actualizar_datos():
    texto_resultado.delete(1.0, tk.END)
    for nombre, datos in asistencia_registro.items():
        puesto = trabajadores[nombre]
        asistencias = datos["Asistencias"]
        faltas = datos["Faltas"]
        retardos = datos["Retardos"]
        texto_resultado.insert(
            tk.END,
            f"{nombre} ({puesto}) - Asistencias: {asistencias}, Faltas: {faltas}, Retardos: {retardos}\n"
        )

ventana = tk.Tk()
ventana.title("Registro de Asistencias - Hospital")
ventana.geometry("400x400")

tk.Label(ventana, text="Selecciona un trabajador:").pack()
lista_trabajadores = tk.StringVar(ventana)
lista_trabajadores.set(list(trabajadores.keys())[0])
menu_desplegable = tk.OptionMenu(ventana, lista_trabajadores, *trabajadores.keys())
menu_desplegable.pack(pady=5)

tk.Label(ventana, text="Hora de llegada (HH:MM):").pack()
entrada_hora = tk.Entry(ventana)
entrada_hora.pack(pady=5)

tk.Button(ventana, text="Registrar Asistencia", command=lambda: registrar(True)).pack(pady=2)
tk.Button(ventana, text="Registrar Falta", command=lambda: registrar(False)).pack(pady=2)

tk.Label(ventana, text="Resumen de Asistencias, Faltas y Retardos:").pack(pady=5)
texto_resultado = tk.Text(ventana, height=10, width=50)
texto_resultado.pack()

actualizar_datos()

ventana.mainloop()



