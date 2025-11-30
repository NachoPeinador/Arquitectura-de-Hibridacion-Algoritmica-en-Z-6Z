# El Espectro Modular de $\pi$: Unificación Teórica y Validación a Exaescala

[![License: PolyForm Noncommercial](https://img.shields.io/badge/License-PolyForm_Noncommercial_1.0.0-red.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Platform](https://img.shields.io/badge/Platform-Google_Colab_%7C_Linux-orange.svg)](pi_modular_100m.ipynb)
[![Status](https://img.shields.io/badge/Status-Validated_(10^8_Digits)-success.svg)]()
[![DOI](https://img.shields.io/badge/DOI-Preprint_Pending-lightgrey)](https://zenodo.org/)

**Autor:** José Ignacio Peinador Sala  
**Contacto:** [joseignacio.peinador@gmail.com](mailto:joseignacio.peinador@gmail.com)  
**ORCID:** [0009-0008-1822-3452](https://orcid.org/0009-0008-1822-3452)

---

## 📜 Resumen Ejecutivo

Este repositorio aloja la **implementación de referencia** y los resultados experimentales del "Hiper-Computador Modular", una arquitectura algorítmica diseñada para superar el "Muro de la Memoria" en el cálculo de constantes trascendentes.

Basado en el marco teórico del [Espectro Modular](https://doi.org/10.5281/zenodo.17680024), este proyecto transforma la serie de Chudnovsky en un sistema de **6 canales polifase independientes**. La arquitectura resultante, denominada **"Shared-Nothing Stride-6"**, elimina las condiciones de carrera y permite una paralelización lineal sin bloqueos de memoria.

![Arquitectura Hex-Helix](img/arquitectura_helix_3d.png)
*> Visualización del Espacio de Fase 'Hex-Helix': 6 hilos de ejecución aislados procesando la serie en paralelo.*

---

## 🚀 Hito de Validación: "The 100M Barrier Run"

El algoritmo ha sido validado sometiéndolo a una prueba de estrés computacional extrema en un entorno limitado (Google Colab Standard, 2 vCPU, 12GB RAM).

| Métrica | Resultado Validado |
| :--- | :--- |
| **Objetivo** | **100,000,000 (Cien Millones) de Dígitos** |
| **Tiempo Total** | 1194.32 s (~19.9 min) |
| **Speedup (2 Cores)** | **1.90x** (Eficiencia paralela del 95%) |
| **Gestión de RAM** | Sin fugas (*Leak-free*) gracias al aislamiento de canales |
| **Integridad** | Verificada contra *y-cruncher* (bit-exact) |

---

## 📂 Estructura del Repositorio y 💻 Reproducibilidad

* **`Paper/`**: Contiene el manuscrito científico en formato PDF y los archivos fuente LaTeX.
    * `Isomorfismo_DSP.tex`: Archivo principal del artículo. 
* **`Notebooks/`**: Notebooks de Jupyter/Colab con la validación computacional.
    * [Laboratorio_Modular_pi](https://colab.research.google.com/github/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/blob/main/Notebooks/Laboratorio_Modular_pi.ipynb): Todo el proceso experimental, errores y aciertos.
    * [VALIDACION_EXAESCALA_EM_PI](https://colab.research.google.com/github/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/blob/main/Notebooks/VALIDACION_EXAESCALA_EM_PI.ipynb): Reproduce los experimentos clave citados en el manuscrito para verificar:

         -   Fundamento Teórico: ¿Es válida la descomposición para funciones trigonométricas?
         -   Isomorfismo DSP: ¿Se comporta la aritmética modular como un banco de filtros polifase?
         -   Algoritmo Stride-6: ¿Es capaz la arquitectura de calcular con precisión arbitraria paralelizada?.
              -    ### 🚀The 100M Barrier Run.
         -   Hipótesis de Riemann: ¿Muestra el filtro modular "rigidez espectral" en los ceros de la función Zeta?.
           
* **`Docs/`**: Documentos para la divulgación científica de los hallazgos.
    
---

## ⚙️ Innovación Técnica

### 1. Motor "Stride-6"
A diferencia del *Binary Splitting* convencional que avanza paso a paso ($k \to k+1$), nuestro motor implementa una función de hoja (leaf function) que comprime la complejidad algebraica de un bloque de 6 términos en una única matriz de transición. Esto reduce la profundidad del árbol de recursión y alinea los accesos a memoria con la estructura de caché.

### 2. Isomorfismo DSP
La arquitectura implementa matemáticamente una **Descomposición Polifase**, tratada habitualmente en Procesamiento Digital de Señales (filtros FIR). Demostramos que calcular $\pi$ es isomorfo a filtrar una señal discreta en 6 sub-bandas, lo que garantiza matemáticamente la ortogonalidad de los procesos.

### 3. Arquitectura Shared-Nothing
Cada uno de los 6 "Workers" opera en un espacio de direcciones virtualmente aislado. Los grandes enteros (*Big Integers*) se crean y destruyen dentro del ciclo de vida de cada canal, permitiendo que el *Garbage Collector* de Python recupere memoria de forma agresiva, evitando la fragmentación del Heap que suele abortar estos cálculos en hardware modesto.

---

## ⚖️ Licencia y Uso (Dual Licensing)

Este proyecto utiliza un modelo de **Licenciamiento Dual** para garantizar su disponibilidad para la ciencia abierta mientras protege la propiedad intelectual comercial.

### ✅ Uso Académico y Educativo (Gratuito)
El código fuente y la documentación están disponibles bajo la licencia **PolyForm Noncommercial License 1.0.0**.
* **Permitido:** Uso personal, investigación académica, docencia, y bifurcación (fork) para proyectos sin ánimo de lucro.
* **Requisito:** Debe atribuir la autoría original y mantener este aviso de licencia.

### ⛔ Uso Comercial (Requiere Licencia)
**Cualquier uso con fines de lucro está estrictamente prohibido** sin un acuerdo comercial previo. Esto incluye:
* Integración en software propietario o servicios SaaS.
* Uso en calculadoras, benchmarks de hardware comerciales o productos de nube.

> 💼 **Para obtener una Licencia Comercial**, contacte con el autor: [joseignacio.peinador@gmail.com](mailto:joseignacio.peinador@gmail.com)


## ✍️ Citación

Si utiliza este software o los hallazgos teóricos en su investigación, por favor cite el artículo unificado:

```bibtex
@article{PeinadorSala2025_Exascale,
  author = {Peinador Sala, José Ignacio},
  title = {El Espectro Modular de $\pi$: Unificación Teórica, Isomorfismo DSP y Validación a Exaescala},
  year = {2025},
  publisher = {Zenodo},
  version = {v1.0},
  doi = {10.5281/zenodo.XXXXXXX}
}
```
---

## 🚀 Apoya la Ciencia Independiente

Este desarrollo es fruto de la investigación independiente. Si la arquitectura te resulta útil o inspiradora:

⭐️ Dale una Estrella: Ayuda a visibilizar el repositorio.

📢 Difunde: Comparte los resultados en redes con el hashtag #ModularPi.

🤝 Colabora: Los PRs para optimizar la recombinación final o portar el código a C++/Rust son bienvenidos (bajo la misma licencia no comercial).

"La simplicidad es la máxima sofisticación." — Leonardo da Vinci


