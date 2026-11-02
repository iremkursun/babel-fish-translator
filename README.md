# 🐟 Babel Fish — Voice Translator with LLM, STT & TTS
![Babel Fish voice translator demo]
(<img width="808" height="638" alt="Voice Assistant_Translation" src="https://github.com/user-attachments/assets/9cdded80-bef8-4d42-b685-621bc533863f" />
)
A full-stack voice translation assistant inspired by the Babel Fish from *The Hitchhiker's Guide to the Galaxy*.
Speak (or type) in English and get the translation back in Spanish — both as text and as spoken audio.

Built as part of the **IBM Skills Network guided project "Babel Fish with LLM STT TTS"**.

## How it works

```
🎤 Voice / ⌨️ Text (English)
        │
        ▼
Watson Speech-to-Text  ──►  transcribed text
        │
        ▼
watsonx.ai LLM (Mistral)  ──►  Spanish translation
        │
        ▼
Watson Text-to-Speech  ──►  🔊 spoken Spanish audio
```

1. The browser records the user's voice and sends it to the Flask backend.
2. **Watson Speech-to-Text** converts the audio into English text.
3. The text is sent to an LLM on **IBM watsonx.ai** with a translation prompt.
4. **Watson Text-to-Speech** turns the translation into audio (base64-encoded in the JSON response).
5. The frontend shows the translation and plays it back.

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, JavaScript, Bootstrap, jQuery, Font Awesome |
| Backend | Python, Flask, Flask-CORS |
| LLM | IBM watsonx.ai — `mistralai/mistral-small-3-1-24b-instruct-2503` |
| Speech | IBM Watson Speech Libraries for Embed (STT & TTS) |

## Project structure

```
├── server.py          # Flask app & API routes
├── worker.py          # STT, LLM and TTS helper functions
├── templates/
│   └── index.html     # UI layout
├── static/
│   ├── script.js      # UI logic, recording, API calls
│   └── style.css      # Styling & animations
├── models/            # Dockerfiles for STT/TTS images
├── Dockerfile
└── requirements.txt
```

## API endpoints

| Method | Route | Description |
|---|---|---|
| `GET` | `/` | Serves the web interface |
| `POST` | `/speech-to-text` | Receives raw audio, returns `{ "text": "..." }` |
| `POST` | `/process-message` | Receives `{ userMessage, voice }`, returns `{ watsonxResponseText, watsonxResponseSpeech }` |

## Running locally (IBM Skills Network Cloud IDE)

```bash
python3.11 -m venv my_env
source my_env/bin/activate
pip install -r requirements.txt
python server.py
```

The app runs on `http://127.0.0.1:8000`.

> **Note:** Inside the Skills Network Cloud IDE, `PROJECT_ID="skills-network"` works without an API key.
> To run anywhere else, set your own watsonx.ai `API_KEY` and `PROJECT_ID` in `worker.py`, and point the
> STT/TTS `base_url` values to your own Watson Speech service endpoints.

## Changing the target language

Edit the prompt in `watsonx_process_message()` in `worker.py`:

```python
prompt = f"""
Translate the following English sentence into Spanish.
Reply ONLY with the translation, no explanations, no formatting.
English: {user_message}
Spanish:
"""
```

Replace `Spanish` with any language and choose a matching voice in the UI.

## Changes from the original lab

- **LLM model:** The lab uses `mistralai/mistral-medium-2505`, which is no longer supported in the environment
  (HTTP 422). It was replaced with `mistralai/mistral-small-3-1-24b-instruct-2503`.
- **Dependencies:** `ibm-watson-machine-learning` was replaced with `ibm-watsonx-ai` in `requirements.txt`.

## Project status

| Step | Status |
|---|---|
| 1–2. Interface & Flask server | ✅ Done |
| 3. watsonx.ai integration | ✅ Done |
| 4. Watson Speech-to-Text | ✅ Done |
| 5. Watson Text-to-Speech | ✅ Done |
| 6. Flask API endpoints | ✅ Done |
| 7. Running the app in Cloud IDE | ✅ Done |
| 9. Deploy to IBM Code Engine *(optional)* | ⏭️ Not completed |

The optional Code Engine deployment was not completed because it requires a personal watsonx.ai API key and
project ID, which were not available for this project. The application was built and tested in the
IBM Skills Network Cloud IDE.

## Credits

Based on the IBM Skills Network guided project by Talha Siddiqui, Rohit Arora and Vicky Kuo
(contributor: Kang Wang). Original lab content is licensed under the
[Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
