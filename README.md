Aquí tienes un **README.md profesional y claro** para tu script:

***

# 📊 Attendance & Exceptions Processor

Este proyecto extrae, procesa y genera reportes de asistencia y excepciones (vacaciones, permisos, licencias, etc.) desde **Odoo (HR module)** utilizando XML-RPC y `pandas`.

***

## 🚀 Descripción

El script realiza las siguientes funciones:

1. 🔐 **Autenticación en Odoo**
2. 👥 **Extracción de empleados**
3. 🏖️ **Extracción y limpieza de vacaciones (excepciones)**
4. ⏱️ **Extracción de asistencia diaria**
5. 🔄 **Transformación y limpieza de datos**
6. 📅 **Aplicación de reglas de negocio (late, on time, exceptions)**
7. 📁 **Generación de reportes finales en Excel**

***

## ⚙️ Requisitos

Instala las dependencias antes de ejecutar:

```bash
pip install pandas numpy
```

***

## 🔐 Configuración

Edita las variables de conexión a Odoo:

```python
url = "https://cabicash.odoo.com"
db = "your_database"
username = "your_email"
api_key = "your_api_key"
```

***

## 📅 Parámetros principales

```python
is_holiday = 'Yes'  # 'Yes' o 'No'
ingresar_fecha = '2026-06-04'
```

* `is_holiday`: Define si el día es feriado
* `ingresar_fecha`: Fecha a procesar

***

## 📥 Fuentes de datos externas

El script requiere los siguientes archivos Excel:

```
Schedule_change/
├── Roster.xlsx
├── Roster_Sabado.xlsx

Schedule_Exception/
├── Schedule_Exception(fecha_explicita).xlsx
├── Schedule_Exception(Rango_fecha).xlsx
├── Schedule_Exceptions(dias_en_la_semana).xlsx
```

***

## 📊 Flujo del Proceso

### 1. Extracción de datos

* Empleados (`hr.employee`)
* Vacaciones (`hr.leave`)
* Asistencia (`hr.attendance`)

***

### 2. Procesamiento de Vacaciones

* Traducción de estados
* Conversión de zona horaria
* Normalización de tipo de excepción

Ejemplos:

* `Vacations → Vacation`
* `Licencia médica → Medical License`

***

### 3. Procesamiento de Asistencia

* Conversión de UTC → America/Santo\_Domingo
* Merge con roster
* Manejo de duplicados
* Cálculo de:
  * Horas trabajadas
  * Lunch
  * Away

***

### 4. Reglas de negocio

Se calcula el estado del empleado:

| Condición                     | Resultado |
| ----------------------------- | --------- |
| Llegó tarde sin justificación | Late      |
| Tiene excepción válida        | Exception |
| Llegó a tiempo                | On Time   |

***

### 5. Manejo de casos especiales

✅ **Shift Start Mode**

* Activado cuando faltan muchos `Clock Out`
* Genera reporte alternativo

✅ **No Show / Holiday**

* Si `is_holiday = Yes` → `Holiday at Home`
* Si no → `No Show`

***

## 📤 Outputs

El script genera:

### 📁 Shift Start

```bash
Attendance/final_results/Shift_Start.xlsx
```

### 📁 Attendance Report

```bash
final_results/Attendance/Attendance_Report.xlsx
```

***

## 🧠 Lógica clave

### ⏰ Evaluación de tardanza

```python
Clock In > Schedule In + 6 min → Late
Clock In ≤ Schedule In + 5 min → On Time
```

***

### 📌 Matching de excepciones

Se cruza por:

* Email
* Rango de fechas

***

## ⚠️ Notas importantes

* El script asume zona horaria: `America/Santo_Domingo`
* Maneja valores nulos en `Clock Out`
* Normaliza formatos de tiempo (AM/PM)
* Puede ejecutarse como `.py` o `.exe`

***

## ✅ Ejecución

```bash
python attendance_script.py
```

***

## 📈 Mejoras futuras

* Agregar logging
* Parametrizar rutas
* Soporte para múltiples fechas
* Validación de archivos externos

***

## 👨‍💻 Autor

Desarrollado para automatización de reportes de asistencia y excepciones en Odoo.

