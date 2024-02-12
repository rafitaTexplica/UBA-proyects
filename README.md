# UBA-proyects
Scripts dedicados a temas Actuariales
# El algoritmo le pide al usuario que defina los coeficientes de un polinomio de grado 2. 
# Luego pregunta por valores de x para devolverle su imágen, graficando los pares ordenados.
# Se calculan las raíces, derivadas, integrales y el extremo del polinomio. 
# Se muestra un boxplot de los valores de x seleccionados.

import numpy as np
import sympy as sp
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px

def display_ordered_pairs(x_values, y_values):
    for x, y in zip(x_values, y_values):
        print(f"Si x = {x} => f({x}) = {y}")

print("Determine los coeficientes del polinomio de grado 2:")
try:
  a = float(input("a: "))
  if a != 0:
      b = float(input("b: "))
      c = float(input("c: "))

      def f(x):
          return(a * x ** 2 + b * x + c)

      x = sp.symbols("x")

      x_values = []
      y_values = []

      while True:
          print("En qué punto de x desea evaluar la función? (Escriba 'stop' para terminar)")
          x_input = input("x: ")

          if x_input.lower() == "stop":
              break

          else:
              try:
                  x_value = float(x_input)
                  if x_value in x_values:
                    print("Ya ha ingresado el valor de x. Intente nuevamente.")
                  else:
                    y_value = f(x_value)
                    x_values.append(x_value)
                    y_values.append(y_value)
              except ValueError:
                  print("Entrada no válida. Escriba un número.")
              
      # Se muestran los pares ordenados seleccionados por el usuario.
      display_ordered_pairs(x_values, y_values)

      # Se grafican los valores seleccionados por el usuario.
      plt.scatter(x_values, y_values, color = "blue", label="f(x)")
      plt.xlabel('x')
      plt.ylabel('y')
      plt.title(f"f(x) = {a}x² + {b}x + {c}")
      plt.legend()
      plt.grid(True)
      plt.show()

      print("Algunos cálculos.")

      # Calculamos las raíces.
      discriminante = b**2 - 4*a*c

      if discriminante >0:
        root_1 = (-b + discriminante**0.5) / (2*a)
        root_2 = (-b - discriminante**0.5) / (2*a)
        print("Las raíces son: ", root_1, "y ", root_2)
      elif discriminante == 0: 
        single_root = -b / (2*a)
        print("La raíz doble es: ", single_root)
      else:  
        real_part = -b / (2*a)
        imag_part = (-discriminante)**0.5 / (2*a)
        print("Las raíces son imaginarias")
        print("Raíz 1: ", real_part, "+", imag_part, "i")
        print("Raíz 2: ", real_part, "-", imag_part, "i")

      # Calcumos la derivada.
      df = sp.diff(f(x),x)
      d2f = sp.diff(f(x),x,2)
      print("La derivada primera es:", df)
      print("La derivada segunda es:", d2f)

      # Calculamos la Integral.
      print("La integral es:", sp.integrate(f(x),x))

      # Calculamos el punto crítico. Determinamos que tipo de extremo es.
      p_critico = sp.solve(sp.Eq(df,0))
      for p in p_critico:
        if d2f.subs(x,p) < 0:
          print("La función tiene un máximo:","(",p,",",f(p),")")
        elif d2f.subs(x,p) > 0:
          print("La función tiene un mínimo:","(",p,",",f(p),")")
        else: 
          print("El criterio de la segunda derivada no decide el tipo de extremo.")


      # Plot de los input del usuario (boxplot).
      sns.set_theme(style="whitegrid")
      plt.figure(figsize=(8, 6))
      sns.boxplot(data=[x_values], width=0.5, palette="viridis")
      plt.ylabel('Distribución')
      plt.title("Box plot de los valores seleccionados de x")
      plt.show()

  else: 
      print("a debe ser distinto de cero.")

except ValueError:
  print("Entrada no válida. Los coeficientes deben ser números.")
