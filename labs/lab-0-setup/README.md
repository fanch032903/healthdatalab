# Laboratorio 01: Cimientos del Patient Journey - Configuración y Reproducibilidad 🛠️

Este laboratorio constituye el punto de partida técnico para el curso **BE3006: Análisis de Datos Biomédicos**. Siguiendo la visión del **Patient Journey**, estableceremos un entorno de trabajo que garantice la **reproducibilidad de grado regulatorio**, un estándar exigido por entidades como la FDA y la EMA para validar resultados analíticos en salud.

## 🎯 Objetivos

- Desplegar un entorno analítico reproducible utilizando **Docker** y **Docker Compose**.
- Diferenciar entre sistemas **OLTP** (transacciones clínicas) y **OLAP** (análisis de datos masivos).
- Verificar la interoperabilidad técnica conectando un **Jupyter Notebook** con una base de datos **PostgreSQL**.

---

## 🚀 Paso 1: Instalación de Herramientas Base

Antes de iniciar, es necesario contar con las únicas herramientas que residen directamente en el sistema operativo local para evitar "dependencias quisquillosas" entre las aplicaciones y el hardware.

1.  **Docker Desktop:** Permite la virtualización a nivel de aplicación (contenedores) sin la sobrecarga de una máquina virtual tradicional.
2.  **Git:** Para el control de versiones y la gestión de la gobernanza del código.

---

## 🏗️ Paso 2: El Blueprint de la Arquitectura (`docker-compose.yml`)

Siguiendo las directrices técnicas del libro _Hands-On Healthcare Data_ (Capítulo 2), utilizaremos contenedores para separar las preocupaciones de infraestructura. Tu archivo `docker-compose.yml` debe orquestar dos servicios principales:

1.  **`db` (PostgreSQL):** Representa el almacenamiento de datos del **EHR (Electronic Health Record)**. Es un sistema orientado a transacciones (**OLTP**).
2.  **`jupyter`:** Representa nuestro entorno de procesamiento analítico (**OLAP**), optimizado para escanear grandes subconjuntos de datos.

**Comando para iniciar:**

```bash
docker compose up -d
```

_Este comando descarga las imágenes oficiales y levanta los servicios de forma aislada._

---

## 🧪 Paso 3: Prueba de Humo y Conexión (`connection_test.ipynb`)

Para confirmar que nuestra pila de datos biomédicos está lista, ejecutaremos una consulta en Python que verifique la comunicación entre el entorno analítico y la base de datos.

**Código de verificación:**

```python
import pandas as pd
from sqlalchemy import create_engine

# String de conexión al contenedor de PostgreSQL
engine = create_engine('postgresql://uvg_user:uvg_password@db:5432/health_data')

try:
    df = pd.read_sql("SELECT 1 as connection_status", engine)
    print("¡Conexión Exitosa! El entorno analítico está listo.")
    print(df)
except Exception as e:
    print(f"Error de conexión: {e}")
```

_Este paso asegura la **Interoperabilidad Técnica**: la capacidad de enviar y recibir "bits y bytes" de forma confiable entre sistemas heterogéneos._

---

## 🧠 Mentalidad Empresarial: OLTP vs. OLAP

Es vital comprender por qué no realizamos análisis directamente sobre las bases de datos de producción de un hospital.

- **OLTP (Online Transactional Processing):** Diseñado para registrar acciones rápidas (ej. una enfermera administrando un medicamento).
- **OLAP (Online Analytical Processing):** Diseñado para responder preguntas de investigación (ej. ¿Cuál es el promedio de estancia de pacientes con sepsis en la región?).

---

## 🏆 Reto: Tarea de Gobernanza

Para cumplir con la **Competencia 1 (Gobernanza de datos)**, cada estudiante debe documentar el origen de sus datos.

**Instrucciones:**

1.  Crea un **Issue** en este repositorio de GitHub titulado "Gobernanza: Metadatos de Proyecto - [Tu Nombre]".
2.  Responde: ¿Cuál es el propósito del set de datos que usarás (MIMIC-III o Synthea)?.
3.  Identifica si tu análisis será de **Uso Primario** (decisión clínica individual) o **Uso Secundario** (investigación/IA/conocimiento poblacional).
4.  Adjunta una captura de pantalla del log de Docker mostrando que tus servicios están en ejecución (`Running`).

---
