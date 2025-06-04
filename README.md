# proyectofinal
Ahora tenemos una version en donde el resumen de los datos registrados ya no se muestra en la pantalla principal sino lo agregue al menu de opciones 

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

    entrada_hora.delete(0, tk.END)

def mostrar_resumen():
    ventana_resumen = tk.Toplevel()
    ventana_resumen.title("Resumen de Asistencias")
    ventana_resumen.geometry("500x400")

    texto_resumen = tk.Text(ventana_resumen,  width=60)
    texto_resumen.pack(padx=10, pady=10)

    for nombre, datos in asistencia_registro.items():
        puesto = trabajadores[nombre]
        resumen = (
            f"{nombre} ({puesto}) - "
            f"Asistencias: {datos['Asistencias']}, "
            f"Faltas: {datos['Faltas']}, "
            f"Retardos: {datos['Retardos']}, "
            f"Vacaciones: {datos['Vacaciones']}\n"
        )
        texto_resumen.insert(tk.END, resumen)

ventana = tk.Tk()
ventana.title("Registro de Asistencias - Hospital")
ventana.geometry("700x400")

menu_frame = tk.Frame(ventana, width=200, bg="lightblue")
menu_frame.pack(side=tk.LEFT, fill=tk.Y)

tk.Label(menu_frame, text="Menú", bg="#e0e0e0", font=("Arial", 14)).pack(pady=10)

tk.Button(menu_frame, text="Registrar Asistencia", command=lambda: registrar("Asistencia"), width=20).pack(pady=5)
tk.Button(menu_frame, text="Registrar Falta", command=lambda: registrar("Falta"), width=20).pack(pady=5)
tk.Button(menu_frame, text="Registrar Vacaciones", command=lambda: registrar("Vacaciones"), width=20).pack(pady=5)
tk.Button(menu_frame, text="Ver Resumen de Asistencias", command=mostrar_resumen, width=20).pack(pady=20)

main_frame = tk.Frame(ventana)
main_frame.pack(side=tk.RIGHT, fill=tk.BOTH, expand=True, padx=10, pady=10)

tk.Label(main_frame, text="Selecciona un trabajador:").pack()
lista_trabajadores = tk.StringVar()
lista_trabajadores.set(list(trabajadores.keys())[0])
tk.OptionMenu(main_frame, lista_trabajadores, *trabajadores.keys()).pack(pady=5)

tk.Label(main_frame, text="Hora de llegada (HH:MM):").pack()
entrada_hora = tk.Entry(main_frame)
entrada_hora.pack(pady=5)

ventana.mainloop()



         
   


