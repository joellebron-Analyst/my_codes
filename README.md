Aquí tienes un **README completo y profesional** para tu script:

***

# 📊 Attendance & Exceptions Processing Pipeline

Este proyecto automatiza la extracción, transformación y generación de reportes de **asistencia** y **excepciones (vacaciones/licencias)** desde Odoo, generando archivos Excel listos para análisis operativo.

***

## 🚀 Funcionalidades principales

✅ Conexión automática a Odoo vía XML-RPC  
✅ Extracción de:

* Empleados
* Asistencia diaria (`hr.attendance`)
* Vacaciones/licencias (`hr.leave`)

✅ Limpieza y transformación de datos  
✅ Normalización de zonas horarias (UTC → America/Santo\_Domingo)  
✅ Aplicación de reglas de negocio (status, excepciones, horarios)  
✅ Generación de reportes finales en Excel:

* `Vacaciones (hr.leave).xlsx`
* `Asistencia (hr.attendance).xlsx`
* `Excepciones.xlsx`
* `Attendance_Report.xlsx`
* `Shift_Start.xlsx` (casos especiales)

***

## 📁 Estructura de carpetas

```
project/
│
├── exception_raw/
├── exception_clean/
├── attendance_raw/
├── final_results/
├── Schedule_change/
├── Schedule_Exception(fecha_explicita).xlsx
├── Schedule_Exception(Rango_fecha).xlsx
├── Schedule_Exceptions(dias_en_la_semana).xlsx
```

***

## ⚙️ Configuración

### Variables principales

```python
ingresar_fecha = 'YYYY-MM-DD'
```

Ejemplo:

```python
ingresar_fecha = '2026-06-18'
```

***

### Credenciales Odoo

```python
url      = "https://cabicash.odoo.com"
db       = "..."
username = "..."
api_key  = "..."
```

⚠️ **IMPORTANTE**: Mantén estas credenciales seguras. No subir a repositorios públicos.

***

## 🔄 Flujo del proceso

### 1. Extracción de datos

* Empleados (`hr.employee`)
* Vacaciones (`hr.leave`)
* Asistencia (`hr.attendance`)

***

### 2. Transformación de Vacaciones

* Conversión a DataFrame
* Mapeo de estados:
  * `validate` → Aprobado
  * `refuse` → Rechazado
* Conversión de fechas a zona horaria local
* Unión con empleados para obtener email
* Limpieza y estandarización

Resultado:

```
exception_raw/Vacaciones (hr.leave).xlsx
```

***

### 3. Creación de Excepciones

* Filtrado solo "Aprobado"
* Estandarización de tipos de permisos:

Ejemplo:

* Vacations → Vacation

* Licencia médica → Medical License

* Tardanza → Late In

* Asignación de "Vacation Year Period"

Resultado:

```
exception_clean/Excepciones.xlsx
```

***

### 4. Procesamiento de Asistencia

* Normalización de:
  * Clock In / Clock Out
  * Away / Lunch / Meeting
* Unión con roster (horarios)
* Manejo de registros duplicados
* Aplicación de horarios especiales:
  * Por fecha
  * Por día de semana

***

### 5. Cálculo de métricas

Se generan campos como:

* ✅ Status:
  * On Time
  * Late
  * Exception (ej: Late In, Medical, etc.)

* ✅ Worked Hours

* ✅ Schedule Hours

* ✅ Holiday flag

***

### 6. Aplicación de excepciones

Se cruzan datos con:

* `Excepciones.xlsx`

Lógica:

* Si el empleado tiene una excepción en esa fecha → se aplica
* Si no tiene asistencia ni excepción → "No Show"

***

### 7. Casos especiales

#### 🔴 Shift Start Mode

Si hay muchos `Clock Out` vacíos (>=50):

→ Se genera:

```
final_results/Shift_Start.xlsx
```

***

### 8. Reporte final

Archivo generado:

```
final_results/Attendance_Report.xlsx
```

Incluye:

* Asistencia
* Excepciones
* No show
* Horarios
* Métricas calculadas

***

## 🧠 Reglas de negocio importantes

### Status

| Condición                                | Resultado |
| ---------------------------------------- | --------- |
| Tarde + exception (Late In / Called Out) | Exception |
| A tiempo + exception                     | Exception |
| Tarde sin excepción                      | Late      |
| Caso contrario                           | On Time   |

***

### No Show

Empleado:

* Está en roster
* No tiene asistencia
* No tiene excepción

→ Status = `No Show`

***

## 🕒 Manejo de tiempo

* Todas las fechas se convierten de UTC a:

```
America/Santo_Domingo (UTC-4)
```

***

## 📦 Dependencias

```bash
pip install pandas numpy openpyxl
```

***

## ⚠️ Consideraciones

* Verificar rutas de archivos (Windows paths)
* Validar formato de Excel de entrada
* Asegurar consistencia en emails (lowercase / trim)
* Evitar duplicados en roster
* Revisar excepciones de calendario

***

## ✅ Recomendaciones

* Automatizar con scheduler (Airflow / Task Scheduler)
* Usar variables de entorno para credenciales
* Agregar logs en producción
* Validar datos antes de exportar

***

## 📌 Resultado final

Este pipeline permite:

✔ Control de asistencia diario  
✔ Identificación automática de:

* Tardanzas
* Ausencias
* Excepciones

✔ Generación de reportes listos para operaciones y RRHH


