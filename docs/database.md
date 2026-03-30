# Documentación de Base de Datos - Gestor360

## Tablas y Relaciones
El sistema Gestor360 utiliza PostgreSQL con Supabase. A continuación se detallan las tablas principales y sus relaciones.

### 1. Entidades de Estructura
*   **companies**: Registro maestro de empresas con aislamiento de datos.
*   **branches**: Sucursales vinculadas a empresas (`company_id`).
*   **departments**: Áreas funcionales dentro de una sucursal o empresa.
*   **job_positions**: Cargos que definen jerarquías e iconos (`icon_name`).

### 2. Gestión de Personal y Contratos
*   **employees**: Información maestra del personal. Vinculada a `profiles` para autenticación.
*   **contracts**: Contratos laborales de empleados. Incluyen salario, moneda y fechas de vigencia.
*   **employee_pins**: Almacena PINs cifrados (`pin_ciphertext`) y digests para autenticación en Kiosco.

### 3. Planificación y Turnos (Matrix System)
*   **shift_templates**: La pieza central de la Fase 3. Define la configuración de días (`days_config`) y tolerancias.
*   **global_schedules**: Mapeo de Plantillas de Turno a Puestos de Trabajo por día de la semana.
*   **branch_default_shifts**: Default por sucursal en caso de no existir planilla global.
*   **employee_shift_overrides**: Excepciones individuales por fecha específica (Nivel 1 de herencia).
*   **shifts**: Tabla *Legacy* para turnos fijos simples.

### 4. Asistencia y Nómina
*   **attendance_logs**: Registros de marcación (Reloj). Incluye coordenadas GPS y metadatos de dispositivo.
*   **time_records**: Historial de auditoría de eventos de tiempo (clock_in, clock_out).
*   **time_corrections**: Solicitudes de corrección de tiempo aprobadas por administradores.
*   **leave_requests**: Solicitudes de permisos y vacaciones.

---

## Lógica JSONB: `days_config`
La tabla `shift_templates` utiliza una columna JSONB llamada `days_config` para manejar la variabilidad semanal en un solo registro.

**Estructura del Array:**
```json
[
  {
    "dayOfWeek": 1, 
    "label": "Lunes",
    "isActive": true,
    "isSeventhDay": false,
    "startTime": "08:00",
    "endTime": "17:00"
  },
  ...
]
```
*   **isActive**: Define si el día es laboral bajo esta plantilla.
*   **isSeventhDay**: Marca el día como descanso obligatorio (Séptimo Día).
*   **startTime / endTime**: Horarios específicos para ese día.

---

## Políticas RLS (Seguridad de Empresa)
Todas las tablas críticas tienen RLS (Row Level Security) habilitado:
*   **Aislamiento**: Se filtra por `company_id` contra el `profile` del usuario autenticado.
*   **Roles**: Solo perfiles con membresía activa en la empresa pueden realizar lecturas/escrituras.

---

## Constraints y Protección de Integridad
*   **Únicos Globales**: La tabla `global_schedules` previene duplicados para un mismo puesto, día y empresa (`unique_job_day_company`).
*   **Herencia Protegida**: Llaves foráneas con `ON DELETE RESTRICT` en tablas maestras.
