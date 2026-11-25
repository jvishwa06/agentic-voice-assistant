# AgenticVoiceAssistant

A voice assistant you talk to through your microphone. It sends your speech to an LLM on Groq and speaks the reply back. A second version adds a web-search tool so it can answer questions that need current information.

## Two versions

| File | What it does |
|---|---|
| `main.py` | Voice chat assistant named SANDY. It uses an LLM with per-session conversation memory. |
| `main_agent.py` | Voice assistant that runs as a LangChain agent (`ZERO_SHOT_REACT_DESCRIPTION`) with a web `Search` tool built on `GoogleSerperAPIWrapper`. It greets you as "Sivanirai2.0". |

## How it works

1. **Listen**: `speech_recognition` records from the microphone with a 5-second phrase limit and transcribes it with Google's recognizer.
2. **Think**: a Groq-hosted `llama-3.1-8b-instant` model (via `langchain-groq`, temperature 0.7) answers. The system prompt asks for replies of 30 words or fewer. Chat history is kept per `session_id` with `RunnableWithMessageHistory`.
3. **Speak**: `pyttsx3` reads the reply aloud. Voice, volume (1.0) and rate (200) are set in the `defaults` dictionary at the top of each file. The default voice is a macOS voice name, so change it on other systems.

In `main_agent.py` the agent can call the `Search` tool to look up up-to-date information before it answers.

## Setup

```bash
pip install -r requirements.txt
```

Create a `.env` file with:

```
GROQ_API_KEY=your_groq_key
SERPAPI_API_KEY=your_serper_key   # only needed for main_agent.py
```

`main_agent.py` passes `SERPAPI_API_KEY` to `GoogleSerperAPIWrapper` as its `serper_api_key`, so it expects a key from Serper.

## Run

```bash
python main.py          # voice chat
python main_agent.py    # voice chat with web search
```

`pyaudio` is required for microphone access. It needs system audio libraries on some platforms.
