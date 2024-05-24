# Capacitor_ne555
Proyecto capacimetro, Fisica II
Codigo Arduino 1
//variables
float rango=2,ciclo=0,cambiodeciclo=0,valledetension=1023;
float ultimamedicion,picodetension,contadorvisualizacion,contadorciclo;
float capacitor,frecuencia;

//comunicacion con el puerto
void setup() {
  Serial.begin(9600);
  pinMode(A0, INPUT);
  //Serial.begin(9600);
  
}

//funcion para calcular la capacitancia
void loop() {
     long sensorValue= analogRead(A0);
     if(micros()>contadorvisualizacion+1000000)
     {
      int frecuencia = contadorciclo;
      capacitor=4,8/(frecuencia);
      int ct=capacitor;
      Serial.println(ct);
      delay(500);
      rango=(2+((picodetension-valledetension)/5)), contadorvisualizacion=micros();
      picodetension=sensorValue, valledetension=sensorValue, contadorciclo=0;
     }

   if (sensorValue>=(ultimamedicion+rango)){ ultimamedicion = sensorValue, ciclo=1;
   if (sensorValue>picodetension){picodetension=sensorValue;}}
   if (sensorValue<= (ultimamedicion-rango)){ultimamedicion =sensorValue, ciclo=0;
   if (sensorValue<valledetension){valledetension=sensorValue;}}
   if (ciclo==1 && cambiodeciclo==0){cambiodeciclo=1, contadorciclo++;}
   if (ciclo==0 && cambiodeciclo==1){cambiodeciclo=0;}

}

*************************
*************************
*************************
*************************
CODIGO PYTHON

from logging import root
from tkinter import *
import tkinter as tk
import tkinter
import serial

def MostrarCapacitancias():
    global valorcapacitanciaP
    global valorcapacitanciaN
    global valorcapacitanciaM
    global boton1
    global boton2
    global boton3

    arduino = serial.Serial("COM9", 9600)
    capacitancia = arduino.readline().decode().strip()
    capacitancia1 = float(capacitancia)
    Nanofaradios = capacitancia1 * 1000
    Microfaradios = Nanofaradios * 1000
    valorcapacitanciaP.set(capacitancia1)
    boton1.config(text=f"{capacitancia1:.2f}", bg="LightCyan2")
    valorcapacitanciaN.set(Nanofaradios)
    boton2.config(text=f"{Nanofaradios:.2f}", bg="LightCyan2")
    valorcapacitanciaM.set(Microfaradios)
    boton3.config(text=f"{Microfaradios:.2f}", bg="LightCyan2")

def NuevanVentana():
    global valorcapacitanciaP
    global valorcapacitanciaN
    global valorcapacitanciaM
    global boton1
    global boton2
    global boton3
    
    ventana2 = tk.Toplevel()
    ventana2.geometry("800x600")
    ventana2.title("Capacimetro 2.0")
    ventana2.config(bg="gray25")
    ventana2.iconbitmap("capacitor.ico")
  

    #img=tkinter.PhotoImage(file="capacitor2.png")
    ventana2.resizable(0,0)
    label14 = Label(ventana2, text="valores en unidades de: ", font=("Daily Chanllenge DEMO", 30), bg="gray86")
    label14.pack(pady=20)
    label11 = Label(ventana2, text="Picofaradios: ", font=("Daily Chanllenge DEMO", 30), bg="gray86")
    label11.pack()
    boton1 = Button(ventana2, text="", font=("212 Orion Sans", 20))
    boton1.pack()
    label12 = Label(ventana2, text="Nanofaradios", font=("212 Orion Sans", 20))
    label12.pack()
    boton2 = Button(ventana2, text="", font=("212 Orion Sans", 20))
    boton2.pack()
    label13 = Label(ventana2, text="Microfaradios", font=("212 Orion Sans", 20))
    label13.pack()
    boton3 = Button(ventana2, text="", font=("212 Orion Sans", 20))
    boton3.pack()
    boton_salir = Button(ventana2, text="Salir", font=("212 Orion Sans", 25), bg="indian red", command=ventana2.destroy)  
    boton_salir.place(x=490, y=480)
    custom=("Daily Chanllenge DEMO",12)
    boton22 = Button(ventana2, text="Mostrar Valores", font=("Daily Chanllenge DEMO",12), bg="indian red", command=MostrarCapacitancias)
    boton22.place(x=140, y=480)

# Crear la ventana principal
ventana = tk.Tk()
ventana.geometry("1200x600")
ventana.title("Capacimetro")
ventana.resizable(0,0)
ventana.config(bg="gray25")
ventana.iconbitmap("capacitor.ico")

# Crear una variable de tipo DoubleVar para almacenar la capacitancia
valorcapacitanciaP = DoubleVar()
valorcapacitanciaN = DoubleVar()
valorcapacitanciaM = DoubleVar()

# Crear widgets en la ventana principal
label = Label(ventana, text="CAPACIMETRO", font=("Daily Chanllenge DEMO", 40))
label.place(x=420, y=20)

boton = Button(ventana, text="Calcular Capacitancias", font=("212 Orion Sans", 25), bg="pale turquoise", command=NuevanVentana)
boton.place(x=450, y=100)

# Iniciar el bucle principal de la interfaz gráfica
ventana.mainloop()
