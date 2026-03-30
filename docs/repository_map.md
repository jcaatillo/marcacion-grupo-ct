# Arquitectura del Repositorio - Gestor360

## Lógica Central (Core Logic)

### 1. Sistema de Herencia de Turnos (`shift-resolver.ts`)
El sistema utiliza un algoritmo de resolución de 4 niveles para determinar el turno de un empleado en una fecha dada:

*   **Nivel 1: Excepción de Día (`employee_shift_overrides`)**: Máxima prioridad. Define cambios temporales para un empleado específico en una fecha única.
*   **Nivel 2: Asignación Fija (`employee_shifts`)**: Turnos fijos (legacy) asignados directamente al empleado.
*   **Nivel 3: Planilla Global (`global_schedules`)**: Definida por Puesto de Trabajo (`job_position_id`) y día de la semana (`day_of_week`).
*   **Nivel 4: Predeterminado de Sucursal (`branch_default_shifts`)**: Configuración base por sucursal si no hay reglas superiores aplicables.

### 2. Motor de Nómina (`payroll-engine.ts`)
Contiene las fórmulas matemáticas para el procesamiento de asistencia:
*   **Llegada Tardía**: `recorded_at - theoretical_start > late_entry_tolerance`.
*   **Salida Temprana**: `theoretical_end - recorded_at > early_exit_tolerance`.
*   **Horas Pagables**: `(Total Minutos Trabajados - lunch_duration) / 60`.
*   **Séptimo Día**: Si el día está marcado como `isSeventhDay`, el motor calcula automáticamente el 100% de las horas como Tiempo Extra.

---

## Server Actions (`/actions/schedules.ts`)
Acciones críticas para la gestión del tiempo:
*   **createShiftTemplate**: Crea plantillas con matriz de 7 días. Valida legalmente el Séptimo Día.
*   **upsertGlobalSchedule**: Gestiona el mapeo masivo Puesto <-> Turno.
*   **upsertBranchDefaultShift**: Establece el nivel 4 de la jerarquía.
*   **pinShift**: Fijar un turno desde la Matriz de Diagnóstico (vía override).

---

## Estándar de Diseño 'Stitch'
El sistema visual utiliza una arquitectura de diseño premium denominada internamente 'Stitch', caracterizada por:
*   **Componentes de Vidrio (Glassmorphism)**: Uso de `backdrop-blur` y opacidades bajas en modales (`CreateShiftModal.tsx`).
*   **Bordes de 32px**: Bordes extremadamente redondeados (`rounded-3xl`) en tarjetas y contenedores principales.
*   **Arquitecto de Turnos**: Interfaz de matriz dinámica que permite arrastrar y soltar (`ShiftCell.tsx`) y acciones en lote (Bulk Actions).
*   **Feedback Inmediato**: Sincronización optimista en la cuadrícula de planificación.

---

## Refactorizaciones Pendientes (Quality Audit)
1.  **Migración Legacy**: Eliminar dependencia de la tabla `shifts` y unificar todo bajo `shift_templates`.
2.  **Sincronización `days_config`**: Las columnas `start_time` y `end_time` en `shift_templates` son redundantes contra el JSONB. Se deben unificar o marcar como legacy.
3.  **Auditoría de Logs**: Implementar `shift_change_logs` para cambios de nivel 3 y 4 (actualmente solo se registran en `audit_logs` generales).
