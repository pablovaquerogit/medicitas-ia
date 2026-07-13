# Historial de cambios

Todos los cambios importantes de MediCitas IA se documentan en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y el proyecto utiliza versionado semántico.

## [Sin publicar]

### Pendiente

- Incorporar mejoras futuras del escenario.
- Registrar nuevas correcciones y funcionalidades antes de crear otra versión.

## [1.0.0] - 2026-07-13

Primera versión pública, documentada y sanitizada de MediCitas IA.

### Agregado

- Automatización principal para gestionar citas médicas mediante Telegram.
- Clasificación de intenciones y extracción de datos estructurados con OpenAI.
- Procesamiento de respuestas mediante `Parse JSON`.
- Rutas para:
  - agendar citas;
  - cancelar citas;
  - reagendar citas;
  - consultar horarios;
  - consultar ubicación;
  - consultar costos;
  - solicitar atención humana;
  - detectar urgencias;
  - solicitar datos faltantes.
- Registro de pacientes nuevos.
- Reutilización de pacientes existentes.
- Actualización de pacientes existentes con información incompleta.
- Validación de disponibilidad antes de crear una cita.
- Sincronización con Google Calendar.
- Creación, actualización y eliminación de eventos.
- Compatibilidad con citas sin `Calendar_Event_ID`.
- Escenario secundario de recordatorios por Telegram.
- Registro de operaciones en la hoja `Bitacora`.
- Procesamiento secuencial para reducir conflictos de disponibilidad.
- Variables de control para validar resultados de búsquedas:
  - `horario_agendar_encontrado`;
  - `paciente_encontrado`;
  - `cita_cancelar_encontrada`;
  - `cita_reagendar_encontrada`;
  - `horario_reagendar_encontrado`.
- Blueprints públicos sanitizados.
- Plantilla limpia de Google Sheets con las hojas:
  - `Citas`;
  - `Disponibilidad`;
  - `Pacientes`;
  - `Bitacora`.
- Formatos de fecha `YYYY-MM-DD`.
- Formatos de hora `HH:mm`.
- Validaciones y listas desplegables en la base de datos.
- Formato condicional para disponibilidad y estados de citas.
- Imagen actualizada del flujo principal de Make.
- Imagen actualizada del escenario de recordatorios.
- Enlace al escenario público de Make.
- Documentación completa del prompt de OpenAI.
- Guía de configuración.
- Documentación técnica.
- Batería de 30 casos de prueba.
- Política de seguridad y privacidad.
- Aviso de alcance y uso responsable.
- Recomendaciones de configuración para GitHub.
- Reglas ampliadas en `.gitignore`.
- README actualizado para presentación en portafolio.

### Cambiado

- Se amplió el prompt de OpenAI para normalizar:
  - nombres;
  - fechas;
  - horas;
  - especialidades;
  - datos faltantes;
  - prioridad;
  - atención humana.
- Se reorganizó el flujo de pacientes existentes y nuevos.
- Se mejoraron los filtros después de los módulos `Search Rows`.
- Se actualizó la documentación para reflejar el flujo real del escenario.
- Se reemplazó la base de datos de pruebas por una plantilla pública limpia.
- Se actualizaron las capturas de la carpeta `assets/`.
- Se amplió el README con arquitectura, seguridad, pruebas y puesta en marcha.
- Se mejoró la configuración recomendada para publicación en GitHub.

### Corregido

- Falso positivo al buscar un horario ocupado.
- Cambio incorrecto de variables a `si` cuando una búsqueda devolvía cero filas.
- Cancelación incorrecta de citas inexistentes.
- Reagendado incorrecto cuando no se encontraba la cita original.
- Búsqueda de horas con formato `9:00` en lugar de `09:00`.
- Duplicación de pacientes existentes.
- Falta de actualización de pacientes incompletos.
- Creación de citas sin datos administrativos completos.
- Manejo de cancelaciones sin evento de Google Calendar.
- Manejo de reagendados sin evento de Google Calendar.
- Liberación del horario después de cancelar una cita.
- Conservación del mismo `ID_Cita` al reagendar.
- Ocupación del nuevo horario y liberación del horario anterior.
- Cancelación de citas con estado `Reagendada`.
- Envío repetido de recordatorios ya marcados como enviados.

### Seguridad

- Se reemplazaron conexiones y webhooks por valores `0`.
- Se sustituyeron identificadores privados por:
  - `CONFIGURAR_SPREADSHEET_ID`;
  - `CONFIGURAR_CALENDAR_ID`;
  - `CONFIGURAR_CHAT_ID_EQUIPO`.
- Se eliminaron Chat IDs reales.
- Se eliminaron identificadores reales de eventos de Calendar.
- Se eliminaron correos personales.
- Se eliminaron datos de pacientes y citas usados durante las pruebas.
- Se documentó el procedimiento para revisar y sanitizar futuras exportaciones.
- Se añadieron reglas para ignorar credenciales, archivos privados y copias sin sanitizar.

### Limitaciones conocidas

- El proyecto es una demostración académica.
- No realiza diagnósticos ni tratamientos.
- No sustituye atención profesional o servicios de emergencia.
- No está preparado para almacenar datos médicos reales.
- No incluye autenticación clínica ni cumplimiento normativo certificado.
- Depende de la disponibilidad y configuración de servicios externos.
