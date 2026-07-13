# Configuración recomendada en GitHub

## 1. Nombre del repositorio

```text
medicitas-ia
```

## 2. Descripción corta

```text
Automatización para gestionar citas médicas por Telegram con Make, OpenAI, Google Sheets y Google Calendar.
```

## 3. Sitio web

Usa como sitio web del repositorio el escenario público de Make:

```text
https://us2.make.com/public/shared-scenario/fYTnGTGaoIk/medi-citas-ia-gestion-de-citas-medicas
```

## 4. Topics recomendados

Agrega estos temas en la sección **About** del repositorio:

```text
make-com
openai
telegram-bot
google-sheets
google-calendar
automation
artificial-intelligence
no-code
nlp
healthcare-demo
appointment-scheduling
workflow-automation
```

## 5. Visibilidad

La visibilidad recomendada es:

```text
Public
```

Siempre que el repositorio contenga únicamente:

- blueprints sanitizados;
- datos ficticios;
- capturas sin información privada;
- documentación sin credenciales;
- archivos de configuración de ejemplo.

## 6. Imagen social del repositorio

Usa como imagen principal:

```text
assets/flujo-principal-make.png
```

En GitHub:

1. Abre `Settings`.
2. Entra a `General`.
3. Busca `Social preview`.
4. Sube la imagen del flujo principal.

Esto mejora la vista previa cuando el repositorio se comparte en redes o mensajes.

## 7. Repositorio destacado

Fija el repositorio en tu perfil de GitHub.

En tu perfil:

1. Selecciona `Customize your pins`.
2. Busca `medicitas-ia`.
3. Márcalo como repositorio destacado.

## 8. Rama principal

Usa:

```text
main
```

como rama predeterminada.

Para cambios grandes futuros, se recomienda trabajar en ramas como:

```text
feature/nueva-funcionalidad
docs/actualizar-documentacion
fix/cancelacion-citas
```

y después crear un pull request.

## 9. Convención de commits

Usa mensajes cortos y claros.

Ejemplos:

```text
feat: actualizar blueprint principal
fix: corregir validación de citas inexistentes
docs: actualizar documentación técnica
data: limpiar base de datos de demostración
chore: actualizar archivos del repositorio
```

Prefijos recomendados:

| Prefijo | Uso |
|---|---|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección |
| `docs` | Documentación |
| `data` | Archivos de datos |
| `chore` | Mantenimiento |
| `refactor` | Reorganización interna |
| `test` | Pruebas |

## 10. Releases

Cuando finalices esta actualización, crea una release:

```text
v1.0.0
```

Título recomendado:

```text
MediCitas IA v1.0.0
```

Descripción sugerida:

```text
Primera versión documentada y sanitizada de MediCitas IA.

Incluye:
- automatización principal en Make;
- escenario de recordatorios;
- blueprints públicos sanitizados;
- base de datos de demostración;
- documentación técnica;
- guía de configuración;
- prompt de OpenAI;
- 30 casos de prueba;
- políticas de seguridad y uso responsable.
```

## 11. Archivos principales

El repositorio debe conservar esta estructura:

```text
medicitas-ia/
├── assets/
│   ├── flujo-principal-make.png
│   └── recordatorio-citas-make.png
├── blueprints/
│   ├── medicitas-ia-sanitizado.blueprint.json
│   └── recordatorio-citas-sanitizado.blueprint.json
├── data/
│   └── base-datos-demo.xlsx
├── docs/
│   ├── casos-de-prueba.md
│   ├── configuracion.md
│   ├── documentacion-tecnica.md
│   └── prompt-openai.md
├── .gitignore
├── CHANGELOG.md
├── DISCLAIMER.md
├── GITHUB.md
├── SECURITY.md
└── README.md
```

## 12. Sección About

Configura la sección **About** con:

**Descripción**

```text
Automatización para gestionar citas médicas por Telegram con Make, OpenAI, Google Sheets y Google Calendar.
```

**Website**

```text
https://us2.make.com/public/shared-scenario/fYTnGTGaoIk/medi-citas-ia-gestion-de-citas-medicas
```

**Topics**

Usa los temas recomendados de este documento.

## 13. Funciones opcionales de GitHub

Puedes activar:

- `Issues`, para registrar mejoras o errores;
- `Discussions`, si deseas recibir preguntas;
- `Projects`, si quieres organizar tareas;
- `Releases`, para publicar versiones.

Para un proyecto de portafolio, `Issues` y `Releases` son las más útiles.

## 14. Protección de la rama

Cuando el proyecto esté estable, puedes proteger `main`:

1. Abre `Settings`.
2. Entra a `Branches`.
3. Crea una regla para `main`.
4. Activa revisión mediante pull request para cambios futuros.

No es obligatorio para un proyecto personal, pero ayuda a mantener un historial más ordenado.

## 15. Revisión antes de publicar

Antes de cada actualización pública, verifica:

- [ ] El blueprint principal está sanitizado.
- [ ] El blueprint de recordatorios está sanitizado.
- [ ] No hay credenciales.
- [ ] No hay IDs privados.
- [ ] No hay datos reales.
- [ ] Las capturas no muestran información sensible.
- [ ] La documentación coincide con la automatización.
- [ ] Los enlaces funcionan.
- [ ] La base de datos de demostración está limpia.
- [ ] El README muestra las imágenes correctas.

## 16. Recomendación para el portafolio

En la descripción del proyecto puedes destacar:

- automatización no-code;
- integración de múltiples servicios;
- procesamiento de lenguaje natural;
- reglas de negocio;
- validación de errores;
- sincronización con Calendar;
- seguridad y sanitización;
- documentación técnica;
- pruebas completas.

Esto permite que el repositorio demuestre tanto la parte técnica como la organización del proyecto.
