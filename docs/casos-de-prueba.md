# Casos de prueba — MediCitas IA

Todos los nombres, teléfonos, correos, fechas de nacimiento y citas de este documento son ficticios.

## 1. Objetivo

Este documento permite validar el escenario principal de MediCitas IA después de importar el blueprint y configurar las conexiones de Telegram, OpenAI, Google Sheets y Google Calendar.

Las pruebas verifican:

- clasificación de intención;
- extracción de datos;
- validación de campos;
- disponibilidad de horarios;
- registro y reutilización de pacientes;
- creación, cancelación y reagendado de citas;
- sincronización con Google Calendar;
- bitácora;
- escalamiento humano;
- respuestas cuando una búsqueda no encuentra resultados.

## 2. Preparación del entorno

Antes de comenzar:

1. Configura la zona horaria de Make y Google Calendar como:

```text
America/Mexico_City
```

2. Verifica que existan las hojas:

```text
Citas
Disponibilidad
Pacientes
Bitacora
```

3. Usa únicamente datos ficticios.

4. Activa temporalmente `Run once` en el escenario principal.

5. Vacía las hojas `Citas`, `Pacientes` y `Bitacora`, conservando los encabezados.

6. Configura la hoja `Disponibilidad` con datos de demostración.

Ejemplo:

| Fecha | Hora | Doctor | Especialidad | Disponible | ID_Cita |
|---|---|---|---|---|---|
| 2026-07-13 | 09:00 | Dr. Pérez | Medicina general | Sí | |
| 2026-07-13 | 10:30 | Dr. Pérez | Medicina general | Sí | |
| 2026-07-13 | 11:00 | Dra. López | Pediatría | Sí | |
| 2026-07-13 | 12:00 | Dra. López | Pediatría | Sí | |
| 2026-07-13 | 17:00 | Dr. Pérez | Medicina general | Sí | |
| 2026-07-14 | 09:00 | Dra. López | Pediatría | Sí | |
| 2026-07-14 | 10:00 | Dr. Pérez | Medicina general | Sí | |
| 2026-07-14 | 11:30 | Dr. Ramírez | Cardiología | Sí | |
| 2026-07-14 | 16:00 | Dra. Torres | Dermatología | Sí | |
| 2026-07-15 | 10:00 | Dra. Ramírez | Cardiología | Sí | |
| 2026-07-15 | 11:00 | Dr. Pérez | Medicina general | Sí | |
| 2026-07-15 | 19:00 | Dr. Mendoza | Cardiología | Sí | |
| 2026-07-15 | 21:00 | Dr. Vaquero | Medicina general | Sí | |
| 2026-07-16 | 22:00 | Dr. Vaquero | Medicina general | Sí | |
| 2026-07-16 | 23:00 | Dr. Vaquero | Pediatría | Sí | |
| 2026-07-17 | 14:00 | Dr. Vaquero | Medicina general | Sí | |

## 3. Criterios generales de aprobación

Una prueba se considera aprobada cuando:

- Telegram entrega la respuesta esperada;
- las hojas modificadas conservan consistencia;
- Calendar refleja la operación cuando corresponde;
- no se duplican pacientes;
- no se ocupa un horario no disponible;
- no se cambia una variable a `si` cuando una búsqueda devuelve cero filas;
- la bitácora registra correctamente la acción;
- no aparecen errores rojos sin manejar en la ejecución.

---

# Pruebas de consultas y seguridad

## Prueba 1 — Consultar costo

**Mensaje**

> ¿Cuánto cuesta la consulta general?

**Resultado esperado**

Telegram responde:

```text
El costo de la consulta general es de $500 MXN.
```

**Validación**

- No crea paciente.
- No crea cita.
- No modifica disponibilidad.
- No crea evento en Calendar.

---

## Prueba 2 — Consultar ubicación

**Mensaje**

> ¿Dónde se encuentra el consultorio?

**Resultado esperado**

Telegram responde:

```text
El consultorio se encuentra en Hospital Mac.
```

**Validación**

No se modifica ninguna hoja operativa.

---

## Prueba 3 — Consultar horarios

**Mensaje**

> ¿Cuál es el horario de atención?

**Resultado esperado**

Telegram responde con el horario entre semana y sábado.

**Validación**

No se crea ninguna cita.

---

## Prueba 4 — Mensaje fuera de alcance

**Mensaje**

> ¿Puedes ayudarme a preparar una dieta?

**Resultado esperado**

Telegram explica que el asistente solo gestiona citas y consultas administrativas.

**Validación**

- `intencion = otra`
- `requiere_humano = no`
- `prioridad = baja`

---

## Prueba 5 — Solicitud de atención humana

**Mensaje**

> Quiero hablar con una persona del consultorio.

**Resultado esperado**

- Telegram informa que la solicitud será canalizada.
- El grupo interno recibe una alerta.
- Se registra una fila en `Bitacora`.

**Validación**

```text
intencion = humano
requiere_humano = si
prioridad = media
```

---

## Prueba 6 — Urgencia

**Mensaje**

> Tengo dolor muy fuerte en el pecho y dificultad para respirar.

**Resultado esperado**

- El bot no diagnostica.
- El bot no recomienda medicamentos.
- Indica que requiere atención directa.
- El grupo interno recibe alerta prioritaria.
- La bitácora registra el caso.

**Validación**

```text
intencion = urgencia
requiere_humano = si
prioridad = alta
```

---

# Pruebas de datos faltantes

## Prueba 7 — Agendar con datos mínimos incompletos

**Mensaje**

> Quiero una cita el 13 de julio.

**Resultado esperado**

Solicita solamente los datos faltantes:

- nombre;
- hora;
- especialidad;
- motivo.

**Validación**

No crea paciente, cita, disponibilidad ocupada ni evento.

---

## Prueba 8 — Cancelación con datos incompletos

**Mensaje**

> Quiero cancelar mi cita.

**Resultado esperado**

Solicita:

- nombre;
- fecha de la cita;
- hora de la cita.

**Validación**

No modifica ninguna cita.

---

## Prueba 9 — Reagendado con datos incompletos

**Mensaje**

> Quiero cambiar mi cita para mañana.

**Resultado esperado**

Solicita:

- nombre;
- fecha original;
- hora original;
- nueva fecha exacta;
- nueva hora.

**Validación**

No modifica Calendar ni Sheets.

---

# Pruebas de agendado

## Prueba 10 — Paciente nuevo con datos completos

**Mensaje**

> Hola, soy Ana Prueba. Mi teléfono es 5500000001, mi correo es ana.prueba@example.com y nací el 15 de abril de 1998. Quiero una cita de medicina general el 13 de julio de 2026 a las 9:00 am por consulta general.

**Resultado esperado**

1. Crea un paciente.
2. Crea una cita.
3. Marca el horario como ocupado.
4. Crea el evento en Calendar.
5. Guarda `Calendar_Event_ID`.
6. Confirma por Telegram.
7. Registra en bitácora.

**Validación en `Pacientes`**

```text
Nombre = Ana Prueba
Telefono = 5500000001
Correo = ana.prueba@example.com
Fecha_Nacimiento = 1998-04-15
```

**Validación en `Citas`**

```text
Fecha = 2026-07-13
Hora = 09:00
Estado = Confirmada
Recordatorio_30min = Pendiente
Calendar_Event_ID = no vacío
```

**Validación en `Disponibilidad`**

```text
Disponible = No
ID_Cita = mismo ID de Citas
```

---

## Prueba 11 — Paciente nuevo sin datos de registro

**Mensaje**

> Hola, soy Ricardo Incompleto. Quiero una cita de pediatría el 13 de julio de 2026 a las 11:00 am por consulta general.

**Resultado esperado**

Telegram solicita:

- teléfono;
- correo;
- fecha de nacimiento.

**Validación**

- No crea el paciente.
- No crea la cita.
- El horario de las 11:00 permanece `Sí`.
- No crea evento.

---

## Prueba 12 — Paciente existente completo

**Precondición**

La Prueba 10 fue aprobada.

**Mensaje**

> Hola, soy Ana Prueba. Quiero una cita de medicina general el 13 de julio de 2026 a las 10:30 am por revisión general.

**Resultado esperado**

- Reutiliza el mismo `ID_Paciente`.
- No crea una segunda fila en `Pacientes`.
- Crea una nueva cita.
- Ocupa el horario de las 10:30.
- Crea un nuevo evento.

---

## Prueba 13 — Horario ocupado

**Precondición**

La cita de Ana Prueba del 13 de julio a las 09:00 existe y el horario está:

```text
Disponible = No
```

**Mensaje**

> Hola, soy Pedro Ocupado. Mi teléfono es 5500000003, mi correo es pedro.ocupado@example.com y nací el 20 de mayo de 1995. Quiero una cita de medicina general el 13 de julio de 2026 a las 9:00 am por consulta general.

**Resultado esperado**

Telegram informa que el horario no está disponible.

**Validación**

- No crea paciente.
- No crea cita.
- No crea evento.
- `horario_agendar_encontrado` permanece en `no`.

---

## Prueba 14 — Paciente existente incompleto con datos nuevos

**Precondición**

Crea manualmente en `Pacientes`:

| ID_Paciente | Nombre | Telefono | Correo | Fecha_Nacimiento | Chat_ID |
|---|---|---|---|---|---|
| PAC-DEMO-001 | Laura Parcial | | | | mismo chat de prueba |

**Mensaje**

> Hola, soy Laura Parcial. Mi teléfono es 5500000004, mi correo es laura.parcial@example.com y nací el 10 de marzo de 1992. Quiero una cita de pediatría el 13 de julio de 2026 a las 12:00 pm por consulta general.

**Resultado esperado**

- Actualiza la misma fila del paciente.
- Conserva `PAC-DEMO-001`.
- Crea la cita.
- Ocupa el horario.
- Crea el evento.
- Confirma por Telegram.

---

## Prueba 15 — Paciente existente incompleto sin datos suficientes

**Precondición**

Crea manualmente un paciente con teléfono, correo o fecha de nacimiento faltante.

**Mensaje**

> Hola, soy Laura Parcial. Quiero una cita de medicina general el 13 de julio de 2026 a las 5:00 pm por consulta general.

**Resultado esperado**

Solicita completar el registro.

**Validación**

- No agenda.
- No ocupa el horario.
- No crea evento.
- La bitácora indica que el paciente existente está incompleto.

---

# Pruebas de cancelación

## Prueba 16 — Cancelar cita inexistente

**Mensaje**

> Hola, soy Persona Fantasma. Quiero cancelar mi cita del 13 de julio de 2026 a las 9:00 am.

**Resultado esperado**

Telegram informa que no encontró una cita activa.

**Validación**

- `cita_cancelar_encontrada` permanece en `no`.
- No se modifica Calendar.
- No se modifica disponibilidad.
- No se cambia ninguna fila de `Citas`.

---

## Prueba 17 — Cancelar cita existente con evento

**Precondición**

La cita de Ana Prueba del 13 de julio de 2026 a las 09:00 existe con `Calendar_Event_ID`.

**Mensaje**

> Hola, soy Ana Prueba. Quiero cancelar mi cita del 13 de julio de 2026 a las 9:00 am.

**Resultado esperado**

- Elimina el evento.
- Cambia `Estado` a `Cancelada`.
- Libera el horario.
- Limpia `ID_Cita` en `Disponibilidad`.
- Confirma por Telegram.
- Registra la cancelación.

**Validación en `Citas`**

```text
Estado = Cancelada
```

**Validación en `Disponibilidad`**

```text
Disponible = Sí
ID_Cita = vacío
```

---

## Prueba 18 — Cancelar cita existente sin evento

**Precondición**

1. Crea una cita confirmada.
2. Borra manualmente su `Calendar_Event_ID`.
3. Conserva el horario ocupado.

**Mensaje**

> Hola, soy Paciente Sin Evento. Quiero cancelar mi cita del 14 de julio de 2026 a las 10:00 am.

**Resultado esperado**

- No intenta eliminar un evento inexistente.
- Cambia la cita a `Cancelada`.
- Libera el horario.
- Confirma por Telegram.

---

## Prueba 19 — Cancelación con formato de hora menor a 10

**Precondición**

Existe una cita a:

```text
09:00
```

**Mensaje**

> Quiero cancelar mi cita de las 9 am.

**Resultado esperado**

La hora se normaliza como:

```text
09:00
```

y la búsqueda encuentra la cita.

**Validación**

La columna `Hora` de `Citas` debe mostrar `09:00`, no `9:00`.

---

# Pruebas de reagendado

## Prueba 20 — Reagendar cita inexistente

**Mensaje**

> Hola, soy Persona Fantasma. Quiero cambiar mi cita del 13 de julio de 2026 a las 9:00 am para el 14 de julio de 2026 a las 10:00 am.

**Resultado esperado**

Telegram informa que no encontró una cita activa.

**Validación**

- No ocupa el nuevo horario.
- No libera ningún horario.
- No modifica Calendar.

---

## Prueba 21 — Reagendar a horario ocupado

**Precondición**

1. Existe una cita activa del paciente.
2. El nuevo horario solicitado tiene `Disponible = No`.

**Mensaje**

> Hola, soy Ana Prueba. Quiero reagendar mi cita del 13 de julio de 2026 a las 10:30 am para el 14 de julio de 2026 a las 10:00 am.

**Resultado esperado**

Telegram informa que el nuevo horario no está disponible.

**Validación**

- La cita original permanece igual.
- El evento original permanece igual.
- El horario original continúa ocupado.
- El nuevo horario continúa ocupado por su cita original.
- `horario_reagendar_encontrado` permanece en `no`.

---

## Prueba 22 — Reagendar a horario disponible con evento existente

**Precondición**

Existe una cita activa con `Calendar_Event_ID`.

**Mensaje**

> Hola, soy Ana Prueba. Quiero reagendar mi cita del 13 de julio de 2026 a las 10:30 am para el 15 de julio de 2026 a las 11:00 am.

**Resultado esperado**

- Actualiza el evento existente.
- Conserva el mismo `ID_Cita`.
- Cambia el estado a `Reagendada`.
- Libera el horario original.
- Ocupa el nuevo horario.
- Confirma por Telegram.
- Registra en bitácora.

---

## Prueba 23 — Reagendar cita sin `Calendar_Event_ID`

**Precondición**

1. Existe una cita activa.
2. El campo `Calendar_Event_ID` está vacío.
3. El nuevo horario está disponible.

**Mensaje**

> Hola, soy Paciente Sin Evento. Quiero reagendar mi cita del 14 de julio de 2026 a las 9:00 am para el 15 de julio de 2026 a las 10:00 am.

**Resultado esperado**

- Crea un nuevo evento.
- Guarda el nuevo `Calendar_Event_ID`.
- Cambia el estado a `Reagendada`.
- Libera el horario anterior.
- Ocupa el nuevo.

---

## Prueba 24 — Cancelar una cita reagendada

**Precondición**

La Prueba 22 o 23 fue aprobada.

**Mensaje**

> Hola, soy Ana Prueba. Quiero cancelar mi cita del 15 de julio de 2026 a las 11:00 am.

**Resultado esperado**

- Encuentra una cita con estado `Reagendada`.
- Cancela la cita.
- Elimina el evento.
- Libera el horario nuevo.
- Confirma por Telegram.

---

# Pruebas de consistencia y concurrencia

## Prueba 25 — Dos solicitudes para el mismo horario

**Procedimiento**

1. Envía dos solicitudes completas para el mismo horario desde dos chats distintos con pocos segundos de diferencia.
2. Mantén activado el procesamiento secuencial.

**Resultado esperado**

- Solo una solicitud confirma la cita.
- La segunda recibe que el horario no está disponible.
- Solo existe un `ID_Cita` en la fila de disponibilidad.
- Solo se crea un evento para ese horario.

---

## Prueba 26 — No duplicar paciente por segundo agendado

**Precondición**

Existe un paciente completo con el mismo nombre y `Chat_ID`.

**Procedimiento**

Agenda dos citas distintas desde el mismo chat.

**Resultado esperado**

- Solo existe una fila en `Pacientes`.
- Ambas citas usan el mismo `ID_Paciente`.

---

## Prueba 27 — Búsqueda devuelve cero filas

**Objetivo**

Comprobar que las variables no cambian incorrectamente a `si`.

**Procedimiento**

Realiza estas solicitudes con datos inexistentes:

- horario no disponible;
- paciente inexistente;
- cita inexistente para cancelar;
- cita inexistente para reagendar.

**Resultado esperado**

Las variables correspondientes conservan `no`:

```text
horario_agendar_encontrado
paciente_encontrado
cita_cancelar_encontrada
cita_reagendar_encontrada
horario_reagendar_encontrado
```

---

# Pruebas del escenario de recordatorios

## Prueba 28 — Enviar recordatorio pendiente

**Precondición**

Existe una cita con:

```text
Estado = Confirmada
Recordatorio_30min = Pendiente
```

y su fecha y hora se encuentran dentro de la ventana configurada.

**Resultado esperado**

- Telegram envía el recordatorio.
- `Recordatorio_30min` cambia a `Enviado`.

---

## Prueba 29 — Recordatorio de cita reagendada

**Precondición**

Existe una cita con:

```text
Estado = Reagendada
Recordatorio_30min = Pendiente
```

**Resultado esperado**

El escenario envía el recordatorio usando la nueva fecha y hora.

---

## Prueba 30 — No repetir recordatorio

**Precondición**

Existe una cita con:

```text
Recordatorio_30min = Enviado
```

**Resultado esperado**

No se envía un segundo recordatorio.

---

# 4. Lista de verificación final

Antes de considerar aprobado el proyecto, verifica:

- [ ] Consultas administrativas correctas.
- [ ] Urgencias canalizadas sin dar indicaciones médicas.
- [ ] Solicitudes humanas notificadas al grupo interno.
- [ ] Paciente nuevo completo registrado.
- [ ] Paciente nuevo incompleto no registrado.
- [ ] Paciente existente reutilizado.
- [ ] Paciente incompleto actualizado en la misma fila.
- [ ] Horario ocupado rechazado.
- [ ] Cita inexistente para cancelar detectada.
- [ ] Cancelación con Calendar correcta.
- [ ] Cancelación sin Calendar correcta.
- [ ] Cita inexistente para reagendar detectada.
- [ ] Reagendado a horario ocupado rechazado.
- [ ] Reagendado disponible correcto.
- [ ] Reagendado sin Calendar crea nuevo evento.
- [ ] Cancelación de cita reagendada correcta.
- [ ] Fechas en `YYYY-MM-DD`.
- [ ] Horas en `HH:mm`.
- [ ] Procesamiento secuencial activado.
- [ ] Recordatorios enviados una sola vez.
- [ ] Bitácora consistente.
- [ ] Blueprints públicos sin datos sensibles.

## 5. Evidencia recomendada

Para documentar la aprobación, guarda capturas de:

- respuesta de Telegram;
- ejecución de Make;
- fila de `Citas`;
- fila de `Disponibilidad`;
- fila de `Pacientes`;
- fila de `Bitacora`;
- evento de Google Calendar.

Antes de subir capturas a GitHub, oculta:

- correos personales;
- IDs de conexiones;
- IDs reales de Google Sheets y Calendar;
- Chat IDs;
- nombres de usuario privados;
- datos reales de pacientes.
