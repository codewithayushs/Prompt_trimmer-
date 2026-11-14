# Prompt Trimmer

Paste a prompt (or upload an image of text) and get a shorter, grammatically correct version that keeps the same meaning. Fewer tokens means lower cost, faster responses, and more room in an AI model's context window.

## Why use it

- **Saves tokens.** Removes filler, politeness padding and repeated words. The saving carries into every later turn of a chat, because AI models re-read the whole conversation each time.
- **Keeps the meaning.** The AI mode is told to preserve every requirement, number, name, URL and code block, and never to add new instructions.
- **Fixes grammar and typos.** Cleans up spelling, spacing, punctuation and capitalization in the same pass.
- **Works from images too.** Upload or paste a screenshot and the text is read in your browser (OCR). This uses no AI tokens, and the image never leaves your computer.
- **Two modes.** *AI* for the best quality, and *Quick* for an instant offline version that needs no API key.
- **Shows the savings.** Displays estimated tokens before and after, plus the percentage saved.
- **Safe by design.** The API key stays on the server and is never exposed to visitors. If the AI is unavailable, the app falls back to Quick mode, and it never returns something longer than your original.
- **Multi-language input.** Handles English, and OCR supports Hindi.
- **Simple to run.** One HTML file and one small Node.js server, with no framework or build step.

## How it works

1. You paste a prompt, or upload or paste an image (the text is extracted in your browser).
2. You click **Optimize**.
3. In **AI mode**, `server.js` sends the text to the Gemini API with strict rewrite instructions. In **Quick mode**, rule-based cleanup runs in your browser.
4. The shorter prompt appears with the token savings. Copy it with one click.

## Project structure

```
index.html     Web page (interface, OCR, Quick mode)
server.js      Node server (serves the page, calls Gemini)
.env.example   Template for your settings
.gitignore     Keeps secrets out of git
README.md      This file
```

## Run it locally

**Requirements:** [Node.js](https://nodejs.org) 20.6 or newer, and a free Gemini API key.

1. **Get the code**
   ```
   git clone https://github.com/codewithayushs/Prompt_trimmer-.git
   cd Prompt_trimmer-
   ```
2. **Get an API key.** Open [Google AI Studio](https://aistudio.google.com), click **Get API key**, and create one.
3. **Create your `.env` file.** Copy `.env.example` to a new file named `.env` and put your key in it:
   ```
   GEMINI_API_KEY=your_real_key_here
   ```
   On Windows PowerShell you can create it with:
   ```
   Copy-Item .env.example .env
   ```
   then open `.env` and replace the placeholder.
4. **Start the server**
   ```
   node --env-file=.env server.js
   ```
   You should see: `Prompt Trimmer running at http://localhost:3000`
5. **Open the app.** Go to http://localhost:3000 in your browser.

Do not open `index.html` by double-clicking it. AI mode needs the Node server running.

### Configuration

| Setting | Purpose | Default |
|---|---|---|
| `GEMINI_API_KEY` | Your Gemini API key (required for AI mode) | none |
| `GEMINI_MODEL` | Model name to use. Check AI Studio for current names | `gemini-3.6-flash` |
| `PORT` | Port for the server | `3000` |

### Using Node older than 20.6

Run `npm i dotenv`, add `require('dotenv').config();` as the first line of `server.js`, and start with `node server.js`.

## Troubleshooting

- **`.env: not found`:** the file must be named exactly `.env` (with the dot, no `.txt`). Check with `dir -Force`.
- **"AI mode unavailable":** check the terminal for the error. Common causes are a missing or wrong key, a wrong model name, or a rate limit.
- **Model not found:** list the models your key can use, then set `GEMINI_MODEL` to one of them:
  ```
  curl "https://generativelanguage.googleapis.com/v1beta/models?key=YOUR_KEY"
  ```
- **Image text is wrong:** use a clearer, higher-resolution image. OCR works best on printed text and screenshots, and struggles with handwriting.
- **Rate limit (429):** the Gemini free tier has per-minute and per-day limits. Wait a minute, or enable billing for higher limits.

## Security

- Never put your API key in `index.html` or commit it to git. Keep it in `.env`, which `.gitignore` excludes.
- If a key is ever exposed, delete it in Google AI Studio and create a new one.
- When deploying, set `GEMINI_API_KEY` in your host's environment variables settings (Render, Railway, Fly.io, etc.).
- Before making the app public, add rate limiting per IP so others cannot use up your API quota.

## Limitations

- Token counts are estimates (about 4 characters per token). Exact counts vary by model.
- Quick mode is conservative and saves less than AI mode. It may miss grammar mistakes.
- OCR reads text only. Charts, diagrams and layout are not captured.
- Always read the optimized prompt before using it, especially if it contains code, numbers or legal or technical wording.

## Roadmap

- Chat compressor: turn a long conversation into a short handoff summary for a fresh chat.
- PDF and multi-page support.
- Real token counts from the API.
- Side-by-side diff view of what changed.

## License

Add a license of your choice (for example MIT) before sharing publicly.
