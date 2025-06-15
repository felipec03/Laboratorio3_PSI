# Laboratorio 3: Difusión Anisotrópica - Procesamiento de Señales e Imágenes

> Este repositorio contiene la implementación del algoritmo de **difusión anisotrópica de Perona-Malik** para el suavizado de imágenes digitales. El objetivo principal es reducir el ruido en una imagen mientras se preservan los bordes y detalles importantes, a diferencia de los filtros de suavizado isotrópicos (como el Gaussiano) que tienden a difuminar todo por igual.

> El proyecto se desarrolla en Python utilizando un Jupyter Notebook (`anisotropic.ipynb`) y bibliotecas estándar de procesamiento de imágenes como NumPy, Matplotlib y Scikit-image.

## ¿Qué es la Difusión Anisotrópica?

La difusión anisotrópica es un proceso de suavizado de imágenes que se basa en la idea de la ecuación del calor, pero con una conductividad (o coeficiente de difusión) que varía espacialmente. Esto permite que el suavizado sea más intenso en regiones homogéneas y menos intenso cerca de los bordes o discontinuidades de la imagen.

El modelo de Perona-Malik propone una ecuación de difusión donde el coeficiente de difusión $c(x,y,t)$ depende de la magnitud del gradiente de la imagen $|\nabla I|$:

$$
\frac{\partial I}{\partial t} = \text{div}(c(|\nabla I|) \nabla I)
$$

Donde:
- $I$ es la intensidad de la imagen.
- $t$ es el tiempo de difusión (iteraciones).
- $\text{div}$ es el operador de divergencia.
- $\nabla I$ es el gradiente de la imagen.
- $c(|\nabla I|)$ es la función de difusión, que controla la "velocidad" del suavizado.

## Funciones de Difusión Implementadas

En este laboratorio, se implementan tres funciones de difusión $g(|x|)$ (donde $|x|$ representa la magnitud del gradiente $|\nabla I|$ y $K$ es un parámetro de umbral):

1.  **Tipo 1:** $g(|x|) = e^{\left(\frac{-|x|^2}{K^2}\right)}$
    *   Favorece regiones de alto contraste (bordes) al disminuir rápidamente la difusión a medida que aumenta el gradiente.

2.  **Tipo 2:** $g(|x|) = \frac{1}{1 + \frac{|x|^2}{K^2}}$
    *   También preserva bordes, con una caída menos pronunciada que el Tipo 1.

3.  **Tipo 3:** $g(|x|) = \frac{1}{\sqrt{1 + \frac{|x|^2}{K^2}}}$
    *   Ofrece un compromiso entre los dos anteriores, siendo robusto a diferentes niveles de ruido.

El parámetro $K$ es crucial:
- Si $|\nabla I| \ll K$, entonces $c(|\nabla I|) \approx 1$ (difusión máxima, como en regiones homogéneas).
- Si $|\nabla I| \gg K$, entonces $c(|\nabla I|) \approx 0$ (difusión mínima, preservando bordes).

## Implementación Principal (`anisotropic.ipynb`)

El notebook `anisotropic.ipynb` contiene:

1.  **Carga de Imágenes:**
    *   Función `load_image(img_name)` para cargar imágenes desde el directorio `images/` y convertirlas a escala de grises.
2.  **Funciones de Difusión:**
    *   `g_tipo1(grado_magnitud, K)`
    *   `g_tipo2(grado_magnitud, K)`
    *   `g_tipo3(grado_magnitud, K)`
3.  **Algoritmo de Difusión Anisotrópica:**
    *   Función `difusion_anisotropica(image, n_iter, K, delta_t, option)`:
        *   `image`: Imagen de entrada en escala de grises.
        *   `n_iter`: Número de iteraciones del proceso de difusión.
        *   `K`: Coeficiente de conducción (kappa), umbral para la magnitud del gradiente.
        *   `delta_t`: Constante de integración (paso de tiempo), controla la estabilidad y velocidad de convergencia.
        *   `option`: Entero (1, 2, o 3) para seleccionar la función de difusión.
    *   Calcula los gradientes en las direcciones Norte, Sur, Este y Oeste.
    *   Aplica la función de difusión seleccionada a estos gradientes.
    *   Actualiza la imagen iterativamente.
4.  **Visualización y Experimentación:**
    *   Función `mostrar_imagen(...)` para aplicar los tres tipos de difusión a una imagen (generalmente con ruido añadido) y mostrar los resultados comparativamente.
    *   Se realizan pruebas con diferentes imágenes (`imagen_1` a `imagen_4`) a las que se les añade ruido (Gaussiano, Sal y Pimienta, Speckle) para observar el rendimiento del filtro.

## Parámetros Clave para la Experimentación

-   **`iteraciones_test` (n_iter):** Número de veces que se aplica el proceso de difusión. Más iteraciones generalmente resultan en un mayor suavizado.
-   **`kappa_test` (K):** El umbral del gradiente. Valores más altos de K permiten un mayor suavizado incluso cerca de los bordes, mientras que valores más bajos preservan más los bordes finos.
-   **`delta_t_test` (delta_t):** El paso de tiempo. Debe ser lo suficientemente pequeño para asegurar la estabilidad del proceso numérico. Un valor típico es entre 0.1 y 0.25.

En los experimentos del notebook, se utilizan:
*   `iteraciones_test = 10`
*   `kappa_test = 0.1`
*   `delta_t_test = 0.1`

## Requisitos

-   Python 3.x
-   NumPy
-   Matplotlib
-   Scikit-image
-   Jupyter Notebook (o cualquier entorno compatible con `.ipynb`)

Puedes instalar las bibliotecas necesarias usando pip:
```bash
pip install numpy matplotlib scikit-image notebook
```