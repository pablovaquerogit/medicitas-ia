# Aviso de alcance y uso responsable

## 1. Naturaleza del proyecto

MediCitas IA es un prototipo académico y de demostración orientado a mostrar la integración entre Telegram, Make, OpenAI, Google Sheets y Google Calendar para automatizar tareas administrativas relacionadas con citas médicas.

El proyecto no está diseñado ni validado para operar como sistema clínico, expediente médico electrónico, plataforma de telemedicina ni servicio de emergencias.

## 2. No es un dispositivo médico

MediCitas IA:

- no realiza diagnósticos;
- no interpreta síntomas;
- no recomienda medicamentos;
- no prescribe tratamientos;
- no indica dosis;
- no reemplaza la valoración de un profesional de la salud;
- no sustituye los servicios de emergencia.

Las respuestas del asistente se limitan a funciones administrativas como:

- agendar citas;
- cancelar citas;
- reagendar citas;
- consultar horarios;
- consultar ubicación;
- consultar costos;
- canalizar solicitudes a una persona.

## 3. Emergencias y situaciones delicadas

Ante una emergencia, síntomas graves o una situación potencialmente peligrosa, no debe utilizarse este sistema como medio principal de atención.

La automatización únicamente puede marcar el caso para atención humana y enviar una alerta interna. Esto no garantiza una respuesta inmediata ni sustituye contactar directamente a los servicios de emergencia o a un profesional de la salud.

## 4. Uso exclusivo con datos ficticios

El repositorio público, los blueprints, la base de datos de demostración y las capturas deben utilizar exclusivamente información ficticia.

No deben almacenarse ni publicarse:

- nombres reales de pacientes;
- teléfonos reales;
- correos personales;
- fechas de nacimiento reales;
- Chat IDs privados;
- información clínica;
- antecedentes médicos;
- resultados de estudios;
- diagnósticos;
- identificadores de cuentas;
- credenciales o tokens.

## 5. No apto para producción

La versión publicada no incluye todos los controles necesarios para un entorno real.

Una implementación de producción requeriría, entre otros:

- autenticación de usuarios;
- autorización y control de acceso;
- cifrado adecuado;
- gestión segura de secretos;
- consentimiento;
- auditoría;
- respaldo y recuperación;
- monitoreo;
- manejo robusto de errores;
- alta disponibilidad;
- protección contra abuso;
- cumplimiento legal y normativo aplicable;
- evaluación profesional de seguridad y privacidad.

## 6. Dependencia de servicios externos

El funcionamiento depende de servicios de terceros:

- Make;
- OpenAI;
- Telegram;
- Google Sheets;
- Google Calendar.

La disponibilidad, privacidad, límites de uso, costos y condiciones de esos servicios son administrados por sus respectivos proveedores.

El proyecto puede dejar de funcionar correctamente si alguno de esos servicios cambia su API, permisos, límites, políticas o comportamiento.

## 7. Resultados generados por IA

OpenAI se utiliza para clasificar intenciones y extraer información estructurada.

Como cualquier sistema basado en IA, puede:

- interpretar incorrectamente un mensaje;
- omitir información;
- devolver datos incompletos;
- clasificar una intención de forma errónea;
- producir una salida que no cumpla el formato esperado.

Por esta razón, el proyecto utiliza filtros, validaciones y casos de prueba, pero no puede garantizar resultados perfectos.

## 8. Exactitud de la información

La información administrativa incluida en la demostración —como horarios, ubicación, costos, doctores y disponibilidad— es ficticia o configurada únicamente para fines de prueba.

No debe asumirse que representa información real de una clínica, hospital o profesional de la salud.

## 9. Responsabilidad de quien lo implemente

Quien importe, modifique o ejecute este proyecto es responsable de:

- configurar sus propias conexiones;
- proteger sus credenciales;
- validar los permisos de sus cuentas;
- revisar la exactitud del flujo;
- realizar pruebas suficientes;
- utilizar datos ficticios;
- verificar el cumplimiento legal aplicable;
- impedir el uso clínico no autorizado.

La publicación del código y los blueprints no implica que el proyecto sea seguro o adecuado para uso real.

## 10. Sin garantía de disponibilidad

El proyecto se proporciona con fines educativos y de portafolio.

No se garantiza:

- funcionamiento continuo;
- ausencia de errores;
- compatibilidad futura;
- integridad de los datos;
- recuperación ante fallos;
- respuesta inmediata;
- disponibilidad de los servicios externos.

## 11. Privacidad y seguridad

Los archivos públicos fueron preparados para no incluir credenciales activas ni identificadores privados.

Aun así, antes de publicar una nueva versión debe realizarse una revisión manual del blueprint, la documentación, las capturas y el historial de Git.

Consulta:

```text
SECURITY.md
```

para conocer las recomendaciones de seguridad del repositorio.

## 12. Uso académico

Este proyecto demuestra conocimientos de:

- automatización no-code;
- integración de APIs y servicios;
- procesamiento de lenguaje natural;
- diseño de flujos;
- validación de reglas de negocio;
- documentación;
- pruebas;
- seguridad básica en repositorios públicos.

No constituye una certificación médica, legal, técnica ni de cumplimiento normativo.
