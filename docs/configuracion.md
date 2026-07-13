# Configuración del proyecto — MediCitas IA

## 1. Requisitos previos

Para ejecutar el proyecto se necesita:

- una cuenta de Make;
- un bot de Telegram;
- acceso a OpenAI desde Make;
- una cuenta de Google;
- Google Sheets;
- Google Calendar;
- los blueprints sanitizados del repositorio.

Este proyecto está diseñado como una demostración académica. Utiliza únicamente datos ficticios.

## 2. Archivos que se deben importar

En Make crea dos escenarios e importa:

```text
blueprints/medicitas-ia-sanitizado.blueprint.json
blueprints/recordatorio-citas-sanitizado.blueprint.json
```

El blueprint principal gestiona:

- mensajes de Telegram;
- clasificación de intención;
- registro de pacientes;
- agendado;
- cancelación;
- reagendado;
- consultas administrativas;
- atención humana;
- bitácora.

El segundo blueprint gestiona recordatorios previos a la cita.

Los archivos publicados están sanitizados y contienen marcadores que deben sustituirse manualmente.

## 3. Marcadores del blueprint

Después de importar, configura los siguientes valores:

| Marcador | Valor requerido |
|---|---|
| `CONFIGURAR_SPREADSHEET_ID` | ID del archivo de Google Sheets |
| `CONFIGURAR_CALENDAR_ID` | ID del calendario de Google |
| `CONFIGURAR_CHAT_ID_EQUIPO` | Chat ID del grupo interno de Telegram |
| `__IMTCONN__: 0` | Seleccionar una conexión propia |
| `__IMTHOOK__: 0` | Crear o seleccionar un webhook propio |

No reemplaces referencias dinámicas como:

```text
{{1.message.chat.id}}
```

Ese valor corresponde al usuario que envió el mensaje y no es un secreto.

## 4. Preparar Google Sheets

### 4.1 Subir la plantilla

1. Sube a Google Drive:

```text
data/base-datos-demo.xlsx
```

2. Ábrelo con Google Sheets.
3. Conserva exactamente estos nombres de hojas:

```text
Citas
Disponibilidad
Pacientes
Bitacora
```

4. Copia el ID del documento.

El ID aparece en una URL con estructura similar a:

```text
https://docs.google.com/spreadsheets/d/ID_DEL_DOCUMENTO/edit
```

Utiliza únicamente la parte `ID_DEL_DOCUMENTO`.

### 4.2 Hoja `Citas`

Conserva estos encabezados:

| Columna | Encabezado |
|---|---|
| A | `ID_Cita` |
| B | `Fecha` |
| C | `Hora` |
| D | `ID_Paciente` |
| E | `Paciente` |
| F | `Doctor` |
| G | `Especialidad` |
| H | `Estado` |
| I | `Canal` |
| J | `Notas` |
| K | `Calendar_Event_ID` |
| L | `Chat_ID` |
| M | `Recordatorio_30min` |

Formatos recomendados:

```text
Fecha: YYYY-MM-DD
Hora: HH:mm
```

La hora debe mostrarse, por ejemplo, como:

```text
09:00
```

y no como:

```text
9:00
```

### 4.3 Hoja `Disponibilidad`

Conserva estos encabezados:

| Columna | Encabezado |
|---|---|
| A | `Fecha` |
| B | `Hora` |
| C | `Doctor` |
| D | `Especialidad` |
| E | `Disponible` |
| F | `ID_Cita` |

Reglas:

```text
Horario libre:
Disponible = Sí
ID_Cita = vacío
```

```text
Horario ocupado:
Disponible = No
ID_Cita = CITA-...
```

Usa exactamente:

```text
Sí
No
```

La búsqueda de disponibilidad distingue el texto configurado.

### 4.4 Hoja `Pacientes`

Conserva estos encabezados:

| Columna | Encabezado |
|---|---|
| A | `ID_Paciente` |
| B | `Nombre` |
| C | `Telefono` |
| D | `Correo` |
| E | `Fecha_Nacimiento` |
| F | `Notas` |
| G | `Usuario_Telegram` |
| H | `Chat_ID` |
| I | `Fecha_Registro` |

### 4.5 Hoja `Bitacora`

Conserva estos encabezados:

| Columna | Encabezado |
|---|---|
| A | `Timestamp` |
| B | `Usuario` |
| C | `Pregunta` |
| D | `Respuesta` |
| E | `Accion` |
| F | `Resultado` |

## 5. Configurar Google Sheets en Make

En cada módulo de Google Sheets:

1. Selecciona tu conexión de Google.
2. Selecciona el archivo de Google Sheets.
3. Verifica que el nombre de la hoja coincida.
4. Confirma que la opción de encabezados esté activada.
5. Revisa que las columnas aparezcan en el mismo orden.

Debes configurar todos los módulos de:

- búsqueda de filas;
- creación de filas;
- actualización de filas;
- registro en bitácora.

Si un módulo muestra:

```text
CONFIGURAR_SPREADSHEET_ID
```

selecciona nuevamente el archivo desde la interfaz de Make.

## 6. Configurar Telegram

### 6.1 Crear el bot

1. Abre Telegram.
2. Habla con `@BotFather`.
3. Ejecuta:

```text
/newbot
```

4. Define el nombre y usuario del bot.
5. Copia el token proporcionado.
6. En Make crea una conexión de Telegram con ese token.

No publiques el token en GitHub, capturas o documentación.

### 6.2 Configurar `Watch Updates`

En el primer módulo del escenario principal:

1. Abre `Telegram Bot — Watch Updates`.
2. Crea un webhook nuevo.
3. Selecciona la conexión del bot.
4. Guarda el módulo.
5. Ejecuta `Run once`.
6. Envía un mensaje al bot para verificar que Make reciba el evento.

### 6.3 Configurar respuestas

En todos los módulos que responden al usuario, conserva:

```text
{{1.message.chat.id}}
```

Esto permite responder al mismo chat que originó el mensaje.

### 6.4 Configurar el grupo interno

Crea un grupo privado para recibir:

- urgencias;
- solicitudes de atención humana;
- casos delicados.

Agrega el bot al grupo y obtén el Chat ID.

Sustituye:

```text
CONFIGURAR_CHAT_ID_EQUIPO
```

por el Chat ID real dentro del módulo de alerta interna.

Los Chat ID de grupo normalmente comienzan con:

```text
-
```

No publiques el Chat ID real.

## 7. Configurar OpenAI

### 7.1 Conexión

En el módulo de OpenAI:

1. Selecciona o crea tu conexión.
2. Configura la autenticación solicitada por Make.
3. Selecciona un modelo compatible con salida de texto estructurado.
4. Conserva el prompt actualizado.

El prompt completo se encuentra en:

```text
docs/prompt-openai.md
```

### 7.2 Salida esperada

El modelo debe devolver únicamente JSON válido con esta estructura:

```json
{
  "intencion": "",
  "nombre": "",
  "telefono": "",
  "correo": "",
  "fecha_nacimiento": "",
  "fecha_solicitada": "",
  "hora_solicitada": "",
  "fecha_original": "",
  "hora_original": "",
  "nueva_fecha": "",
  "nueva_hora": "",
  "especialidad": "",
  "motivo": "",
  "datos_faltantes": [],
  "requiere_humano": "",
  "prioridad": "",
  "respuesta_usuario": ""
}
```

No debe incluir:

- Markdown;
- bloques de código;
- comentarios;
- texto antes o después del JSON.

## 8. Configurar `Parse JSON`

El módulo `Parse JSON` debe conservar una estructura con estos campos:

```text
intencion
nombre
telefono
correo
fecha_nacimiento
fecha_solicitada
hora_solicitada
fecha_original
hora_original
nueva_fecha
nueva_hora
especialidad
motivo
datos_faltantes
requiere_humano
prioridad
respuesta_usuario
```

Tipos recomendados:

| Campo | Tipo |
|---|---|
| Campos de texto | Text |
| `datos_faltantes` | Array de Text |

Si al importar el blueprint Make no reconoce la estructura:

1. Abre `Parse JSON`.
2. Crea una estructura nueva.
3. Usa un JSON de ejemplo con todos los campos.
4. Guarda.
5. Revisa los filtros y mapeos que dependen del módulo.

## 9. Configurar Google Calendar

### 9.1 Crear un calendario de demostración

Crea un calendario independiente, por ejemplo:

```text
MediCitas IA - Calendario de Demo
```

No utilices un calendario personal con eventos reales.

### 9.2 Obtener el ID

En Google Calendar:

1. Abre la configuración del calendario.
2. Entra a `Integrar calendario`.
3. Copia `ID del calendario`.

Sustituye:

```text
CONFIGURAR_CALENDAR_ID
```

en todos los módulos de Calendar.

### 9.3 Módulos que deben revisarse

Configura la misma conexión y calendario en los módulos que:

- crean eventos;
- actualizan eventos;
- eliminan eventos.

### 9.4 Zona horaria

Configura:

```text
America/Mexico_City
```

en:

- Make;
- Google Calendar;
- Google Sheets, cuando corresponda.

### 9.5 Campos importantes

Al crear una cita:

```text
Calendar_Event_ID
```

debe guardar el ID devuelto por Google Calendar.

Al cancelar:

- si existe `Calendar_Event_ID`, se elimina el evento;
- si está vacío, la cancelación debe continuar sin intentar eliminarlo.

Al reagendar:

- si existe `Calendar_Event_ID`, se actualiza;
- si está vacío, se crea un evento nuevo y se guarda el ID.

## 10. Configuración del escenario principal

### 10.1 Procesamiento secuencial

Activa:

```text
Process data in order = Yes
```

Esto reduce el riesgo de que dos ejecuciones ocupen el mismo horario al mismo tiempo.

### 10.2 Programación

El escenario principal usa un webhook instantáneo de Telegram. Debe quedar activado para recibir mensajes.

### 10.3 Variables de control

Verifica que estas variables comiencen en `no`:

```text
horario_agendar_encontrado
paciente_encontrado
cita_cancelar_encontrada
cita_reagendar_encontrada
horario_reagendar_encontrado
```

Solo deben cambiar a `si` después de que una búsqueda devuelva una fila real.

### 10.4 Filtros importantes

#### Horario realmente encontrado

Antes de cambiar `horario_agendar_encontrado` a `si`, verifica:

```text
Row number existe
Disponible = Sí
```

#### Paciente realmente encontrado

Antes de cambiar `paciente_encontrado` a `si`, verifica:

```text
ID_Paciente existe
ID_Paciente no está vacío
```

#### Cita realmente encontrada para cancelar

Antes de cambiar `cita_cancelar_encontrada` a `si`, verifica:

```text
Row number existe
ID_Cita no está vacío
```

#### Cita realmente encontrada para reagendar

Aplica la misma validación:

```text
Row number existe
ID_Cita no está vacío
```

#### Nuevo horario de reagendado

Antes de cambiar `horario_reagendar_encontrado` a `si`, verifica:

```text
Row number existe
Disponible = Sí
```

## 11. Configurar el escenario de recordatorios

Importa:

```text
blueprints/recordatorio-citas-sanitizado.blueprint.json
```

Configura:

- conexión de Google Sheets;
- archivo de Sheets;
- conexión de Telegram;
- frecuencia de ejecución;
- zona horaria.

El escenario debe buscar citas con:

```text
Estado = Confirmada
```

o:

```text
Estado = Reagendada
```

y:

```text
Recordatorio_30min = Pendiente
```

Después de enviar el mensaje debe actualizar:

```text
Recordatorio_30min = Enviado
```

La ventana de tiempo configurada debe coincidir con lo que se anuncia al usuario. Si el mensaje dice “30 minutos antes”, el filtro debe usar aproximadamente esa ventana.

## 12. Datos de demostración

Usa pacientes y citas ficticias.

Ejemplo de paciente:

```text
Nombre: Ana Prueba
Teléfono: 5500000001
Correo: ana.prueba@example.com
Fecha de nacimiento: 1998-04-15
```

No utilices:

- nombres reales;
- teléfonos reales;
- correos personales;
- fechas de nacimiento reales;
- información clínica real.

## 13. Primera prueba

Ejecuta `Run once` y envía:

```text
¿Cuánto cuesta la consulta general?
```

Resultado esperado:

```text
El costo de la consulta general es de $500 MXN.
```

Después prueba un agendado completo:

```text
Hola, soy Ana Prueba. Mi teléfono es 5500000001, mi correo es ana.prueba@example.com y nací el 15 de abril de 1998. Quiero una cita de medicina general el 13 de julio de 2026 a las 9:00 am por consulta general.
```

Comprueba:

- paciente creado;
- cita creada;
- disponibilidad ocupada;
- evento creado;
- ID del evento guardado;
- respuesta por Telegram;
- registro en bitácora.

La lista completa está en:

```text
docs/casos-de-prueba.md
```

## 14. Solución de problemas

### El bot no responde

Revisa:

- escenario activado;
- webhook configurado;
- `Run once`;
- conexión de Telegram;
- filtros del router;
- salida de `Parse JSON`.

### `Parse JSON` falla

Revisa que OpenAI devuelva solo JSON y que todos los campos existan.

### Se ejecuta la ruta equivocada

Abre la salida del módulo OpenAI y confirma:

```text
intencion
requiere_humano
prioridad
datos_faltantes
```

### No encuentra un horario que sí existe

Comprueba:

- fecha en `YYYY-MM-DD`;
- hora en `HH:mm`;
- especialidad exacta;
- `Disponible = Sí`;
- que el módulo apunte al archivo correcto.

### No encuentra una cita a las 9:00

Comprueba que la hoja muestre:

```text
09:00
```

y no:

```text
9:00
```

### Se cambia una variable a `si` sin resultados

Revisa que exista un filtro entre la búsqueda y el módulo `Set variable`.

### No cancela o reagenda

Comprueba:

- nombre;
- fecha;
- hora;
- Chat ID;
- estado de la cita;
- formato de hora;
- `Calendar_Event_ID`.

### Duplica pacientes

Comprueba que la búsqueda de pacientes use:

```text
Nombre
Chat_ID
```

y que el filtro `Paciente realmente encontrado` valide un `ID_Paciente` no vacío.

## 15. Seguridad antes de publicar

Antes de subir cambios a GitHub, verifica que el blueprint no contenga:

```text
ID real de Google Sheets
ID real de Google Calendar
Chat ID real del grupo
correo personal
IDs de conexiones
webhooks activos
token de Telegram
API key de OpenAI
datos reales de pacientes
```

Los valores públicos deben ser:

```text
CONFIGURAR_SPREADSHEET_ID
CONFIGURAR_CALENDAR_ID
CONFIGURAR_CHAT_ID_EQUIPO
```

y:

```json
"__IMTCONN__": 0
```

```json
"__IMTHOOK__": 0
```

No subas capturas que muestren:

- barra del navegador con IDs;
- correo de conexión;
- Chat ID;
- token;
- datos personales.

## 16. Activación final

Después de completar las pruebas:

1. Desactiva `Run once`.
2. Activa el escenario principal.
3. Activa el escenario de recordatorios con la frecuencia elegida.
4. Verifica una última vez la zona horaria.
5. Confirma que todas las conexiones pertenezcan a cuentas de demostración.
6. Mantén restringidos Google Sheets y Google Calendar.
