# Satomi Nakamichi

  24/7. Always online. Always listening.

  ---

  ## Links

  | | |
  |--|--|
  | satomi.world | [satomi.world](https://satomi.world) |
  | Telegram | [t.me/satominakamichi](https://t.me/satominakamichi) |
  | Telegram stream | [telegram.satomi.world](https://telegram.satomi.world) |
  | Twitter stream | [x.satomi.world](https://x.satomi.world) |
  | Twitter | [x.com/ask_satomi](https://x.com/ask_satomi) |

  ---

  ## How It Works

  ### Telegram

  ```mermaid
  flowchart TD
      A[User sends message in Telegram group] --> B{Mentioned or replied to bot?}
      B -- No --> Z[Ignored]
      B -- Yes --> C[Bot receives update]
      C --> D{Voice message?}
      D -- Yes --> E[ElevenLabs STT transcribes audio]
      D -- No --> F[Use text directly]
      E --> G[Claude generates Satomi response]
      F --> G
      G --> H[Response ready]
      H --> I[ElevenLabs TTS synthesizes voice]
      H --> J[WebSocket broadcasts event]
      I --> K[Bot sends voice message to group]
      J --> L[Stream page receives event]
      L --> M[3D avatar animates and speaks]
      H --> N[Q and A saved to PostgreSQL]
  ```

  ### Twitter

  ```mermaid
  flowchart TD
      A[User mentions or replies on Twitter] --> B[Bot detects mention]
      B --> C[Claude generates Satomi response]
      C --> D[Response ready]
      D --> E[ElevenLabs TTS synthesizes voice]
      D --> F[WebSocket broadcasts event]
      E --> G[Audio plays on stream page]
      F --> H[3D avatar animates and speaks]
      D --> I[Reply posted to Twitter thread]
  ```

  ### Stream Page

  ```mermaid
  flowchart LR
      A[OBS captures stream page] --> B[Livestream output]
      subgraph Stream page
          C[WebSocket hook listens for events]
          C --> D[Avatar animates on new message]
          C --> E[Chat log updates in real time]
      end
  ```

  ---

  ## Repositories

  | Repo | |
  |------|--|
  | [telegram](https://github.com/satominakamichi/telegram) | Telegram bot, voice TTS, WebSocket bridge, stream page |

  ---

  ## Stack

  Claude, ElevenLabs, Three.js, React, Express, PostgreSQL, Docker
  