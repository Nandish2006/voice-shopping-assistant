# Voice Command Shopping Assistant

A voice-based shopping list manager with smart suggestions, built as a single-page web app.

**Live app:** _[paste your Vercel URL here]_

## Features

- **Voice input** via the Web Speech API — tap the mic, speak naturally ("add 2 bottles of milk", "I need bananas", "remove milk")
- **NLP command parsing** using Groq's LLM API (`openai/gpt-oss-20b`) — understands varied phrasing, extracts action/item/quantity
- **Auto-categorization** — each item is tagged (dairy, produce, snacks, bakery, beverages, meat, household, other)
- **Substitute suggestions** — after adding an item, a second LLM call suggests a common alternative, shown as a dismissible chip with a one-click swap
- **Manual text input** as a fallback for testing or noisy environments
- **Offline fallback parser** — a rule-based parser runs automatically if no API key is set or the API call fails, so the app never breaks
- **Loading states** — spinner + "Thinking..." while the LLM call is in flight
- **Error handling** — mic permission denial, no-speech timeout, unsupported browser, and failed API calls are all caught and surfaced without crashing the app

## Tech stack

- Vanilla HTML/CSS/JS — no build step, no framework, single file
- Web Speech API (`SpeechRecognition`) for voice-to-text (Chrome/Chromium only)
- Groq API (`openai/gpt-oss-20b`) for command parsing and substitute suggestions
- `localStorage` for persisting the user's API key between sessions
- Deployed on Vercel as a static site

## Setup

1. Open the app (locally or the live URL above)
2. Click the ⚙️ settings icon
3. Paste a free Groq API key from [console.groq.com/keys](https://console.groq.com/keys)
4. Click Save — you'll see "✓ Key saved — smart parsing enabled"
5. Tap the mic and speak a command, or type one in the text box

Without an API key, the app still works using a basic offline rule-based parser (handles common phrasing like "add X", "remove X", "N bottles of X").

## Architecture / approach

The app tries the LLM parser first for any voice or text command. If no API key is set, or the Groq call fails (network issue, rate limit, model error), it silently falls back to a local regex-based parser so the core add/remove flow is never broken. Substitute suggestions are fetched asynchronously after an item is added and don't block the UI.

## Known limitations / future work

- **API key is stored client-side.** For a production app this should be proxied through a small backend/serverless function so the key is never exposed in the browser.
- **No multilingual support yet** — voice recognition is locked to `en-US`. The Web Speech API supports other locales; this would mean adding a language selector and passing it to `recognition.lang`.
- **No shopping history or seasonal recommendations** — the brief mentions suggesting items based on past purchases ("running low on bread") or what's in season. This would need persistent order history and a recommendation step, which was out of scope for the time budget.
- **No voice-activated search/filtering by brand, size, or price** — out of scope for the time budget; would require a product catalog/database to search against.
- **Web Speech API is Chrome-only** — Firefox and Safari have limited/no support for continuous speech recognition.

## Time investment

Built within the ~8 hour budget specified in the assignment, prioritizing the core voice → parse → list flow, categorization, and one smart-suggestion feature over full coverage of every listed requirement.
