# Documentación técnica

## Descripción general

MediCitas IA automatiza la administración básica de citas médicas desde Telegram. El escenario principal recibe un mensaje, lo transforma en una estructura JSON mediante OpenAI y utiliza routers y filtros de Make para ejecutar la acción correspondiente.

## Intenciones admitidas

- `agendar_cita`
- `reagendar_cita`
- `cancelar_cita`
- `consultar_horarios`
- `consultar_costo`
- `consultar_ubicacion`
- `urgencia`
- `humano`
- `otra`

## Rutas principales

### Atención humana

Se activa ante una urgencia, un caso delicado o una solicitud explícita de hablar con una persona. Registra el evento en la bitácora, responde de forma segura al usuario y envía una alerta interna.

### Agendar cita

1. Valida los datos mínimos.
2. Consulta la hoja `Disponibilidad`.
3. Busca o registra al paciente.
4. Crea la cita en `Citas`.
5. Marca el horario como ocupado.
6. Crea el evento en Google Calendar.
7. Guarda `Calendar_Event_ID`.
8. Responde por Telegram.
9. Registra la acción en `Bitacora`.

### Cancelar cita

Busca la cita, elimina el evento de Calendar, cambia el estado a `Cancelada`, libera el horario y limpia el identificador de la cita en `Disponibilidad`.

### Reagendar cita

Comprueba primero el nuevo horario. Si está disponible, actualiza Calendar y la hoja `Citas`, libera el horario original y ocupa el nuevo. Si no está disponible, conserva la cita original.

### Faltan datos

Solicita únicamente la información necesaria y registra la solicitud incompleta.

### Consulta general

Responde información administrativa como horario, ubicación y costo.

## Modelo de datos

### Citas

| Campo | Descripción |
|---|---|
| `ID_Cita` | Identificador único |
| `Fecha` | Fecha de la cita |
| `Hora` | Hora solicitada |
| `ID_Paciente` | Relación con Pacientes |
| `Paciente` | Nombre normalizado |
| `Doctor` | Profesional asignado |
| `Especialidad` | Área médica |
| `Estado` | Confirmada, Reagendada o Cancelada |
| `Canal` | Canal de origen |
| `Notas` | Motivo breve |
| `Calendar_Event_ID` | Identificador del evento |
| `Chat_ID` | Destino para mensajes |
| `Recordatorio_30min` | Estado del recordatorio |

### Disponibilidad

Controla fecha, hora, doctor, especialidad, disponibilidad e ID de la cita que ocupa el espacio.

### Pacientes

Almacena un identificador interno, datos básicos y referencias de Telegram.

### Bitacora

Registra timestamp, usuario, pregunta, respuesta, acción y resultado.

## Reglas de seguridad del asistente

- No diagnosticar.
- No recomendar medicamentos.
- No interpretar síntomas.
- Escalar urgencias.
- Limitar el alcance a tareas administrativas.
- Usar solamente datos ficticios en demostraciones públicas.

## Alcance

El proyecto demuestra integración y automatización. No incluye autenticación clínica, expediente médico electrónico, cifrado especializado, cumplimiento normativo certificado ni disponibilidad de producción.
