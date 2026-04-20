# FlashStudy

Go to the website: [https://anindita-sa.github.io/FlashStudy/]

A self-contained AI-powered flashcard and revision tool — one HTML file, no backend, no installation. Built for engineering students who want to quiz themselves on topic lists imported from Google Sheets.

## Features

- **Google Sheets import** — publish any tab as CSV and import it directly. Columns: `Topic, Subtopic, Notes, Stars, Grade`
- **Smart quiz mode** — subtopic shown, you recall the parent topic. Full cycle before reshuffle, no back-to-back repeats
- **Spaced repetition** — weak topics and 3-star topics appear twice per cycle
- **AI explanations** — "Explain it to me" gives a concise 2-3 sentence explanation in context using Groq or Gemini
- **AI answer checking** — type or speak your answer, get a scored breakdown with your answer vs ideal
- **Manual grading** — ✓ ~ ✗ buttons to grade yourself without using the API
- **Voice input** — Groq Whisper transcribes your spoken answer (works in all browsers including Brave)
- **Text-to-speech** — each card is read aloud automatically; mutable per session
- **Difficulty toggle** — Easy / Medium / Hard changes how strictly AI grades your answers
- **Star importance** — 1-3 stars per topic; 3-star topics quizzed twice per cycle
- **Manual traffic light** — set green/yellow/red per topic manually, saved across sessions
- **Progress export** — download a CSV of all scores, stars, flags, and grades to paste back into your sheet
- **Dark / light theme** — toggle in the header, remembered across sessions
- **No installation** — open the HTML file in any browser, everything runs locally

## Setup

### 1. Get an API key

**Groq (recommended — free, no billing required)**
1. Go to [console.groq.com](https://console.groq.com)
2. Create an account and generate an API key
3. Paste it in FlashStudy → Settings → Groq API key

**Gemini (alternative)**
1. Go to [aistudio.google.com](https://aistudio.google.com)
2. Generate an API key
3. Paste it in FlashStudy → Settings → Gemini API key

> Note: The free Gemini API tier has tight quota limits. Groq is more reliable for regular study sessions.

### 2. Prepare your Google Sheet

Create a Google Sheet with one tab per subject. Each tab must have this header row:

```
Topic,Subtopic,Notes,Stars,Grade
```

- **Topic** — the parent category (e.g. `Interrupts`, `Memory`, `Timers`)
- **Subtopic** — the specific concept shown on the flashcard (e.g. `INT0 and INT1 external interrupts`)
- **Notes** — extra context passed to the AI (optional, can be blank)
- **Stars** — 0, 1, 2, or 3 (optional). 3-star topics are quizzed twice per cycle
- **Grade** — `green`, `yellow`, `red`, or blank (optional). Your manual performance rating

Example rows:
```
Interrupts,INT0 and INT1 external interrupts,Triggered by low level or falling edge on P3.2 and P3.3,3,green
Interrupts,Timer interrupt TF0,Set when Timer 0 overflows from FFH to 00H,2,
Memory,Internal RAM structure,256 bytes split into register banks and SFRs,1,yellow
```

### 3. Publish your sheet as CSV

For each tab you want to import:
1. **File → Share → Publish to web**
2. Select the specific tab from the dropdown
3. Choose **CSV** format
4. Click **Publish** and copy the URL

The URL will look like:
```
https://docs.google.com/spreadsheets/d/SHEET_ID/pub?gid=TAB_ID&single=true&output=csv
```

### 4. Import into FlashStudy

1. Open FlashStudy in your browser
2. Click **↓ Import** → paste the subject name and the CSV URL
3. Click **Import** — the app tries multiple CORS proxies automatically

> If the Sheets import fails, use the **CSV file** tab: export the tab from Google Sheets (File → Download → CSV) and drag it into the app.

## Hosting on GitHub Pages

1. Create a new GitHub repository (public or private)
2. Upload `flashstudy.html` and rename it to `index.html`
3. Go to **Settings → Pages → Source** → select `main` branch → save
4. Your app will be live at `https://yourusername.github.io/repo-name/`

All data (API keys, subjects, scores) is stored in your browser's `localStorage` — it stays on each device you use and is never sent to any server except Groq/Gemini for AI calls.

## Saving progress across sessions

FlashStudy saves everything to `localStorage`. If you clear your browser cache, this data is lost. To back up:

1. Open a subject's topic panel
2. Click **↑ Export** — downloads a CSV with all scores, stars, flags, and grades
3. Paste these columns into your Google Sheet (`Stars` and `Grade` columns)
4. Next time you reimport the sheet, your progress is restored automatically

> The app warns you before closing the tab if you have unsaved progress this session.

## Usage

### Quiz mode

- Each card shows a **subtopic** — try to recall which parent **topic** it belongs to
- Click **Reveal topic** to check
- Click **💡 Explain it to me** for an AI explanation
- Type or speak your understanding, then **Check with AI** for scored feedback
- Or use **✓ ~ ✗** to grade yourself manually (no API call)

### Filters in topic panel

| Button | Effect |
|---|---|
| All | Select every topic |
| Weak only | Select only flagged weak topics |
| Unattempted | Select topics with no score or manual grade yet |
| None | Deselect all |

### Difficulty (E / M / H)

| Level | Behaviour |
|---|---|
| Easy | Keywords and short phrases = full marks. No elaboration needed |
| Medium | Correct concepts + some explanation. Minor gaps OK |
| Hard | Full accurate explanation required. Current strictness |

Toggle in the quiz header or in Settings. Only affects AI grading — manual grades are unaffected.

## File structure

```
index.html    # The entire app — one self-contained file
README.md     # This file
```

## Browser compatibility

| Browser | Quiz | AI features | Voice input |
|---|---|---|---|
| Chrome | ✅ | ✅ | ✅ |
| Brave | ✅ | ✅ | ✅ |
| Firefox | ✅ | ✅ | ✅ |
| Safari | ✅ | ✅ | ✅ (iOS 14.5+) |
| Edge | ✅ | ✅ | ✅ |

Voice input uses Groq's Whisper API via `MediaRecorder` — no browser-native speech recognition required.

## Privacy

- API keys are stored only in your browser's `localStorage`
- No data is sent to any server except Groq or Gemini for AI inference calls
- The app has no analytics, no tracking, and no external dependencies beyond Google Fonts and CORS proxy services used during CSV import
