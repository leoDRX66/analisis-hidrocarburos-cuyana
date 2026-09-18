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

## 🔍 Hallazgos del Análisis Exploratorio (AED)
> *⏳ Sección en desarrollo (Corresponde a la pre-entrega 2).*
Durante la auditoria y depuración de los datos de la Cuenca Cuyana (2021-2026), identificamos patrones claves que definen el modelado del proyecto:
- **Calidad y consistencia de los datos:** Consolidamos una base de 246.240 registros históricos garantizando trazabilidad perfecta (0 duplicados). Excluimos variables con nulos masivos (vida_util, 87% nulos) y corregimos anomalías físicas (truncamiento de producciones negativas). Además, validamos una alta completitud temporal: el 99,7% de los pozos activos posee la serie mensual ininterrumpida.
- **Detección de sesgos administrativos:** Al analizar el padrón histórico, descubrimos 3.290 pozos figuran con "fecha de primera producción" exactamente en enero de 2006. Nos percatamos de esto como un artefacto del sistema de registro y no como un evento físico real, un sesgo que será controlado al generar variables de antigüedad.
- **Redefinición empírica del alcance (Ramp-up vs. Madurez):** El cruce de datasets demostró la extrema madurez del bloque: apenas 42 pozos (1,13%) iniciaron su producción en los últimos 5 años. Este hallazgo empírico justifica enfocar nuestro modelo principal en la resiliencia y declinación de los pozos maduros, derivando el estudio de ramp-up a un análisis exploratorio secundario.

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
| ---------- | Inicio del sprint 3 | Equipo | ⏳ Pendiente |


