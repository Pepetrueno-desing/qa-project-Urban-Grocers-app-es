# \# Proyecto Urban Grocers

# 

# Automatización de pruebas para el endpoint `POST /api/v1/kits` de la API de Urban Grocers.

# 

# \## 📋 Descripción

# 

# Suite de pruebas automatizadas con `pytest` que valida la creación de kits de productos según la lista de comprobación del campo `name`.

# 

# \## 🛠️ Requisitos

# 

# \- Python 3.13+

# \- pytest

# \- requests

# 

# \## 🚀 Instalación

# 

# 1\. Clona el repositorio:

# &#x20;  ```bash

# &#x20;  git clone git@github.com:Pepetrueno-desing/qa-project-Urban-Grocers-app-es.git

# &#x20;  cd qa-project-Urban-Grocers-app-es
## 📊 Resultados de las pruebas

Al ejecutar `pytest create_kit_name_kit_test.py -v`:

- ✅ 5 passed
- ❌ 4 failed (esperados según la lista de comprobación)

### Bugs detectados

La API no cumple con la especificación en 4 casos:

| # | Caso | Esperado | Obtenido | Impacto |
|---|---|---|---|---|
| 3 | Nombre vacío | `400` | `201` | API acepta vacío |
| 4 | Nombre de 512 caracteres | `400` | `201` | API acepta longitud mayor a la permitida |
| 8 | Falta el parámetro `name` | `400` | `500` | **Error del servidor** |
| 9 | Tipo incorrecto (número) | `400` | `201` | API acepta tipo no válido |

### Notas

Los 4 fallos son **hallazgos de QA** y forman parte del resultado esperado de este proyecto.

