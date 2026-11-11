# Dosis exacta

**Métodos numéricos aplicados a la planificación de la radioterapia mediante un simulador web interactivo**

Feria de Ciencias Exactas DCEX 2026 · Universidad de las Fuerzas Armadas ESPE, Sede Santo Domingo
Asignatura: Métodos Numéricos · Ingeniería en Tecnologías de la Información

🔗 **Pruébalo aquí:** https://4nderr.github.io/MetodosNumericos/

---

## ¿De qué trata?

La radioterapia de intensidad modulada (IMRT) tiene que dar una dosis letal al tumor (60 Gy, es decir 2 Gy × 30 sesiones) y proteger los órganos sanos que lo rodean, como la médula espinal o el esófago. Ese problema no tiene una fórmula cerrada: se resuelve con **métodos numéricos** que aproximan e iteran con un error controlado.

*Dosis exacta* es una página web que recorre, paso a paso y de forma interactiva, el flujo de planificación del proyecto de código abierto [PortPy](https://github.com/PortPy-Project/PortPy) (Memorial Sloan Kettering Cancer Center) y muestra en qué parte actúa cada método.

> Es una simulación 2D con fines educativos. **No es una herramienta clínica.**

## Los 7 métodos numéricos

| # | Método | Para qué se usa en el simulador |
|---|---|---|
| 1 | **Serie de Taylor** | Aproximar la atenuación del haz e^(−μz) con sumas y multiplicaciones (tolerancia 10⁻¹², con reducción de rango). |
| 2 | **Newton-Raphson** (con bisección de respaldo) | Encontrar el punto donde cada rayo entra en la piel. Converge en unas 4 iteraciones. |
| 3 | **Regla de Simpson 1/3** (comparada con punto medio y trapecio) | Integrar la densidad del tejido a lo largo del rayo (profundidad radiológica), con h ≈ 2,5 mm. |
| 4 | **Interpolación lineal / bilineal** | Estimar la densidad entre vóxeles y trazar las curvas de isodosis. |
| 5 | **Descenso del gradiente proyectado** (Nesterov) | Optimizar la intensidad de 224 haces elementales (beamlets). Se detiene con tolerancia 3 × 10⁻⁵. |
| 6 | **Método de la potencia** | Estimar el mayor valor propio de AᵀWA y así fijar el tamaño del paso del optimizador. |
| 7 | **Método de Jacobi** | Calcular valores propios dentro de la SVD aleatorizada que comprime la matriz de influencia (A ≈ S + H·W). |

Base matemática (no son métodos numéricos): discretización en vóxeles, matriz de influencia dispersa d = A·x e histograma dosis-volumen (DVH).

## Recorrido de la página

| Capítulo | Contenido |
|---|---|
| 0. Portada | «Apuntar al cáncer. Con matemáticas.» y la lista de métodos. |
| 1. El reto | Haces que giran alrededor del paciente y serie de Taylor con deslizador del grado. |
| 2. Vóxeles | Discretización del cuerpo, Newton-Raphson e integración numérica. |
| 3. Matriz de influencia | d = A·x (2.738 vóxeles × 224 beamlets, ~79 % de ceros). |
| 4. Tu turno | El visitante ajusta a mano 7 haces y trata de cubrir el tumor. |
| 5. Algoritmo | Descenso del gradiente + método de la potencia, en vivo. |
| 6. DVH | Histograma dosis-volumen e isodosis por interpolación lineal. |
| 7. Compresión | A ≈ S + H·W con SVD aleatorizada y método de Jacobi. |
| 8. Resumen | Los 7 métodos y la base matemática. |

**Controles:** `←` `→` cambiar de capítulo · `F` pantalla completa · `T` modo claro/oscuro. Tras 3 minutos sin uso vuelve a la portada.

## Resultados principales

Caso de estudio: tumor en forma de «C» que rodea la médula espinal, con 7 haces.

| Indicador | Plan manual | Plan optimizado |
|---|---|---|
| Dosis máxima en la médula | 61,4 Gy | **28,2 Gy** (−54 %) |
| Dosis media en el esófago | 39,9 Gy | **14,1 Gy** (−65 %) |
| Cobertura del tumor | 95,6 % | 93 % |

- **Taylor:** con grado 1 el error a 30 cm es del 235 %; con grado 10 baja a 0,0002 %.
- **Newton-Raphson:** convergencia cuadrática (10⁻¹ → 10⁻³ → 10⁻⁶ → 10⁻¹¹).
- **Simpson** no siempre le gana al trapecio: la densidad salta en los bordes del pulmón y del hueso.
- **Optimizador:** se detiene en unas 377 iteraciones.
- **Compresión:** con rango 5 el error baja a ~6,7 % guardando solo ~7,9 % de A.

## Cómo usarlo

No necesita instalación, librerías ni internet. Todo el proyecto está en un solo archivo:

1. Descarga `index.html`.
2. Ábrelo con Chrome, Edge o Safari.

O entra directamente a la versión publicada en GitHub Pages.

## Tecnologías

- HTML, CSS y JavaScript puro (código propio, sin librerías).
- GitHub Pages para la publicación.
- PortPy como referencia conceptual (flujo de planificación, datos citados e idea de CompressRTP).

## Autores

- Jordy Jair Muela Mera
- Melanie Mabel Segura Briones
- Joseph Darian Andrade Basurto
- Kleber Samir Zambrano Mena
- Ander Humberto López Balcázar

**Tutor:** Ing. Rodrigo Bastidas

## Referencias

- Beck, A., & Teboulle, M. (2009). A fast iterative shrinkage-thresholding algorithm for linear inverse problems. *SIAM Journal on Imaging Sciences, 2*(1), 183–202. https://doi.org/10.1137/080716542
- Bortfeld, T. (2006). IMRT: A review and preview. *Physics in Medicine and Biology, 51*(13), R363–R379. https://doi.org/10.1088/0031-9155/51/13/R21
- Ehrgott, M., Güler, Ç., Hamacher, H. W., & Shao, L. (2010). Mathematical optimization in intensity modulated radiation therapy. *Annals of Operations Research, 175*(1), 309–365. https://doi.org/10.1007/s10479-009-0659-4
- Halko, N., Martinsson, P. G., & Tropp, J. A. (2011). Finding structure with randomness: Probabilistic algorithms for constructing approximate matrix decompositions. *SIAM Review, 53*(2), 217–288. https://doi.org/10.1137/090771806
- PortPy-Project. (2023). *PortPy: Planning and optimization for radiation therapy in Python* [Repositorio de software]. GitHub. https://github.com/PortPy-Project/PortPy
- Siddon, R. L. (1985). Fast calculation of the exact radiological path for a three-dimensional CT array. *Medical Physics, 12*(2), 252–255. https://doi.org/10.1118/1.595715
