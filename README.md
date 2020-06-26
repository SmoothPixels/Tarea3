
---

# Universidad de Costa Rica

### Facultad de Ingeniería

### Escuela de Ingeniería Eléctrica

#### IE0405 - Modelos Probabilísticos de Señales y Sistemas

---
---

# Tarea 03: Frecuencia relativa de dos variables aleatorias

*Realizado por:* **Jean Carlos Alvarado Brenes**

## 1) (25 %) A partir de los datos, encontrar la mejor curva de ajuste (modelo probabilístico) para las funciones de densidad marginales de X y Y



#Se importan las librerías a utilizar
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

#Leer el archivo
file=pd.read_csv("xy2.csv")

xvects=np.linspace(5,15,11)

yvects=np.linspace(5,25,21)

sumvecty=np.sum(file, axis=0)
sumvectx=np.sum(file, axis=1)


plt.plot(xvects,sumvectx)
plt.grid()
plt.title('Curva de ajuste para la función de densidad marginal de X')
plt.ylabel('Probabilidad')
plt.xlabel('Valor de X')
plt.show()

plt.plot(yvects,sumvecty)
plt.title('Curva de ajuste para la función de densidad marginal de Y')
plt.ylabel('Probabilidad')
plt.xlabel('Valor de Y')
plt.grid()
plt.show()


from scipy.optimize import curve_fit

def gaussiana(x, mu, sigma):
    return 1/(np.sqrt(2*np.pi*sigma**2))*np.exp(-(x-mu)**2/(2*sigma**2))


param1, _ = curve_fit(gaussiana, sumvectx ,xvects)
print("Parámetros de ajuste Gaussiana para X= ",param1)


param2, _ = curve_fit(gaussiana, sumvecty ,yvects)
print("Parámetros de ajuste Gaussiana para Y = ",param2)

'''
2. (25 %) Asumir independencia de X y Y. Analíticamente, 
¿cuál es entonces la expresión de la función de densidad 
conjunta que modela los datos?
'''

#presentacion #9 revisar, independencia estadistica, ec (23)


'''
3. (25 %) Hallar los valores de correlación, 
covarianza y coeficiente de correlación (Pearson) 
para los datos y explicar su significado.
'''
#Correlación
#5*5*p+6*5*p+7*5*p...
#Leer el archivo
file2=pd.read_csv("xyp.csv")
#Realizar las multiplicaciones para encontrar la correlacion (faltaria la suma)
file2['resultado1'] = file2['x']*file2['y']*file2['p']
print(file2['resultado1'] )
#Hacer la suma 
correlacion=np.sum(file2['resultado1'] , axis=0)
print("La correlación es = ",correlacion)

#Covarianza
#se encuentra la media
mediax=np.mean(file2['x'])
mediay=np.mean(file2['y'])
print("Media de x =",mediax)
print("Media de y =",mediay)
#se realiza la fórmula
file2['resultado2'] = (file2['x']-mediax)*(file2['y']-mediay)*file2['p']

#Hacer la suma 
covarianza=np.sum(file2['resultado2'] , axis=0)
print("La covarianza es = ",covarianza)



#Coeficiente de correlación
#Calcular la varianza de x y y
varianzax=np.var(file2['x'])
varianzay=np.var(file2['y'])
#Calcular la "multiplicacion"
file2['resultado3'] = (file2['resultado2'])/(varianzax*varianzay)
#Hacer la suma
coefcorre=np.sum(file2['resultado3'] , axis=0)
#imprimir el resultado
print('El coeficiente de correlación es =', coefcorre)



