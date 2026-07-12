# Casos de prueba

Todos los datos de este documento son ficticios.

## 1. Paciente nuevo con datos completos

**Mensaje**

> Hola, soy Ana López. Mi teléfono es 5500000001, mi correo es ana.lopez@example.com y nací el 15 de abril de 1998. Quiero una cita de medicina general el 15 de junio a las 10:30 am por consulta general.

**Resultado esperado**

Crea el paciente, registra la cita, ocupa el horario, crea el evento de Calendar y responde con la confirmación.

## 2. Paciente nuevo sin datos completos

**Mensaje**

> Hola, soy Ricardo Méndez. Quiero una cita de pediatría el 15 de junio a las 11 am por consulta general.

**Resultado esperado**

No agenda todavía y solicita teléfono, correo y fecha de nacimiento.

## 3. Paciente existente

**Mensaje**

> Hola, soy Ana López. Quiero una cita de medicina general el 17 de junio a las 11 am por consulta general.

**Resultado esperado**

Reutiliza el `ID_Paciente` existente y agenda si hay disponibilidad.

## 4. Horario no disponible

**Mensaje**

> Hola, soy Carlos Ruiz. Quiero una cita de dermatología el 16 de junio a las 4 pm por consulta general.

**Resultado esperado**

Informa que el horario está ocupado y solicita otra opción.

## 5. Faltan datos de la cita

**Mensaje**

> Quiero una cita el 16 de junio.

**Resultado esperado**

Solicita nombre, hora, especialidad y motivo.

## 6. Reagendar a horario no disponible

**Mensaje**

> Hola, soy Ana López. Quiero reagendar mi cita del 15 de junio a las 10:30 am para el 16 de junio a las 4 pm.

**Resultado esperado**

No modifica la cita original.

## 7. Reagendar a horario disponible

**Mensaje**

> Hola, soy Ana López. Quiero reagendar mi cita del 15 de junio a las 10:30 am para el 17 de junio a las 11 am.

**Resultado esperado**

Actualiza Calendar y Citas, libera el horario original y ocupa el nuevo.

## 8. Cancelar cita

**Mensaje**

> Hola, soy Carlos Ruiz. Quiero cancelar mi cita de dermatología del 16 de junio a las 4 pm.

**Resultado esperado**

Cambia el estado a `Cancelada`, elimina el evento y libera la disponibilidad.

## 9. Consultar costo

**Mensaje**

> ¿Cuánto cuesta la consulta general?

**Resultado esperado**

Responde con el costo administrativo configurado.

## 10. Urgencia

**Mensaje**

> Tengo dolor muy fuerte en el pecho y dificultad para respirar, necesito ayuda.

**Resultado esperado**

No ofrece indicaciones médicas, marca el caso para atención humana y genera la alerta interna.
