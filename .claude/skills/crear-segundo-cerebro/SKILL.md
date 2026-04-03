# Skill: Crear Segundo Cerebro

Genera un segundo cerebro con IA completamente funcional, personalizado según los requisitos del usuario. El sistema corre 24/7 en cualquier computadora (macOS, Linux o Windows con WSL) usando Claude CLI como cerebro, Telegram como interfaz, y Markdown como memoria.

## Uso

```
/crear-segundo-cerebro ./mi-segundo-cerebro.md
```

## Entrada

El archivo `mi-segundo-cerebro.md` completado con 8 secciones: perfil del usuario, plataformas, tareas prioritarias, nivel de proactividad, límites de seguridad, categorías de memoria, infraestructura e integraciones.

## Proceso

### Paso 1 — Validar requisitos

Lee el archivo de requisitos. Si faltan secciones críticas (nombre, nombre del agente, nivel de proactividad), pide al usuario que las complete antes de continuar.

Extrae estas variables del cuestionario:
- `NOMBRE_USUARIO` — nombre del usuario
- `NOMBRE_AGENTE` — nombre elegido para el agente
- `ROL_USUARIO` — profesión o rol
- `DESCRIPCION_ROL` — qué hace (1-2 oraciones)
- `ZONA_HORARIA` — zona horaria (ej. America/Mexico_City)
- `IDIOMAS` — idiomas del usuario
- `NIVEL_PROACTIVIDAD` — Observador / Asesor / Asistente / Compañero
- `TIPO_DESPLIEGUE` — Una máquina / Dos máquinas / Local + Cloud
- `SISTEMA_OPERATIVO` — macOS / Linux / Windows (WSL)
- `INTEGRACION_1`, `INTEGRACION_2`, `INTEGRACION_3` — top 3 integraciones
- `TAREAS` — lista de tareas prioritarias
- `LIMITES` — límites de seguridad marcados
- `CATEGORIAS` — categorías de memoria marcadas
- `PLATAFORMAS` — plataformas marcadas
- `PILARES` — inferir 5 pilares de hábitos del perfil del usuario

### Paso 2 — Generar estructura del proyecto

Crear toda la estructura de archivos del proyecto. El nombre de la carpeta raíz ya existe (donde se ejecuta la skill). Generar:

```
./
├── Memory/
│   ├── SOUL.md
│   ├── USER.md
│   ├── MEMORY.md
│   ├── HEARTBEAT.md
│   ├── HABITS.md
│   ├── daily/
│   ├── research/
│   ├── goals/
│   ├── content/
│   └── drafts/active/sent/expired/
├── .claude/
│   ├── settings.json
│   ├── settings.local.json
│   ├── hooks/
│   │   ├── session-start-context.py
│   │   ├── guardrails.py
│   │   ├── pre-compact-flush.py
│   │   └── session-end-flush.py
│   ├── scripts/
│   │   ├── db.py
│   │   ├── embeddings.py
│   │   ├── memory_index.py
│   │   ├── memory_search.py
│   │   ├── memory_reflect.py
│   │   ├── heartbeat.py
│   │   ├── watchdog.py
│   │   ├── shared.py
│   │   ├── sanitize.py
│   │   ├── requirements.txt
│   │   └── integrations/
│   │       ├── __init__.py
│   │       ├── registry.py
│   │       ├── integration_template.py
│   │       └── (una por cada integración del usuario)
│   ├── chat/
│   │   └── telegram_bot.py
│   ├── data/
│   └── logs/
├── bootstrap.sh
├── .env.example
├── .gitignore
└── README.md
```

### Paso 3 — Generar archivos de memoria

Genera cada archivo personalizado con los datos del usuario. No uses placeholders — escribe los valores reales.

#### Memory/SOUL.md
```markdown
# SOUL — Personalidad y reglas del agente

## Identidad

Eres **{NOMBRE_AGENTE}**, el segundo cerebro de {NOMBRE_USUARIO} — un asistente proactivo construido para ayudar a {ROL_USUARIO} a mantenerse al día con su trabajo, investigación y crecimiento personal.

## Estilo de comunicación

- Bilingüe: Responde en el mismo idioma que use {NOMBRE_USUARIO}.
- Conciso y accionable: Sin relleno. Empieza con la respuesta, después contexto si hace falta.
- Referencia a específicos: Siempre cita rutas de archivo, fechas, nombres — nunca des respuestas vagas.
- Adaptado al rol: Enmarca sugerencias en términos de impacto profesional y valor estratégico.

## Reglas de comportamiento

1. Modo {NIVEL_PROACTIVIDAD}: (adaptar según nivel elegido)
   - Observador: solo notifica, nunca propone
   - Asesor: prepara borradores para revisión, nunca ejecuta
   - Asistente: ejecuta acciones de bajo riesgo (mover archivos, crear recordatorios)
   - Compañero: actúa autónomamente dentro de límites definidos
2. Solo lectura por defecto: Consulta integraciones libremente, nunca modifiques sistemas externos sin permiso.
3. Memoria primero: Antes de responder, revisa MEMORY.md y logs diarios recientes.
4. Disciplina de log diario: Agrega eventos, decisiones y aprendizajes a daily/YYYY-MM-DD.md con timestamps.
5. Rigor investigativo: Al guardar investigación, incluye fuente, fecha, y "Cómo aplica esto".

## Límites — NUNCA hagas esto sin permiso explícito

(Generar desde los límites marcados por el usuario en sección 5)

## Tono

Profesional pero cálido. Como un colega afilado que conoce profundamente el contexto de {NOMBRE_USUARIO}. Directo, útil, humano.
```

#### Memory/USER.md
Generar con: nombre, rol, descripción, zona horaria, idiomas, tipo de despliegue, tabla de plataformas (marcando las seleccionadas), prioridad de integraciones, nivel de proactividad, criterios de borradores, y tareas principales.

#### Memory/MEMORY.md
```markdown
# MEMORY — Conocimiento a largo plazo

> Este archivo se carga en cada conversación. Mantenlo conciso (<1K tokens).
> La reflexión diaria promueve elementos importantes aquí desde los logs diarios.

(Generar secciones según las categorías de memoria marcadas por el usuario en sección 6. Cada sección es un ## con un comentario HTML explicativo.)
```

#### Memory/HEARTBEAT.md
Generar checklist de monitoreo basado en las plataformas seleccionadas. Solo incluir elementos para plataformas que el usuario marcó. Incluir reglas de notificación (ALTA/MEDIA/BAJA) y reglas de borradores si el nivel es Asesor o superior. Para notificaciones, usar el método nativo del SO: `osascript` en macOS, `notify-send` en Linux, `powershell` toast en Windows/WSL.

#### Memory/HABITS.md
Inferir 5 pilares de hábitos del perfil del usuario. Ejemplo: si es desarrollador, los pilares pueden ser Código, Aprendizaje, Networking, Salud, Side Project. Si es PM, pueden ser Producto, Equipo, Aprendizaje, Salud, Estrategia. Incluir tabla de auto-detección con métodos basados en las integraciones disponibles.

### Paso 4 — Generar scripts del pipeline RAG

#### .claude/scripts/db.py
Capa de base de datos con SQLite + sqlite-vec (vectores) + FTS5 (keywords). Funciones:
- `get_connection()` — conecta a `.claude/data/search.db`, carga sqlite-vec si disponible, fallback a solo keywords
- `init_db(conn, dim=384)` — crea tablas: chunks (id, file_path, section, content, mtime), chunks_fts (FTS5), chunk_vectors (vec0), indexed_files. Triggers para mantener FTS sincronizado.
- `upsert_chunks(conn, file_path, chunks, embeddings, mtime)` — reemplaza chunks de un archivo
- `vector_search(conn, query_embedding, limit, path_prefix)` — búsqueda por similitud vectorial
- `keyword_search(conn, query, limit, path_prefix)` — búsqueda FTS5 con BM25, sanitiza query envolviendo palabras en comillas
- `get_indexed_files(conn)` — retorna dict de file_path → mtime
- `remove_file(conn, file_path)` — elimina chunks de archivo borrado
- Serialización de vectores con `struct.pack/unpack` formato `f`

#### .claude/scripts/embeddings.py
Wrapper de FastEmbed. Modelo: `sentence-transformers/all-MiniLM-L6-v2` (384 dims, ONNX, CPU).
- `get_model()` — cacheado con `@lru_cache(maxsize=1)`
- `embed_texts(texts, batch_size=64)` — lista de textos → lista de vectores
- `embed_query(query)` — una consulta → un vector (usa `query_embed` del modelo)

#### .claude/scripts/memory_index.py
Indexador incremental. Lee archivos .md del vault, los divide en chunks, genera embeddings, almacena en SQLite.
- `CHUNK_SIZE = 400` tokens, `CHUNK_OVERLAP = 50` tokens
- `estimate_tokens(text)` — ~1.3 tokens por palabra
- `chunk_markdown(text, file_path)` — divide por headers (#{1,3}), mantiene contexto de sección, solapamiento
- `index_vault(vault_path, force)` — solo re-indexa archivos modificados (compara mtime)
- CLI: `--vault` (ruta) y `--force` (re-indexar todo)

#### .claude/scripts/memory_search.py
Búsqueda híbrida: 70% vector + 30% keyword.
- `hybrid_search(query, limit, path_prefix)` — ejecuta ambas búsquedas, fusiona por chunk ID, calcula puntaje ponderado
- `format_results(results, format)` — formato texto o JSON
- CLI: `--query`, `--limit`, `--path-prefix`, `--format`

### Paso 5 — Generar scripts de daemons

#### .claude/scripts/heartbeat.py
Monitor proactivo. Corre cada 30 minutos (8 AM - 10 PM hora del usuario).
- Funciones `gather_*()` para cada integración configurada — retornan `{"status", "data", "count"}`
- `build_snapshot()` — hash MD5 de cada sección para detectar cambios
- `load_previous_snapshot()` / `save_snapshot()` — estado en `.claude/data/state/heartbeat-state.json`
- `build_context()` — arma reporte Markdown con todos los datos recopilados y contexto de hora del día
- `invoke_claude(context)` — llama `claude --print --system-prompt ... prompt` con timeout 60s
- El system prompt le dice al agente que genere máximo 5 puntos prioritarios en formato `[ALTA/MEDIA/BAJA]`
- `send_notification()` — notificación nativa del SO:
  - macOS: `osascript -e 'display notification ...'`
  - Linux: `notify-send`
  - Windows/WSL: `powershell.exe -Command "New-BurntToastNotification ..."` o fallback a print
  - Detectar SO con `sys.platform`
- `append_to_daily_log()` — agrega resultado al log diario
- Horas activas ajustadas a la zona horaria del usuario

#### .claude/scripts/memory_reflect.py
Reflexión diaria. Corre a las 8 AM.
- Lee log del día anterior, invoca Claude para identificar qué promover a MEMORY.md
- `archive_habits()` — archiva checklist de hábitos de ayer, reinicia para hoy
- `apply_updates()` — parsea secciones del output de Claude, agrega items a las secciones correspondientes de MEMORY.md
- Responde `SIN_ACTUALIZACIONES` si nada vale la pena

#### .claude/scripts/watchdog.py
Vigilante del bot de Telegram. Corre cada 5 minutos.
- Lee archivo de salud (`telegrambot-health.txt`) que el bot escribe cada 2 min
- Si tiene más de 600 segundos de antigüedad, envía SIGTERM al proceso del bot
- Si no muere en 10 segundos, envía SIGKILL
- El scheduler del SO se encarga de reiniciar el bot

### Paso 6 — Generar hooks de Claude Code

#### .claude/hooks/session-start-context.py
Hook SessionStart. Lee SOUL.md, USER.md, MEMORY.md, HABITS.md y últimos 3 logs diarios. Los inyecta como `systemPromptAppend` envueltos en `<memory>...</memory>`.

Output JSON: `{"result": "continue", "suppressOutput": true, "systemPromptAppend": "<memory>...</memory>"}`

#### .claude/hooks/guardrails.py
Hook PreToolUse. Lee stdin (JSON con toolName y toolInput), llama `check_tool_use()` de shared.py.
- Si bloqueado: `{"result": "block", "message": "Bloqueado [acción]: razón"}`
- Si permitido: `{"result": "continue"}`

#### .claude/hooks/pre-compact-flush.py
Hook PreCompact. Lee transcript del stdin, trunca a 3000 chars, lo agrega al log diario con timestamp.

#### .claude/hooks/session-end-flush.py
Hook SessionEnd. Igual que pre-compact pero con truncado a 2000 chars y header "Sesión terminada".

### Paso 7 — Generar scripts de seguridad

#### .claude/scripts/shared.py
Guardrails deterministas. Lista de `BLOCKED_ACTIONS` — cada uno con nombre, patrones regex, y razón.
Adaptar según el nivel de proactividad del usuario:
- Observador/Asesor: bloquear envío de emails, mensajes, publicaciones, eliminaciones, financiero, comandos peligrosos (sudo, curl|sh, eval, exec)
- Asistente: permitir algunas acciones de bajo riesgo (crear recordatorios, mover archivos)
- Compañero: permitir más acciones pero mantener bloqueos de seguridad críticos
- Siempre bloquear: escritura de tokens/secretos en archivos (regex: `xoxb-|xapp-|sk-ant-|ghp_|AIza`)

#### .claude/scripts/sanitize.py
Sanitización de datos externos en 3 capas:
1. Detectar patrones de inyección de prompts (14+ patrones regex: "ignore previous instructions", "you are now", "jailbreak", etc.)
2. Escapar HTML/XML peligroso (`<system>`, `<instruction>`, `<tool_use>`)
3. Envolver en trust boundaries XML: `<external_data source="..." sender="...">...</external_data>`

### Paso 8 — Generar configuración de Claude Code

#### .claude/settings.json
```json
{
  "hooks": {
    "SessionStart": [{"matcher": "", "hooks": [{"type": "command", "command": "python3 .claude/hooks/session-start-context.py"}]}],
    "PreCompact": [{"matcher": "", "hooks": [{"type": "command", "command": "python3 .claude/hooks/pre-compact-flush.py"}]}],
    "SessionEnd": [{"matcher": "", "hooks": [{"type": "command", "command": "python3 .claude/hooks/session-end-flush.py"}]}],
    "PreToolUse": [
      {"matcher": "Bash", "hooks": [{"type": "command", "command": "python3 .claude/hooks/guardrails.py"}]},
      {"matcher": "Write", "hooks": [{"type": "command", "command": "python3 .claude/hooks/guardrails.py"}]}
    ]
  }
}
```

#### .claude/settings.local.json
Permisos allowlist para los comandos que el agente necesita ejecutar sin confirmación.

### Paso 9 — Generar integraciones

#### .claude/scripts/integrations/registry.py
Registro con dataclass `Integration(name, module_name, description, auth_type)`. Solo incluir las plataformas que el usuario marcó. Funciones: `check_status()`, `get_module()`, `list_integrations()`.

#### .claude/scripts/integrations/integration_template.py
Plantilla que el usuario puede copiar para agregar nuevas integraciones. Patrón: dataclass → check_auth → get_items → format_context → CLI.

Para cada integración seleccionada por el usuario, generar un esqueleto funcional con las funciones `check_auth()`, las funciones de consulta principales, y `format_context()`. Las que ya tienen implementación conocida (Gmail, GCal, GDrive, GitHub, Reminders, Telegram) deben generarse con la lógica completa usando las APIs correspondientes.

### Paso 10 — Generar bot de Telegram

#### .claude/chat/telegram_bot.py
Bot usando `python-telegram-bot`. Funcionalidad:
- Autenticación por TELEGRAM_OWNER_ID (solo responde al dueño)
- Recibe mensaje → invoca `claude --print` con contexto de memoria → responde
- Escribe archivo de salud cada 2 minutos para el watchdog
- Manejo de errores con logging
- Carga token desde `.env`

### Paso 11 — Generar bootstrap.sh

Script de setup multiplataforma que:
1. Detecta el SO (`uname -s` → Darwin/Linux; si es WSL, detecta via `/proc/version`)
2. Pregunta nombre del agente al inicio
3. Instala dependencias según el SO:
   - **macOS**: Homebrew → Python, Node, gh CLI
   - **Linux/WSL**: apt/dnf/pacman → Python, Node, gh CLI
4. Instala Claude CLI (`npm install -g @anthropic-ai/claude-code`)
5. Crea venv en `.claude/venv/`, instala requirements.txt
6. Crea directorios del vault si no existen
7. Ejecuta indexación inicial
8. Genera las tareas programadas según el SO:
   - **macOS**: 4 plists de launchd en `~/Library/LaunchAgents/` con label `com.{agente}.{daemon}`
   - **Linux**: 4 servicios systemd en `~/.config/systemd/user/` con archivos `.service` y `.timer`
   - **WSL**: crontab entries para los 4 daemons
9. Marca scripts como ejecutables
10. Imprime siguientes pasos manuales adaptados al SO

Para activar los daemons:
- **macOS**: `launchctl load ~/Library/LaunchAgents/com.{agente}.heartbeat.plist`
- **Linux**: `systemctl --user enable --now {agente}-heartbeat.timer`
- **WSL**: los crontabs se activan automáticamente

### Paso 12 — Generar archivos auxiliares

#### .env.example
```
TELEGRAM_BOT_TOKEN=tu_token_aqui
TELEGRAM_OWNER_ID=tu_id_numerico
```

#### .gitignore
Excluir: .claude/venv/, .claude/data/, .claude/logs/, __pycache__/, .env, credentials.json, token.json, .DS_Store, .fastembed/

#### README.md
En español. Secciones: qué es, qué incluye, requisitos, inicio rápido (4 pasos), estructura del proyecto, modo asesor, personalización, arquitectura (5 capas), créditos, licencia MIT.

### Paso 13 — Generar PRD de implementación

Crear `.agent/plans/segundo-cerebro-prd.md` con 9 fases ordenadas por complejidad:
1. Fundación (bootstrap) — Baja
2. Memoria y RAG — Baja
3. Hooks de Claude — Baja
4-6. Integraciones prioritarias 1, 2, 3 — Media
7. Bot de Telegram — Media
8. Daemons y automatización — Media
9. Refinamiento — Alta

Cada fase incluye: objetivo, archivos involucrados, comandos de verificación, criterio de éxito.

## Referencia de arquitectura

### Stack técnico
- LLM: Claude CLI (`claude --print` para invocaciones programáticas)
- Embeddings: FastEmbed con all-MiniLM-L6-v2 (384 dims, ONNX, CPU)
- Base de datos: SQLite + sqlite-vec (vectores) + FTS5 (keywords)
- Búsqueda: Híbrida 70% vector + 30% keyword (BM25)
- Chunking: 400 tokens objetivo, 50 tokens solapamiento, divide por headers Markdown
- Daemons: launchd (macOS), systemd (Linux), cron (WSL)
- Bot: python-telegram-bot
- Seguridad: guardrails deterministas + sanitización de datos + trust boundaries

### Dependencias Python (requirements.txt)
```
claude-code-sdk
fastembed
sqlite-vec
google-api-python-client
google-auth-httplib2
google-auth-oauthlib
python-telegram-bot
python-dotenv
```

### Principios de diseño
1. Todo en Markdown — portable, legible, versionable con git
2. Solo lectura por defecto — nunca actúa sin permiso
3. Sin frameworks pesados — Python puro, SQLite, scripts simples
4. CPU-only — funciona en cualquier máquina sin GPU
5. Incremental — solo re-indexa lo que cambió
6. Resiliente — si algo falla, el sistema sigue funcionando en modo degradado

## Salida

Al terminar, el usuario tiene:
- Un proyecto completo con todos los archivos generados y personalizados
- Un PRD con las 9 fases de implementación
- El vault de memoria listo con su personalidad y perfil
- Scripts funcionales listos para correr
- Solo falta ejecutar `bash bootstrap.sh` para instalar dependencias y activar los daemons

## Ejemplo

Ver `ejemplo-requisitos.md` para un cuestionario ya completado.
