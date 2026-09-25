# Resolución numérica del oscilador armónico cuántico

Este proyecto calcula los niveles de energía y las funciones de onda del **oscilador armónico cuántico unidimensional** mediante diferencias finitas y diagonalización matricial.

El código se encuentra en el notebook `osciladorarmonico.ipynb` y compara los resultados numéricos con la solución analítica conocida.

## Descripción

El oscilador armónico cuántico es uno de los sistemas fundamentales de la mecánica cuántica. Su Hamiltoniano es:

$$
\hat H=-\frac{\hbar^2}{2m}\frac{d^2}{dx^2}+\frac{1}{2}m\omega^2x^2
$$

La ecuación de Schrödinger estacionaria viene dada por:

$$
\hat H\psi_n(x)=E_n\psi_n(x)
$$

Para resolverla numéricamente, el espacio continuo se sustituye por una malla finita. De esta forma, el Hamiltoniano se convierte en una matriz cuyos autovalores son las energías y cuyos autovectores representan las funciones de onda.

## Método utilizado

El procedimiento seguido es:

1. Discretizar el intervalo espacial $[-5,5]$ en 500 puntos.
2. Construir la matriz del potencial armónico.
3. Aproximar la segunda derivada mediante diferencias finitas centradas.
4. Construir la matriz de energía cinética.
5. Sumar las matrices cinética y potencial para obtener el Hamiltoniano.
6. Diagonalizar el Hamiltoniano con `scipy.linalg.eigh`.
7. Comparar las energías obtenidas con la solución teórica.
8. Representar gráficamente los niveles de energía y las funciones de onda.

## Diferencias finitas

La segunda derivada de la función de onda se aproxima como:

$$
\frac{d^2\psi}{dx^2}\bigg|_{x_i}
\approx
\frac{\psi_{i+1}-2\psi_i+\psi_{i-1}}{\Delta x^2}
$$

Esta expresión genera una matriz tridiagonal con $-2$ en la diagonal principal y $1$ en las diagonales superior e inferior:

$$
D_2=\frac{1}{\Delta x^2}
\begin{pmatrix}
-2&1&0&\cdots&0\\
1&-2&1&\ddots&\vdots\\
0&1&-2&\ddots&0\\
\vdots&\ddots&\ddots&\ddots&1\\
0&\cdots&0&1&-2
\end{pmatrix}
$$

A partir de ella se construye la energía cinética:

$$
T=-\frac{\hbar^2}{2m}D_2
$$

El potencial se representa mediante una matriz diagonal:

$$
V_{ii}=\frac{1}{2}m\omega^2x_i^2
$$

Finalmente:

$$
H=T+V
$$

## Parámetros de la simulación

| Parámetro | Valor |
|---|---:|
| $\hbar$ | 1 |
| Masa $m$ | 1 |
| Frecuencia $\omega$ | 2 |
| Puntos de la malla $N$ | 500 |
| Intervalo espacial | $[-5,5]$ |
| Niveles principales estudiados | $n=0,1,2,3,4,5$ |

Se utilizan unidades naturales. Con estos parámetros, las energías analíticas son:

$$
E_n=\hbar\omega\left(n+\frac{1}{2}\right)=2n+1
$$

## Resultados

| Nivel $n$ | Energía numérica | Energía teórica |
|---:|---:|---:|
| 0 | 0.9999 | 1.0000 |
| 1 | 2.9997 | 3.0000 |
| 2 | 4.9993 | 5.0000 |
| 3 | 6.9987 | 7.0000 |
| 4 | 8.9979 | 9.0000 |
| 5 | 10.9969 | 11.0000 |

Para el nivel $n=5$, el notebook obtiene un error absoluto aproximado de `0.00306`, equivalente a un error relativo del `0.02785 %`.

La proximidad entre los resultados numéricos y analíticos indica que la discretización reproduce correctamente los primeros niveles del sistema.

## Gráficas generadas

El notebook produce:

- Una comparación entre las energías numéricas y las analíticas.
- Una gráfica individual de cada función de onda entre $n=0$ y $n=5$.
- Una gráfica conjunta de los estados impares $n=1,3,5,7,9$.

Las funciones de onda alternan su paridad:

- Si $n$ es par, $\psi_n(x)$ es par.
- Si $n$ es impar, $\psi_n(x)$ es impar.

Además, el estado $n$ tiene $n$ nodos. El signo global de los autovectores puede aparecer invertido sin alterar el significado físico del resultado.

## Requisitos

- Python 3.
- NumPy.
- SciPy.
- Matplotlib.
- Jupyter Notebook o JupyterLab.

Instalación:

```bash
python -m pip install numpy scipy matplotlib notebook
```

## Ejecución

Inicia Jupyter Notebook desde la carpeta del proyecto:

```bash
jupyter notebook
```

Después, abre `osciladorarmonico.ipynb` y ejecuta la celda.

El notebook también puede ejecutarse desde Visual Studio Code con las extensiones de Python y Jupyter.

## Estructura del proyecto

```text
.
├── osciladorarmonico.ipynb
└── README.md
```

## Aspectos que se pueden modificar

El código permite cambiar:

- La masa de la partícula.
- La frecuencia del oscilador.
- El número de puntos de la malla.
- Los límites del intervalo espacial.
- Los niveles de energía representados.

Al estudiar niveles más altos puede ser necesario aumentar el tamaño del intervalo, ya que sus funciones de onda se extienden más en el espacio.

## Limitaciones

- El dominio espacial es finito, aunque el problema físico está definido en toda la recta real.
- El error depende del tamaño de la malla y del intervalo elegido.
- El error mostrado por el programa corresponde únicamente al último nivel del bucle, $n=5$.
- Los autovectores están normalizados como vectores discretos; una normalización continua rigurosa debe incluir $\Delta x$.
- Se diagonaliza el Hamiltoniano completo aunque solo se utilizan sus primeros autovalores.

## Posibles ampliaciones

- Calcular el error de cada nivel por separado.
- Representar la densidad de probabilidad $|\psi_n(x)|^2$.
- Añadir el potencial y las funciones de onda sobre un mismo gráfico.
- Estudiar la convergencia al variar $N$.
- Comprobar la normalización y ortogonalidad de las funciones de onda.
- Utilizar matrices dispersas para mejorar la eficiencia.

## Licencia

Este proyecto tiene finalidad educativa. No se ha especificado una licencia de distribución.
