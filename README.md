```
 ╔═══════════════════════════════════════════════════════════════╗
 ║                                                               ║
 ║    ██████╗ █████╗ ██████╗ ██╗██████╗ ███████╗                 ║
 ║   ██╔════╝██╔══██╗██╔══██╗██║██╔══██╗██╔════╝                ║
 ║   ██║     ███████║██████╔╝██║██████╔╝█████╗                   ║
 ║   ██║     ██╔══██║██╔══██╗██║██╔══██╗██╔══╝                   ║
 ║   ╚██████╗██║  ██║██║  ██║██║██████╔╝███████╗                 ║
 ║    ╚═════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝╚═════╝ ╚══════╝                 ║
 ║                                                               ║
 ║   ██████╗ ██████╗  █████╗ ██╗███╗   ██╗                      ║
 ║   ██╔══██╗██╔══██╗██╔══██╗██║████╗  ██║                      ║
 ║   ██████╔╝██████╔╝███████║██║██╔██╗ ██║                      ║
 ║   ██╔══██╗██╔══██╗██╔══██║██║██║╚██╗██║                      ║
 ║   ██████╔╝██║  ██║██║  ██║██║██║ ╚████║                      ║
 ║   ╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝╚═╝  ╚═══╝                      ║
 ║                                                               ║
 ║   ◇ tu segundo cerebro con IA — starter kit ◇                ║
 ║                                                               ║
 ╚═══════════════════════════════════════════════════════════════╝
```

<p align="center">
  <img src="caribebrain-banner.png" alt="CaribeBrain" width="600">
</p>

Un segundo cerebro con IA que corre 24/7 en tu computadora. Funciona en macOS, Linux y Windows (WSL). Usa Claude CLI como cerebro, Telegram como interfaz de chat, y archivos Markdown como memoria persistente.

## Cómo funciona

Este repo no contiene el código del segundo cerebro — contiene una **skill de Claude Code** que lo genera personalizado para ti. Tú llenas un cuestionario con tu perfil, plataformas y preferencias, y Claude genera todo el sistema adaptado a tu configuración: scripts, hooks, bot, daemons, memoria, y bootstrap.

## Inicio rápido

### 1. Clona el repo

```bash
git clone https://github.com/TU_USUARIO/caribebrain-starter.git ~/Desktop/MiSegundoCerebro
cd ~/Desktop/MiSegundoCerebro
```

### 2. Llena el cuestionario

Abre `.claude/skills/crear-segundo-cerebro/mi-segundo-cerebro.md` y completa las 8 secciones: tu nombre, plataformas que usas, tareas para la IA, nivel de autonomía, límites de seguridad, tipos de memoria, infraestructura, y prioridad de integraciones.

Hay un ejemplo completado en `ejemplo-requisitos.md` para referencia.

### 3. Genera tu segundo cerebro

```bash
claude
```

Dentro de Claude CLI:

```
/crear-segundo-cerebro ./mi-segundo-cerebro.md
```

Claude genera todos los archivos personalizados: pipeline RAG, hooks, bot de Telegram, daemons, plantillas de memoria, bootstrap, y un plan de implementación de 9 fases.

### 4. Instala y activa

```bash
bash bootstrap.sh
```

Sigue los pasos que imprime al final para autenticar Claude, configurar integraciones, y activar los daemons.

## Qué genera

- **Pipeline RAG** — chunking de Markdown, embeddings vectoriales (384 dims), búsqueda híbrida 70% vector + 30% keyword
- **4 hooks de Claude** — inyección de memoria, guardrails de seguridad, flush pre-compactación, resumen al cerrar sesión
- **4 daemons** — heartbeat cada 30 min, reflexión diaria, bot de Telegram, watchdog (launchd en macOS, systemd en Linux, cron en WSL)
- **Integraciones read-only** — Gmail, Google Calendar, Google Drive, GitHub, Telegram, Apple Reminders (macOS), y más
- **Sistema de seguridad** — guardrails deterministas, sanitización anti-inyección, trust boundaries
- **Vault de memoria** — personalidad del agente, perfil de usuario, memoria a largo plazo, hábitos, monitoreo

## Requisitos

- Python 3.11+
- Node.js 18+
- Claude CLI (`npm install -g @anthropic-ai/claude-code`)
- Cuenta de Anthropic

## Más información

Para más información sobre CaribeBrain, visita [https://www.linkedin.com/pulse/por-qu%25C3%25A9-constru%25C3%25AD-mi-propio-segundo-cerebro-en-lugar-de-gerardo-barcia-swb8f/] este artículo.

## Créditos

Inspirado en [second-brain-starter](https://github.com/coleam00/second-brain-starter) de Cole Medin. Arquitectura basada en CaribeBrain.

## Licencia

MIT
