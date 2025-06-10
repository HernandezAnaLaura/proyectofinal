# proyectofinal
AQUI AGREGUE UNA CLASE PADFRE Y UNA CLASE HIJA AL IGUAL QUE ASIGNE DIFERENTES HORARIOS DE ENTRADA PARA LOS TRABAJADORES EN SUS DIFERENTES TURNOS

import tkinter as tk
from tkinter import messagebox
from datetime import datetime, time, timedelta

class RegistroAsistencia:
    hora_entrada_estandar = {
        "Matutino": time(8, 0),
        "Vespertino": time(15, 0),
        "Nocturno": time(22, 0)
    }
    max_tolerancia = 10
    def __init__(self, nombre, puesto, turno):
        self.nombre = nombre
        self.puesto = puesto
        self.turno = turno
        self.asistencias = self.faltas = self.retardos = self.vacaciones = 0

    def procesar_llegada(self, hora_str):
        try:
            hora_llegada = datetime.strptime(hora_str, "%H:%M").time()
        except ValueError:
            raise ValueError("Formato de hora inválido. Usa HH:MM (24 h).")

        std = self.hora_entrada_estandar[self.turno]
        delta = (datetime.combine(datetime.today(), hora_llegada) -
                 datetime.combine(datetime.today(), std))
        minutos_tarde = delta.total_seconds() / 60

        if minutos_tarde > self.max_tolerancia:
            self.retardos += 1
            return f"Retardo: llegó {int(minutos_tarde)} min tarde (entrada estándar {std})."
        else:
            self.asistencias += 1
            return "Asistencia registrada correctamente."

    def registrar_falta(self):
        self.faltas += 1
        return "Falta registrada."

class AppGUI:
    def __init__(self, master):
        self.master = master
        master.title("Registro Asistencias - Hospital")

        trabajadores = {
            "Luis": ("Recepcionista", "Matutino"),
            "Maria": ("Doctora", "Vespertino"),
            "Pedro": ("Camillero", "Nocturno"),
            "Ana": ("Enfermera", "Matutino"),
            "Carlos": ("Cirujano", "Vespertino")
        }
        self.empleados = {
            n: RegistroAsistencia(n, p, t) for n,(p,t) in trabajadores.items()
        }
        self.vac_previas = {
            "Luis": ["2025-01-10", "2025-03-15"],
            "Maria": ["2025-02-20"],
            "Pedro": [], "Ana": ["2025-04-05"],
            "Carlos": ["2025-05-12", "2025-05-13"]
        }

        self.setup_ui()

    def setup_ui(self):
        frame = tk.Frame(self.master)
        frame.pack(padx=10, pady=10)

        tk.Label(frame, text="Trabajador:").grid(row=0, column=0, sticky="w")
        self.var_nombre = tk.StringVar(value=list(self.empleados.keys())[0])
        tk.OptionMenu(frame, self.var_nombre, *self.empleados).grid(row=0, column=1)

        tk.Label(frame, text="Hora de llegada (HH:MM):").grid(row=1, column=0, sticky="w")
        self.entry_hora = tk.Entry(frame)
        self.entry_hora.grid(row=1, column=1)

        tk.Button(frame, text="Registrar Asistencia", command=self.on_asistencia).grid(row=2, column=0, pady=5)
        tk.Button(frame, text="Registrar Falta", command=self.on_falta).grid(row=2, column=1, pady=5)
        tk.Button(frame, text="Resumen", command=self.mostrar_resumen).grid(row=3, column=0, columnspan=2, pady=5)

    def on_asistencia(self):
        nombre = self.var_nombre.get()
        hora = self.entry_hora.get().strip()
        emp = self.empleados[nombre]
        msg = ""
        try:
            msg = emp.procesar_llegada(hora)
        except ValueError as e:
            messagebox.showerror("Error", str(e))
            return
        messagebox.showinfo("Asistencia", f"{nombre}: {msg}")
        self.entry_hora.delete(0, tk.END)

    def on_falta(self):
        nombre = self.var_nombre.get()
        msg = self.empleados[nombre].registrar_falta()
        messagebox.showinfo("Falta", f"{nombre}: {msg}")

    def mostrar_resumen(self):
        win = tk.Toplevel(self.master)
        win.title("Resumen")
        txt = tk.Text(win, width=60, height=20)
        txt.pack(padx=10, pady=10)
        for emp in self.empleados.values():
            vac = self.vac_previas.get(emp.nombre, [])
            line = (f"{emp.nombre} ({emp.puesto}, Turno {emp.turno}) → "
                    f"Asist: {emp.asistencias}, Faltas: {emp.faltas}, Retardos: {emp.retardos}, "
                    f"Vacac previas: {', '.join(vac) or 'Ninguna'}\n")
            txt.insert(tk.END, line)

if __name__ == "__main__":
    root = tk.Tk()
    AppGUI(root)
    root.mainloop()

         
   


