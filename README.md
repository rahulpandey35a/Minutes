# Minute — Deploy & Test on iPhone

Free, on-device (Whisper via transformers.js) meeting recorder, transcript, and rule-based action-item extractor. No account, no API key, no server. Audio never leaves the phone.

## 1. Deploy to GitHub Pages

1. Create a new repo (or reuse an existing one), e.g. `minute-app`.
2. Copy these files into the repo root:
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - `icon-192.png`
   - `icon-512.png`
3. Commit and push to `main`.
4. In the repo: **Settings → Pages → Source → Deploy from a branch → `main` / root**.
5. Wait ~1 minute, then your app is live at:
   `https://<your-username>.github.io/minute-app/`

## 2. Install on iPhone (as a PWA)

1. Open the GitHub Pages URL in **Safari** (must be Safari, not Chrome, for Add to Home Screen to register the service worker correctly on iOS).
2. Tap the Share icon → **Add to Home Screen**.
3. Open it from the home screen icon — it now runs full-screen like a native app.
4. First launch: tap **Load model** in Settings before recording, so the Whisper model downloads once (needs an internet connection the first time only; cached after that for offline use).

## 3. Using it

- **Start recording** → speak → **Stop recording** → transcript appears after a short on-device processing pause.
- Tap **Extract action items** to pull likely action sentences (rule-based: looks for phrases like "need to," "will," "by Friday," "follow up," and Hindi equivalents).
- Add your department's abbreviations/terms under **Nomenclature glossary** — they'll highlight in the transcript so you can spot-check the model's guesses.
- Everything (transcript, action items, glossary) persists in the browser via localStorage — clearing Safari's site data will erase it.

## Known limitations (by design, to stay free and private)

- **Model size vs. accuracy**: "Base" is the default balance. If accuracy on your real meeting audio is weak, try "Small" in Settings — bigger download, slower, more accurate.
- **Action-item extraction is rule-based, not AI-summarized.** It flags likely action sentences by keyword pattern; it does not understand meaning the way a paid LLM would. Expect to manually edit the list.
- **iOS Safari + WebAssembly**: on-device transcription is CPU-intensive. Longer recordings (10+ min) may take a noticeably longer time to process than the recording itself — that's expected, not a bug.
- **No cloud fallback by default** — this was a deliberate choice given the app may handle work meeting content. If accuracy turns out too weak on-device, the next fallback to consider is a free-tier cloud API (e.g. Groq's hosted Whisper), which trades privacy for accuracy. Not wired in here on purpose.

## Updating the app later

Whenever you change `index.html`, `manifest.json`, or `sw.js`, bump `CACHE_NAME` in `sw.js` (e.g. `minute-v2`) — otherwise iOS Safari may keep serving the old cached version.
