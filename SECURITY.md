# Seguridad y privacidad

## 1. Objetivo

Este documento describe las medidas utilizadas para reducir la exposición de credenciales, identificadores privados y datos personales dentro del repositorio público de MediCitas IA.

El proyecto es una demostración académica y no debe utilizarse con información real de pacientes.

## 2. Alcance

Estas medidas aplican a:

- blueprints de Make;
- archivos de configuración;
- hojas de cálculo;
- capturas de pantalla;
- documentación;
- historial de Git;
- enlaces públicos del proyecto;
- datos de prueba.

## 3. Información que no debe publicarse

No se deben subir al repositorio:

- tokens de bots de Telegram;
- claves de API de OpenAI;
- credenciales OAuth;
- archivos de credenciales de Google;
- contraseñas;
- URLs privadas de webhooks;
- IDs de conexiones de Make;
- IDs reales de Google Sheets;
- IDs reales de Google Calendar;
- Chat IDs de grupos privados;
- correos personales usados en conexiones;
- nombres, teléfonos o correos reales de pacientes;
- fechas de nacimiento reales;
- información clínica;
- capturas con datos privados visibles.

## 4. Blueprints públicos

Los blueprints publicados deben ser copias sanitizadas.

Valores esperados:

```json
"__IMTCONN__": 0
```

```json
"__IMTHOOK__": 0
```

Marcadores utilizados:

```text
CONFIGURAR_SPREADSHEET_ID
CONFIGURAR_CALENDAR_ID
CONFIGURAR_CHAT_ID_EQUIPO
```

El blueprint público no debe funcionar inmediatamente después de importarse. Cada persona debe configurar sus propias conexiones y recursos.

## 5. Datos de demostración

La base pública debe contener únicamente datos ficticios.

Permitido:

```text
Ana Prueba
5500000001
ana.prueba@example.com
1998-04-15
```

No permitido:

- datos de pacientes reales;
- información de contactos personales;
- Chat IDs reales;
- eventos de Calendar reales;
- identificadores internos obtenidos durante pruebas privadas.

Las hojas `Citas`, `Pacientes` y `Bitacora` deben publicarse vacías o con ejemplos completamente ficticios.

## 6. Permisos de Google

Los recursos reales utilizados durante el desarrollo deben mantenerse restringidos.

### Google Sheets

Usar:

```text
Acceso general: Restringido
```

Evitar:

```text
Cualquier persona con el enlace
```

### Google Calendar

No activar acceso público para calendarios que contengan eventos reales.

Se recomienda usar un calendario independiente de demostración.

## 7. Telegram

El token del bot es una credencial crítica.

No debe aparecer en:

- blueprints;
- capturas;
- mensajes de commit;
- documentación;
- archivos `.env` publicados;
- historial del repositorio.

El Chat ID de un grupo no autoriza acceso por sí solo, pero tampoco debe publicarse porque revela información interna del sistema.

## 8. OpenAI

Las claves de API deben configurarse directamente en Make o en un gestor seguro.

No deben guardarse en:

```text
README.md
docs/
blueprints/
data/
.env
capturas de pantalla
```

El archivo `.env` real debe permanecer fuera del repositorio.

Puede publicarse un archivo de ejemplo sin valores reales:

```text
.env.example
```

## 9. Archivos ignorados por Git

El `.gitignore` debe excluir como mínimo:

```gitignore
# macOS
.DS_Store

# Variables y credenciales
.env
.env.*
!.env.example
*.pem
*.key
credentials.json
service-account*.json

# Blueprints privados
*.private.blueprint.json
blueprints/private/

# Datos privados
data/private/
data/raw/
backups/

# Archivos temporales
*.tmp
*.log
~$*
```

`.gitignore` no elimina archivos que ya fueron registrados por Git.

## 10. Lista de revisión antes de publicar

Antes de cada commit o actualización pública, revisar:

- [ ] No hay tokens de Telegram.
- [ ] No hay claves de OpenAI.
- [ ] No hay credenciales de Google.
- [ ] `__IMTCONN__` tiene valor `0`.
- [ ] `__IMTHOOK__` tiene valor `0`.
- [ ] El ID de Sheets fue reemplazado.
- [ ] El ID de Calendar fue reemplazado.
- [ ] El Chat ID del grupo fue reemplazado.
- [ ] No hay correos personales.
- [ ] No hay datos reales de pacientes.
- [ ] Las capturas no muestran barras de navegador con IDs.
- [ ] La base de datos contiene únicamente datos ficticios.
- [ ] Los archivos privados están incluidos en `.gitignore`.
- [ ] El historial de Git no contiene secretos anteriores.

## 11. Qué hacer si se publica una credencial

Si un token, clave o credencial aparece en GitHub:

1. Revoca o regenera inmediatamente la credencial.
2. Actualiza la conexión correspondiente en Make.
3. Desactiva temporalmente el escenario cuando sea necesario.
4. Elimina el archivo del repositorio.
5. Limpia el secreto del historial de Git.
6. Revisa commits, ramas, etiquetas y pull requests.
7. Comprueba registros de uso y accesos no reconocidos.
8. Ejecuta nuevamente la revisión de seguridad.
9. Publica únicamente una copia sanitizada.

Eliminar el secreto del último commit no es suficiente si permanece en commits anteriores.

## 12. Limpieza del historial

Cuando un secreto ya fue confirmado en el historial, debe eliminarse con una herramienta apropiada, por ejemplo:

```text
git filter-repo
```

Después se debe forzar la actualización del repositorio remoto y pedir a otros colaboradores que vuelvan a clonar el proyecto.

La credencial expuesta debe revocarse incluso después de limpiar el historial.

## 13. Reporte de vulnerabilidades

No publiques tokens, credenciales ni detalles explotables dentro de un issue público.

Para reportar un problema:

1. Utiliza un canal privado del responsable del repositorio.
2. Describe el archivo o componente afectado.
3. Explica el impacto potencial.
4. No incluyas credenciales completas.
5. Espera confirmación antes de divulgar públicamente el detalle.

## 14. Dependencias externas

MediCitas IA depende de:

- Make;
- OpenAI;
- Telegram;
- Google Sheets;
- Google Calendar.

Cada servicio administra sus propios controles de autenticación, disponibilidad y permisos. La seguridad completa del sistema depende también de la configuración de esas cuentas.

## 15. Limitaciones

Este repositorio no acredita:

- cumplimiento normativo clínico;
- cumplimiento certificado de privacidad;
- cifrado especializado de expedientes;
- autenticación de pacientes;
- control de acceso por roles;
- auditoría certificada;
- disponibilidad para producción.

No debe utilizarse como sistema clínico ni como repositorio de información médica real.
