# Manual de Reglas de Negocio - Gestor360

## Ley Laboral y Cumplimiento

### Regla del Séptimo Día (Descanso Obligatorio)
El sistema implementa validaciones estrictas basadas en la legislación laboral vigente:
*   **Validación de Diseño**: El "Arquitecto de Turnos" bloquea el guardado de cualquier plantilla que no contenga al menos **1 día de descanso (Séptimo Día)** por cada 6 días trabajados.
*   **Cálculo de Nómina**: Si un empleado labora en su día marcado como `isSeventhDay`, el sistema aplica un recargo automático del 100% (Tiempo Extra) independientemente de haber cumplido su jornada semanal.

---

## Gestión de Tolerancias

### 1. Late Entry (Entrada Tardía)
*   **Configuración**: Definida por `late_entry_tolerance` (predeterminado 15 minutos).
*   **Lógica**: El sistema otorga un periodo de gracia. Si la marcación supera la tolerancia, se descuenta el tiempo total desde el inicio teórico (no solo el excedente).
*   **Visualización**: El Kiosco alerta en color ámbar si hay una llegada con retraso pero dentro de la tolerancia.

### 2. Early Exit (Salida Temprana)
*   **Configuración**: Definida por `early_exit_tolerance` (predeterminado 15 minutos).
*   **Lógica**: Al igual que el ingreso, si la salida ocurre antes del horario programado superando la tolerancia, se genera una alerta proactiva y se registran los minutos faltantes para deducción.

---

## Configuración de Pausas
*   **lunch_duration**: El tiempo de pausa (ej: 60 min para almuerzo) se descuenta automáticamente del cálculo de `payable_hours`.
*   **Restricción**: El sistema no permite que la duración de la pausa sea mayor o igual a la duración total del turno programado.
