# Configuración del proyecto

## 1. Importar los blueprints

En Make, crea dos escenarios e importa:

- `blueprints/medicitas-ia-sanitizado.blueprint.json`
- `blueprints/recordatorio-citas-sanitizado.blueprint.json`

Los archivos contienen marcadores que deben configurarse manualmente.

## 2. Preparar Google Sheets

1. Sube `data/base-datos-demo.xlsx` a Google Drive.
2. Ábrelo con Google Sheets.
3. Conserva exactamente estos nombres de hojas:
   - `Citas`
   - `Disponibilidad`
   - `Pacientes`
   - `Bitacora`
4. Copia el identificador de la hoja y úsalo en todos los módulos de Google Sheets.

## 3. Configurar conexiones

Reconecta cada módulo con tus propias cuentas:

- Telegram
- OpenAI
- Google Sheets
- Google Calendar

No publiques tokens, claves de API ni archivos de credenciales.

## 4. Sustituir marcadores

Busca y reemplaza los siguientes valores en los módulos:

| Marcador | Valor requerido |
|---|---|
| `CONFIGURAR_SPREADSHEET_ID` | ID del archivo de Google Sheets |
| `CONFIGURAR_CALENDAR_ID` | ID del calendario de Google |
| `CONFIGURAR_CHAT_ID_EQUIPO` | Chat o grupo que recibirá alertas humanas |

También es necesario seleccionar nuevamente las conexiones que aparecen con valor `0`.

## 5. Telegram

1. Crea o selecciona un bot.
2. Configura el webhook `Watch Updates`.
3. Revisa que todos los módulos de respuesta utilicen el `chat.id` recibido.
4. Para las alertas internas, configura un grupo independiente.

## 6. OpenAI

El prompt completo está en `docs/prompt-openai.md`.

La salida esperada es JSON válido. El módulo `Parse JSON` debe conservar la misma estructura de campos descrita en el prompt.

## 7. Google Calendar

- Usa un calendario de demostración separado.
- Configura la zona horaria `America/Mexico_City`.
- Comprueba que el ID del evento se guarde en `Calendar_Event_ID`.
- Verifica creación, actualización y eliminación con citas nuevas.

## 8. Escenario de recordatorios

Configura la frecuencia del escenario y asegúrate de que:

- solo procese citas con estado `Confirmada` o `Reagendada`;
- el campo `Recordatorio_30min` esté en `Pendiente`;
- la ventana de tiempo del filtro coincida con el texto enviado al usuario;
- después de enviar, el estado cambie a `Enviado`.

## 9. Pruebas

Realiza primero las pruebas con datos ficticios. No uses información de pacientes reales en un entorno de demostración.
