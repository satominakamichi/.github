# Satomi Nakamichi

  **AI VTuber moderator, streamer, and community host.**

  Satomi is a 24/7 AI persona that reads messages from the community, responds in voice, and appears live on a 3D avatar stream.

  ---

  ## Platform Links

  | Platform | Link |
  |----------|------|
  | Main website | [satomi.world](https://satomi.world) |
  | Telegram group | [t.me/satominakamichi](https://t.me/satominakamichi) |
  | Telegram stream page | [telegram.satomi.world](https://telegram.satomi.world) |
  | Twitter/X stream page | [x.satomi.world](https://x.satomi.world) |

  ---

  ## How It Works

  ### Telegram Flow

  ```mermaid
  flowchart TD
      A[User sends message in Telegram group] --> B{Mentioned @asksatomibot or replied?}
      B -- No --> Z[Ignored]
      B -- Yes --> C[telegram-bot.ts receives update]
      C --> D{Voice message?}
      D -- Yes --> E[ElevenLabs STT transcribes audio]
      D -- No --> F[Use text directly]
      E --> G[satomi-ai.ts calls Claude AI]
      F --> G
      G --> H[AI generates Satomi response]
      H --> I[satomi-tts.ts generates voice via ElevenLabs]
      H --> J[satomi-ws.ts broadcasts event via WebSocket]
      I --> K[Bot sends voice message to Telegram group]
      J --> L[stream.tsx receives WS event]
      L --> M[3D avatar animates and speaks on stream page]
      H --> N[satomi-db-logs.ts saves Q and A to PostgreSQL]
  ```

  ### Stream Page Flow

  ```mermaid
  flowchart LR
      A[OBS captures stream page] --> B[Telegram livestream]
      subgraph Stream Page at telegram.satomi.world
          C[use-satomi-ws.ts hooks WebSocket]
          C --> D[Avatar animates on new message]
          C --> E[Chat log shows last 5 entries]
          F[GET /api/satomi/history] --> E
      end
  ```

  ---

  ## Repositories

  | Repo | Description |
  |------|-------------|
  | [telegram](https://github.com/satominakamichi/telegram) | Full Telegram bot with voice TTS, WebSocket bridge, and 3D avatar stream page |

  ---

  ## Tech Stack

  - **AI**: Claude (via Anthropic API)
  - **Voice**: ElevenLabs TTS and STT
  - **Bot**: node-telegram-bot-api
  - **Stream page**: React, Three.js, VRM avatar
  - **Backend**: Express, WebSocket
  - **Database**: PostgreSQL (persistent Q&A logs)
  - **Deploy**: Docker, docker-compose, GitHub Actions
  