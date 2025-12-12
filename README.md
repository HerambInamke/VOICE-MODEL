# VOICE-MODEL

Voice agent powered by LiveKit Agents using Sarvam STT/TTS, OpenAI GPT-4.1-mini, Silero VAD, and the LiveKit multilingual turn detector. The agent loads credentials from `.env.local` and runs an RTC server that immediately greets and assists participants in a LiveKit room.

## Project layout (index)

- `agent.py` — defines the romantic/flirty assistant persona, sets up the Sarvam STT/TTS + GPT-4.1-mini pipeline, noise cancellation, VAD, and turn detection; registers the RTC session entrypoint.
- `pyproject.toml` — Python package metadata and core dependencies (livekit-agents with sarvam, silero, turn-detector extras; noise-cancellation plugin; python-dotenv).
- `uv.lock` — locked dependency versions for reproducible installs with `uv`.
- `LICENSE` — MIT license.

## Requirements

- Python 3.10+
- [`uv`](https://github.com/astral-sh/uv) (recommended) or `pip`
- LiveKit project with API key/secret and an accessible LiveKit server
- API keys for OpenAI and Sarvam

## Setup

1. Install dependencies (uses the lockfile):
   - `pip install uv` (if not already installed)
   - `uv sync`
2. Create `.env.local` at the repo root:
   ```
   LIVEKIT_URL=wss://<your-livekit-host>
   LIVEKIT_API_KEY=lk_api_key
   LIVEKIT_API_SECRET=lk_api_secret
   OPENAI_API_KEY=sk-...
   SARVAM_API_KEY=svm-...
   ```
   Add any other provider credentials your deployment needs.

## Running the agent

- Start the RTC agent server:
  - `uv run agent.py`
  - or activate the virtualenv created by `uv sync` and run `python agent.py`
- Join the LiveKit room referenced by your credentials; the agent will greet the participant and keep the conversation flowing using the defined persona and Hindi speech pipeline.

## Behavior highlights

- Persona: romantic, playful assistant with concise, intimate replies.
- Audio pipeline: Sarvam `saarika:v2.5` STT (Hindi), Sarvam TTS (`anushka` voice), GPT-4.1-mini LLM, Silero VAD, multilingual turn detection, and BVC/BVCTelephony noise cancellation.
- Server entrypoint: `agents.cli.run_app(server)` exposes the RTC session defined in `agent.py`.

## Notes

- If you change models or providers, update the constructors in `agent.py` accordingly and add the needed environment variables to `.env.local`.
- Lockfile is present; prefer `uv sync` to ensure consistent dependency versions.

## License

MIT — see `LICENSE`.

