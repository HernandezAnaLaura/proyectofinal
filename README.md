# proyectofinal
aqui agregue que las vacaciones ya esten registradas los dias que tenga vacaciones cada trabajador y en vez de estar el boton de registrar vacaciones ahora hay uno de ver los dias de vacaciones de cada trabjador para que asi ellos puedan ver cuales son las vacaciones que le corresponden 

import tkinter as tk
from tkinter import messagebox
from datetime import datetime, time

trabajadores = {
    "Luis": {"puesto": "Recepcionista", "turno": "Matutino"},
    "Maria": {"puesto": "Doctora", "turno": "Vespertino"},
    "Pedro": {"puesto": "Camillero", "turno": "Nocturno"},
    "Ana": {"puesto": "Enfermera", "turno": "Matutino"},
    "Carlos": {"puesto": "Cirujano", "turno": "Vespertino"}
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

vacaciones_previas = {
    "Luis": ["2025-01-10", "2025-03-15"],
    "Maria": ["2025-02-20"],
    "Pedro": [],
    "Ana": ["2025-04-05"],
    "Carlos": ["2025-05-12", "2025-05-13"]
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

    entrada_hora.delete(0, tk.END)

def mostrar_resumen():
    ventana_resumen = tk.Toplevel()
    ventana_resumen.title("Resumen de Asistencias")
    ventana_resumen.geometry("550x450")

    texto_resumen = tk.Text(ventana_resumen, height=25, width=70)
    texto_resumen.pack(padx=10, pady=10)

    for nombre, datos in asistencia_registro.items():
        puesto = trabajadores[nombre]["puesto"]
        turno = trabajadores[nombre]["turno"]
        resumen = (
            f"{nombre} ({puesto} | Turno: {turno})\n"
            f"  Asistencias: {datos['Asistencias']}, Faltas: {datos['Faltas']}, "
            f"Retardos: {datos['Retardos']}, Vacaciones (totales): {datos['Vacaciones']}\n\n"
        )
        texto_resumen.insert(tk.END, resumen)

def mostrar_vacaciones():
    ventana_vacaciones = tk.Toplevel()
    ventana_vacaciones.title("Vacaciones Registradas")
    ventana_vacaciones.geometry("500x400")

    texto = tk.Text(ventana_vacaciones, height=25, width=60)
    texto.pack(padx=10, pady=10)

    for nombre, fechas in vacaciones_previas.items():
        turno = trabajadores[nombre]["turno"]
        puesto = trabajadores[nombre]["puesto"]
        if fechas:
            texto.insert(tk.END, f"{nombre} ({puesto}, Turno: {turno}) - Vacaciones: {', '.join(fechas)}\n")
        else:
            texto.insert(tk.END, f"{nombre} ({puesto}, Turno: {turno}) - Vacaciones: Ninguna\n")

ventana = tk.Tk()
ventana.title("Registro de Asistencias - Hospital")
ventana.geometry("700x400")

menu_frame = tk.Frame(ventana, width=200, bg="lightblue")
menu_frame.pack(side=tk.LEFT, fill=tk.Y)

tk.Label(menu_frame, text="Menú",  font=("Arial", 14)).pack(pady=10)

tk.Button(menu_frame, text="Registrar Asistencia", command=lambda: registrar("Asistencia"), width=20).pack(pady=5)
tk.Button(menu_frame, text="Registrar Falta", command=lambda: registrar("Falta"), width=20).pack(pady=5)
tk.Button(menu_frame, text="Ver Resumen de Asistencias", command=mostrar_resumen, width=20).pack(pady=10)
tk.Button(menu_frame, text="Ver Vacaciones de Trabajadores", command=mostrar_vacaciones, width=25).pack(pady=10)

main_frame = tk.Frame(ventana)
main_frame.pack(side=tk.RIGHT, fill=tk.BOTH, expand=True, padx=10, pady=10)

tk.Label(main_frame, text="Selecciona un trabajador:").pack()
lista_trabajadores = tk.StringVar()
lista_trabajadores.set(list(trabajadores.keys())[0])
tk.OptionMenu(main_frame, lista_trabajadores, *trabajadores.keys()).pack(pady=5)

tk.Label(main_frame, text="Hora de llegada ").pack()
entrada_hora = tk.Entry(main_frame)
entrada_hora.pack(pady=5)

ventana.mainloop()


         
   


