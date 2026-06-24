# Visión por Computador - Escuela de Primavera 2026 UNAB

Este repositorio contiene el material e itinerario para las tres clases prácticas del curso de **Visión por Computador**, dictado en la **Escuela de Primavera 2026 de la Universidad Andrés Bello (UNAB)**. El diseño curricular y los laboratorios han sido desarrollados por **Pamela Franco**.

El curso está especialmente diseñado para estudiantes y profesionales de las ciencias exactas e ingeniería (**Físicos, Astrónomos e Ingenieros Físicos**), abordando la disciplina desde una perspectiva formal, matemática y computacional en Python.

---

## Acceso Directo a los Laboratorios (Google Colab)

Haz clic en los siguientes botones para abrir directamente los entornos interactivos de cada clase y comenzar a trabajar:

| Clase / Laboratorio | Botón de Acceso |
| :--- | :--- |
| **Clase 1:** Fundamentos Matemáticos, Computacionales y Patrones de Difracción | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU_USUARIO_GITHUB/TU_REPOSITORIO/blob/main/Clase_1_Fundamentos.ipynb) |
| **Clase 2:** Transformaciones Geométricas y Operadores Lineales | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU_USUARIO_GITHUB/TU_REPOSITORIO/blob/main/Clase_2_Transformaciones.ipynb) |
| **Clase 3:** Extracción de Atributos, Segmentación y Toma de Decisión | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU_USUARIO_GITHUB/TU_REPOSITORIO/blob/main/Clase_3_Segmentacion.ipynb) |

> **Nota:** Recuerda reemplazar `TU_USUARIO_GITHUB` y `TU_REPOSITORIO` en los enlaces de los botones de arriba por los datos reales de tu repositorio para que direccionen correctamente a tus archivos `.ipynb`.

---

## Estructura Teórica y Descripción del Curso

El programa está estructurado para conectar las teorías clásicas de la óptica y el espacio-tiempo con la visión por computador moderna y el análisis de datos tensoriales de sensores digitales (CCD/CMOS).

### 1. Procesamiento vs. Análisis de Imágenes
Se introduce formalmente la imagen como un operador matemático y como una matriz cuántica/discreta de intensidades $I(x,y)$.
* **Procesamiento de Imágenes:** Algoritmos de entrada y salida de imágenes para restauración o mejora del contraste (ej. eliminación de ruido cósmico).
* **Análisis de Imágenes:** Extracción de mediciones cuantitativas a partir de una imagen (ej. fotometría de apertura en estrellas).
* **Visión por Computador:** Adquisición y comprensión profunda de datos bidimensionales y de alta dimensión para producir información simbólica y decisiones automatizadas.

### 2. Evolución Óptica, Geométrica y Transición al Píxel
Un recorrido por los hitos que permitieron modelar el espacio tridimensional mediante ecuaciones algebraicas lineales computables:
* Las bases geométricas del **Teorema de Tales** y la perspectiva lineal de **Brunelleschi**.
* El plano cartesiano de **René Descartes** $(X, Y, Z)$ para la proyección de cámaras.
* La evolución del sensor (desde la transferencia química de fotones hasta la discretización digital en silicio), interpretando una imagen digital moderna como un **tensor de orden 2 (grises) o 3 (color)**.

```text
Objeto Real ──> Adquisición Óptica ──> Imagen I(x,y) ──> Procesamiento (Filtros) ──> Segmentación ──> Extracción de Atributos ──> Clasificación / Decisión
```


---

## Requisitos e Instalación

Para ejecutar los cuadernos de manera local, asegúrate de tener instalado Python 3 junto con las siguientes dependencias:

```bash
pip install numpy opencv-python matplotlib requests

```

---

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)