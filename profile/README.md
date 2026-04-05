# Satomi Nakamichi

  24/7. Always online. Always listening.

  ## Overview

  Satomi Nakamichi is a fully autonomous online presence operating around the clock across Telegram and Twitter. Incoming messages are processed by a language model, converted to audio via voice synthesis, and delivered back to the conversation in real time. A 3D avatar stream page mirrors every interaction live, built to be captured by OBS and broadcast as a continuous livestream.

  The system is event-driven with zero downtime: no manual moderation, no scripts, no scheduled posts.

  ## Live Platforms

  | Platform | Link |
  |----------|------|
  | Main website | [satomi.world](https://satomi.world) |
  | Telegram group | [t.me/satominakamichi](https://t.me/satominakamichi) |
  | Telegram stream | [telegram.satomi.world](https://telegram.satomi.world) |
  | Twitter | [x.com/ask_satomi](https://x.com/ask_satomi) |
  | Twitter stream | [x.satomi.world](https://x.satomi.world) |

  ## Architecture

  ### Telegram Flow

  ```mermaid
  flowchart TD
      A[User sends message in Telegram group] --> B{Mentioned or replied to bot?}
      B -- No --> Z[Ignored]
      B -- Yes --> C[telegram-bot.ts receives update]
      C --> D{Voice message?}
      D -- Yes --> E[ElevenLabs STT transcribes audio to text]
      D -- No --> F[Use text directly]
      E --> G[satomi-ai.ts calls Claude with system prompt and session history]
      F --> G
      G --> H[Satomi response generated]
      H --> I[satomi-tts.ts synthesizes voice via ElevenLabs]
      H --> J[satomi-ws.ts broadcasts WebSocket event to stream page]
      H --> K[satomi-db-logs.ts persists Q and A to PostgreSQL]
      I --> L[Bot sends voice reply to Telegram group]
      J --> M[stream.tsx receives event and avatar animates]
  ```

  ### Twitter Flow

  ```mermaid
  flowchart TD
      A[User mentions or replies on Twitter] --> B[Bot detects mention via polling]
      B --> C[satomi-ai.ts calls Claude with system prompt and session history]
      C --> D[Satomi response generated]
      D --> E[satomi-tts.ts synthesizes voice via ElevenLabs]
      D --> F[satomi-ws.ts broadcasts WebSocket event to stream page]
      D --> G[Reply posted to Twitter thread]
      E --> H[Audio plays on stream page]
      F --> I[stream.tsx receives event and avatar animates]
  ```

  ### Stream Page and OBS Pipeline

  ```mermaid
  flowchart LR
      subgraph Backend
          WS[WebSocket server]
          TTS[ElevenLabs TTS]
      end
      subgraph StreamPage["Stream Page · satomi.world"]
          Hook[use-satomi-ws.ts hooks WebSocket]
          Hook --> Avatar[3D VRM avatar animates]
          Hook --> Chat[Live chat log updates]
          Hook --> Speech[Browser plays synthesized audio]
      end
      Backend --> Hook
      OBS[OBS captures stream page] --> Live[Telegram / Twitter livestream]
      StreamPage --> OBS
  ```

  ## Tech Stack

  | Layer | Technology |
  |-------|-----------|
  | Language model | Claude via Anthropic API |
  | Voice synthesis | ElevenLabs TTS |
  | Speech to text | ElevenLabs STT |
  | Telegram bot | node-telegram-bot-api |
  | Backend | Node.js, Express, WebSocket |
  | Frontend | React, Vite, Three.js |
  | 3D avatar | VRM model rendered with Three.js |
  | Database | PostgreSQL |
  | Package manager | pnpm workspaces |
  | Deployment | Docker, docker-compose, GitHub Actions |

  ## Repositories

  | Repo | Description |
  |------|-------------|
  | [satominakamichi/telegram](https://github.com/satominakamichi/telegram) | Telegram bot, ElevenLabs TTS/STT, WebSocket bridge, React stream page with 3D VRM avatar, PostgreSQL chat history, Docker deployment |

  ## Key Features

  **Conversational memory:** Each user has a rolling session history. Satomi references earlier messages naturally within the same conversation.

  **Emotion and gesture system:** Every response carries an emotion tag and a gesture key. The stream page maps these directly to facial expressions and arm animations on the VRM avatar.

  **Voice input support:** Users can send voice notes to the Telegram bot. Audio is transcribed via ElevenLabs STT before reaching the language model.

  **Idle presence:** When no questions arrive, Satomi generates short natural audience check-ins on a cooldown timer, all routed through the language model with no hardcoded strings.

  **Persistent history:** Every Q and A pair is written to PostgreSQL. The stream page loads the last five entries on mount, independent of the live WebSocket connection.

  **Duplicate prevention:** A sliding window deduplicator blocks the same message from being processed twice within a short window, protecting against spam and double-sends.

  ## Deployment

  The system ships with `docker-compose.yml` covering the API service and a PostgreSQL database. A GitHub Actions workflow builds and pushes the Docker image to GitHub Container Registry on every push to `main`, then deploys via SSH to the target server.

  Copy `.env.example` to `.env`, fill in the required secrets, then run:

  ```bash
  docker compose up -d
  ```

  Required secrets: `TELEGRAM_BOT_TOKEN`, `ELEVENLABS_API_KEY`, `AI_INTEGRATIONS_ANTHROPIC_API_KEY`, `DATABASE_URL`, `SESSION_SECRET`.

  *Built and maintained by [kai-liam](https://github.com/kai-liam)*
  