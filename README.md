# 🛢️ Análisis de la Dinámica Productiva de Hidrocarburos en la Zona Cuyana (Mendoza)

![Data Science](https://img.shields.io/badge/Data_Science-EnergIA_Digital_2026-00A4E4?style=for-the-badge&logo=python)
![Fundación YPF](https://img.shields.io/badge/Fundación_YPF-Jump_Educación-005a9c?style=for-the-badge)

Este proyecto representa el trabajo final integrador del curso de Data Science de EnergIA Digital. Nos enfocamos en analizar la industria de hidrocarburos aplicando técnicas de análisis exploratorio, ingeniería de características y algoritmos de Machine Learning.

## 🎯 Objetivo del Proyecto
Analizar la evolución de la producción de hidrocarburos en la Cuenca Cuyana (enfocándonos en la provincia de Mendoza), estudiando tanto su comportamiento frente a distintos contextos del mercado económico como los tiempos de maduración de sus pozos desde el inicio de producción hasta alcanzar su pico productivo (ramp-up).

## 📊 Origen de los Datasets
Para llevar a cabo este análisis, utilizamos información pública y oficial:
- **Fuente Principal:** SESCO (Secretaría de Energía de la Nación) - Capítulo IV. Provee datos de producción de petróleo y gas con granularidad mensual por pozo.
- **Fuente Complementaria:** Argentina Oil & Gas Dataset (Kaggle). Utilizado para incorporar variables de contexto del mercado, específicamente las variaciones del precio del barril Brent.

## 🔍 Hallazgos del análisis exploratorio (AED)
> *⏳ Sección en desarrollo (Corresponde a la pre-entrega 2).*
Durante la auditoria y depuración de los datos de la Cuenca Cuyana (2021-2026), identificamos patrones claves que definen el modelado del proyecto:
- **Calidad y consistencia de los datos:** Consolidamos una base de 246.240 registros históricos garantizando trazabilidad perfecta (0 duplicados). Excluimos variables con nulos masivos (vida_util, 87% nulos) y corregimos anomalías físicas (truncamiento de producciones negativas). Además, validamos una alta completitud temporal: el 99,7% de los pozos activos posee la serie mensual ininterrumpida.
- **Detección de sesgos administrativos:** Al analizar el padrón histórico, descubrimos 3.290 pozos figuran con "fecha de primera producción" exactamente en enero de 2006. Nos percatamos de esto como un artefacto del sistema de registro y no como un evento físico real, un sesgo que será controlado al generar variables de antigüedad.
- **Redefinición empírica del alcance (Ramp-up vs. Madurez):** El cruce de datasets demostró la extrema madurez del bloque: apenas 42 pozos (1,13%) iniciaron su producción en los últimos 5 años. Este hallazgo empírico justifica enfocar nuestro modelo principal en la resiliencia y declinación de los pozos maduros, derivando el estudio de ramp-up a un análisis exploratorio secundario.

## 🤖 Hallazgos del modelado supervisado
> *Sección de desarrollo (Corresponde a la pre-entrega 3).* 
Durante el desarrollo del modelado supervisado, implementamos y evaluamos distintos algoritmos de Machine Learning para predecir la producción futura de petróleo de los pozos maduros de la Cuenca Cuyana, obteniendo los siguientes resultados y hallazgos:
- **Selección de pozos maduros y preparación de datos:** A partir de los 3.702 pozos originales, identificamos **851 pozos maduros con producción continua**, aplicando criterios de extracción efectiva, producción positiva y al menos 24 meses consecutivos de actividad. Obtuvimos un dataset de **44.948 registros**, sin interrupciones temporales dentro de las secuencias seleccionadas.
- **Ingeniería de características y predicción por horizontes:** Incorporamos variables temporales (*lags*, promedios y desviaciones móviles) para representar el comportamiento histórico de cada pozo. Construimos seis variables objetivo (H1–H6), permitiendo estimar la producción entre uno y seis meses hacia adelante. El conjunto histórico con características y objetivos completos quedó compuesto por **34.736 registros**.
- **Selección de variables y prevención de fuga temporal:** Dividimos los datos cronológicamente, utilizando 2021–2024 para entrenamiento y 2025 para evaluación, con controles para reducir el *data leakage*. Excluimos `empresa` debido a cambios de operadores durante 2025 y descartamos la antigüedad productiva por inconsistencias en las fechas históricas de inicio. Mantuvimos `areayacimiento` como variable categórica.
- **Entrenamiento y evaluación de modelos:** Implementamos y comparamos modelos como Regresión Lineal, Random Forest y k-NN, utilizando MAE, RMSE y R² para evaluar su desempeño. En el baseline de predicción del mes siguiente (H1), la **Regresión Lineal obtuvo un MAE de 11,68 m³**, mejorando la referencia de persistencia (12,18 m³). También incorporamos ajustes de hiperparámetros y análisis de *overfitting*.
- **Pronóstico de producción para el segundo semestre de 2026:** Aplicamos el enfoque de predicción directa H1–H6 sobre **522 pozos maduros**, estimando su producción mensual de julio a diciembre. Los resultados proyectan una disminución de **49.780,97 m³ en julio a 48.939,53 m³ en diciembre**, equivalente a una reducción del **1,69 %**. Estas estimaciones corresponden exclusivamente a los pozos seleccionados y no representan la producción total de la cuenca.
**Conclusión:** El modelado supervisado permitió pasar del análisis descriptivo de la producción histórica a la generación de pronósticos cuantitativos, incorporando herramientas para evaluar el desempeño predictivo y analizar la evolución futura de los pozos maduros.

## 🧩 Hallazgos del modelado no supervisado (Clustering)
> *Sección de desarrollo (Corresponde a la entrega final).*
Con el objetivo de complementar los pronósticos cuantitativos y descubrir tipologías operativas latentes, aplicamos algoritmos de aprendizaje no supervisado para segmentar los pozos maduros según su comportamiento productivo y características físicas:
- **Construcción del dataset a nivel pozo:** Partiendo de los 38.858 registros históricos sin fuga temporal (hasta diciembre de 2024), agregamos las métricas mensuales en una única fila por pozo (`idpozo`), consolidando una matriz de **580 pozos únicos**.
- **Selección de variables y estandarización:** Tras descartar variables sin variabilidad (`iny_gas`, `iny_co2`), con nulos masivos (`vida_util`, 99,94 %) o altamente colineales (`prod_pet_mediana` y `tef_std`), seleccionamos 6 atributos clave: nivel y dispersión de petróleo (`prod_pet_media`, `prod_pet_std`), producción media de gas y agua (`prod_gas_media`, `prod_agua_media`), tiempo efectivo de flujo (`tef_media`) y `profundidad`. Aplicamos `StandardScaler` para evitar sesgos por magnitud en el cálculo de distancias.
- **Optimización y selección del modelo (K-Means):** Evaluamos configuraciones entre $k = 2$ y $k = 8$ mediante el método del codo y el coeficiente de silueta. Seleccionamos **`k = 4`** al alcanzar el máximo índice Silhouette (**0,333**) y presentar el punto de inflexión óptimo en la curva de inercia.
- **Identificación de perfiles productivos:** La segmentación permitió caracterizar cuatro grupos operativos bien diferenciados:
  - **Cluster 0 (122 pozos — Alta extracción de agua y gas):** Pozos relativamente someros (profundidad media de 2.019 m) con buena producción de petróleo (122,46 m³), elevada producción de gas y un corte de agua muy alto (10.030,41 m³).
  - **Cluster 1 (71 pozos — Baja productividad e inestabilidad operativa):** Presentan el menor rendimiento medio de petróleo (62,37 m³), alta dispersión relativa y el menor tiempo efectivo de flujo (21,29 días/mes).
  - **Cluster 2 (356 pozos — Perfil base de la cuenca):** Concentra el 61,4 % de la muestra analizada, exhibiendo valores intermedios y estables en producción de petróleo (79,68 m³), agua y profundidad (2.599 m).
  - **Cluster 3 (31 pozos — Pozos profundos de alta productividad):** Grupo reducido ubicado a mayor profundidad (3.138 m promedio) que lidera la extracción de petróleo (332,15 m³) y gas con muy baja presencia de agua.

---

## 👥 Nuestro Equipo
Somos un equipo de tres desarrolladores de software con formación previa en programación, metodologías ágiles, ingeniería de datos y gestión del ciclo de vida de proyectos. Si bien cada integrante asumió un foco específico según sus fortalezas para organizar el flujo de trabajo, todos participamos activamente en el análisis y aplicamos de manera conjunta los conocimientos de Data Science adquiridos a lo largo del curso.

<table align="center">
  <tr>
    <td align="center" width="33%">
      <h3>Leandro Olarte</h3>
      <img src="https://img.shields.io/badge/Rol-Líder_Técnico_&_Git-00A4E4?style=for-the-badge" alt="Rol Leandro"/>
      <p>Responsable de la <b>arquitectura del repositorio</b>: estructura base, ramas de trabajo e integración de cambios en <code>main</code>. Garantiza que el código y la documentación se mantengan ordenados y trazables.</p>
    </td>
    <td align="center" width="33%">
      <h3>Milagros Ranaldi</h3>
      <img src="https://img.shields.io/badge/Rol-Data_Analyst_&_EDA-005a9c?style=for-the-badge" alt="Rol Milagros Ranaldi"/>
      <p>Encargada de la <b>limpieza y depuración de datos</b> de SESCO y del análisis exploratorio: distribuciones, variables categóricas, valores faltantes y outliers.</p>
    </td>
    <td align="center" width="33%">
      <h3>Milagros Carrillo</h3>
      <img src="https://img.shields.io/badge/Rol-Scrum_Master_&_Analista-00A4E4?style=for-the-badge" alt="Rol Milagros Carrillo"/>
      <p>Lidera la <b>planificación ágil</b> (backlog y tablero en Trello) y construye las <b>tablas macro y micro</b> de producción mensual que sustentan el análisis.</p>
    </td>
  </tr>
</table>

---

## 📅 Tabla de actualizaciones y progreso
Para quienes nos visitan y quieren seguir la evolución del proyecto, aquí registramos nuestros avances:

| Fecha | Tarea / Hito | Responsable | Estado |
| :---: | :--- | :---: | :---: |
| 13/09/2026 | Creación de la estructura del repositorio base y ramas. | Leandro Olarte | ✅ Completado |
| 14/09/2026 | Descarga y limpieza del dataset de SESCO (Filtro Mendoza). | Milagros Ranaldi | ✅ Completado |
| 14/09/2026 | Generar Backlog 2° pre-entrega y creación de tablero en Trello. | Milagros Carrillo | ✅ Completado |
| 15/09/2026 | Finalizada la Sprint 1, con todos sus hitos cumplidos y la documentación actualizada. | Equipo | ✅ Completado |
| 16/09/2026 | Construcción de tablas macro y micro para mostrar la producción total mensual | Milagros Carrillo | ✅ Completado |
| 16/09/2026 | EDA — Distribuciones y variables categóricas | Milagros Ranaldi | ✅ Completado |
| 17/09/2026 | Tratamiento de valores faltantes y outliers | Milagros Ranaldi | ✅ Completado |
| 18/09/2026 | Finalización del sprint 2: análisis y exploración de datos | Equipo | ✅ Completado |
| 18/09/2026 | Integración y aplicación de cambios de las ramas de trabajo en la rama principal (main) | Leandro Olarte | ✅ Completado |
| 30/09/2026 | Inicio del Sprint 3: Feature Engineering y preparación del modelo supervisado. | Milagros Ranaldi | ✅ Completado |
| 02/10/2026 | Selección de pozos maduros con producción continua y construcción de variables temporales (lags y ventanas móviles). | Milagros Ranaldi | ✅ Completado |
| 03/10/2026 | Creación de las variables objetivo H1–H6 para predecir la producción futura de petróleo. | Milagros Ranaldi | ✅ Completado |
| 04/10/2026 | Implementación del split temporal y controles para evitar data leakage. | Milagros Carrillo | ✅ Completado |
| 06/10/2026 | Encoding de variables categóricas, escalado y selección de características para el modelo. | Milagros Carrillo | ✅ Completado |
| 07/10/2026 | Análisis complementario de `meses_hasta_pico` e implementación del baseline de producción t+1. | Milagros Carrillo | ✅ Completado |
| 07/10/2026 | Integración inicial del Feature Engineering en `develop` mediante Pull Request #3. | Leandro Olarte | ✅ Completado |
| 10/10/2026 | Implementación de la comparación de modelos de regresión, ajuste de hiperparámetros y evaluación de overfitting para H1–H6. | Leandro Olarte | ✅ Completado |
| 10/10/2026 | Actualización del README y documentación del proyecto. | Leandro Olarte | ✅ Completado |
| 12/11/2026 | Inicio del Sprint 5: Diseño del pipeline de clustering y agregación del dataset a nivel pozo. | Milagros Ranaldi | ✅ Completado |
| 13/11/2026 | Análisis de correlación, selección de 6 variables productivas/físicas y estandarización mediante `StandardScaler`. | Milagros Carrillo | ✅ Completado |
| 14/11/2026 | Evaluación de hiperparámetros de clustering (Método del Codo e índice Silhouette para $k=2..8$) y entrenamiento de K-Means ($k=4$). | Milagros Ranaldi | ✅ Completado |
| 15/11/2026 | Caracterización estadística de los 4 perfiles de pozos, generación de visualizaciones comparativas e integración en el repositorio. | Milagros Ranaldi | ✅ Completado |
| 16/11/2026 | Actualización final del README con los hallazgos del modelado no supervisado. | Leandro Olarte | ✅ Completado |
| 16/11/2026 | Inicio de la SPRINT 6 - Integración y Entregas (Git). | Leandro Olarte | ✅ Completado |
| 16/11/2026 | Pull request de las ramas del repositorio a la rama main para entrega final. | Leandro Olarte | ✅ Completado |
| Por confirmar | Sprint review final para validar el cierre del proyecto, revisar entregables y preparar la presentación. | Equipo | 🟡 Pendiente |
