# 🛒 Urban Grocers — Automatización de pruebas de API

[![API Tests](https://github.com/Pepetrueno-desing/qa-project-Urban-Grocers-app-es/actions/workflows/tests.yml/badge.svg)](https://github.com/Pepetrueno-desing/qa-project-Urban-Grocers-app-es/actions/workflows/tests.yml)

> ℹ️ El badge muestra el estado del workflow de CI. Los 4 fallos de tests corresponden a **bugs reales de la API**, documentados más abajo.

Automatización de pruebas para el endpoint `POST /api/v1/kits` de la API de Urban Grocers.

---

## 📋 Descripción

Suite de pruebas automatizadas con **pytest** que valida la creación de kits de productos según la lista de comprobación del campo `name`.

**Resultado actual:** `5 passed, 4 failed` — los 4 fallos corresponden a **bugs reales de la API** documentados más abajo.

---

## 🛠️ Requisitos

- Python 3.13+
- pytest
- requests

---

## 🚀 Instalación

Clona el repositorio y prepara el entorno:

```bash
git clone git@github.com:Pepetrueno-desing/qa-project-Urban-Grocers-app-es.git
cd qa-project-Urban-Grocers-app-es
python -m venv .venv
source .venv/bin/activate        # Linux/macOS
# .venv\Scripts\activate         # Windows
pip install -r requirements.txt


📁 Estructura del proyecto
text
qa-project-Urban-Grocers-app-es/
├── .gitignore
├── README.md
├── requirements.txt
├── configuration.py
├── data.py
├── sender_stand_request.py
└── create_kit_name_kit_test.py
Archivo	Propósito
configuration.py	URLs y rutas (URL_SERVICE, CREATE_USER_PATH, KITS_PATH)
data.py	Datos de prueba (headers, user_body, kit_body)
sender_stand_request.py	Funciones de interacción con la API (post_new_user, post_new_client_kit)
create_kit_name_kit_test.py	Suite de 9 tests automatizados
requirements.txt	Dependencias del proyecto
▶️ Ejecución
bash
pytest create_kit_name_kit_test.py -v
Resultado esperado:

text
9 collected
5 passed, 4 failed
⚠️ Los 4 fallos no son errores del proyecto, sino bugs de la API detectados por los tests.

🧪 Cobertura de pruebas
#	Test	Caso cubierto	Resultado
1	test_create_kit_1_letter_in_name_get_success_response	Nombre con 1 carácter ("a")	✅ PASSED
2	test_create_kit_511_letter_in_name_get_success_response	Nombre con 511 caracteres (límite máximo)	✅ PASSED
3	test_create_kit_0_letter_in_name_get_error_response	Nombre vacío	❌ FAILED → Bug 1
4	test_create_kit_512_letter_in_name_get_error_response	Nombre con 512 caracteres (excede límite)	❌ FAILED → Bug 2
5	test_create_kit_special_symbol_in_name_get_success_response	Caracteres especiales ("№%@")	✅ PASSED
6	test_create_kit_has_space_in_name_get_success_response	Nombre con espacios (" A Aaa ")	✅ PASSED
7	test_create_kit_has_number_in_name_get_success_response	Nombre con números ("123")	✅ PASSED
8	test_create_kit_no_name_get_error_response	Sin parámetro name	❌ FAILED → Bug 3
9	test_create_kit_number_type_name_get_error_response	name con tipo número	❌ FAILED → Bug 4
🐛 Bugs detectados
🐞 Bug 1 — La API acepta nombres vacíos
Campo	Detalle
Test	test_create_kit_0_letter_in_name_get_error_response
Esperado	400 Bad Request
Obtenido	201 Created
Impacto	Se pueden crear kits sin nombre
Severidad	Alta
🐞 Bug 2 — La API no valida el límite superior de longitud
Campo	Detalle
Test	test_create_kit_512_letter_in_name_get_error_response
Esperado	400 Bad Request (máximo 511 caracteres)
Obtenido	201 Created
Impacto	La validación de longitud falla en el límite superior
Severidad	Media
🐞 Bug 3 — La API devuelve 500 cuando falta el parámetro name (el más grave)
Campo	Detalle
Test	test_create_kit_no_name_get_error_response
Esperado	400 Bad Request
Obtenido	500 Internal Server Error
Impacto	Un 500 indica un fallo no controlado del servidor
Severidad	Crítica
🐞 Bug 4 — La API acepta tipos incorrectos en name
Campo	Detalle
Test	test_create_kit_number_type_name_get_error_response
Esperado	400 Bad Request
Obtenido	201 Created
Impacto	La API no valida el tipo del parámetro
Severidad	Alta
📚 Aprendizajes
Automatización de API con pytest + requests.

Estructura de proyectos QA: separación de configuration.py, data.py, sender_stand_request.py y test_*.py.

Manejo de tokens: obtención del authToken desde la respuesta de creación de usuario y uso en headers Authorization: Bearer {authToken}.

Casos positivos y negativos: tests que esperan 201 y tests que esperan 400.

Detección de bugs: identificar cuándo la API no cumple la especificación y clasificarlos por severidad.

Análisis de causa raíz: distinguir entre errores del cliente (4xx) y errores del servidor (5xx).

👤 Autor
Juan Mendoza (Juancho) — QA Automation Engineer en formación
🔗 GitHub · LinkedIn