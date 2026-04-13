# StudyAI — AI-Powered Study Assistant

> Paste your notes. Get a summary, flashcards, and a quiz — instantly.

![StudyAI](https://img.shields.io/badge/StudyAI-v1.0-f0c040?style=for-the-badge&logoColor=black)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Gemini API](https://img.shields.io/badge/Google_Gemini-API-4285F4?style=flat&logo=google&logoColor=white)

---

## 🎯 What Is This?

**StudyAI** is a zero-dependency, browser-based study tool that transforms any block of text into three interactive learning resources using the **Google Gemini AI API**. Built with pure HTML, CSS, and Vanilla JavaScript — no frameworks, no build tools, no installation required.

| Feature | Description |
|---|---|
| 📝 **Smart Summary** | Overview, 5 key bullet points, and a "why it matters" section |
| 🃏 **Flashcards** | 8 interactive flip-cards with 3D animation and keyboard navigation |
| 🧠 **MCQ Quiz** | 5 multiple-choice questions with live scoring and AI explanations |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Structure | HTML5 (semantic, single-file) |
| Styling | CSS3 — custom properties, flexbox, 3D transforms, animations |
| Logic | Vanilla JavaScript (ES6+) — no frameworks |
| AI Engine | Google Gemini API (`gemini-2.0-flash`) |
| Storage | `localStorage` — API key persisted in browser only |

**Single file. No npm. No build step. Just open `index.html`.**

---

## 📁 Project Structure

```
studyai/
├── index.html       ← Entire app: HTML + CSS + JS in one self-contained file
└── README.md
```

Everything lives in `index.html` — no external CSS files, no separate JS modules, no CDN dependencies that can fail to load.

---

## ⚙️ How It Works

### 1. Smart Mode Detection
When the user clicks **Generate Study Set**, the app checks whether a built-in sample is loaded. If yes, it serves pre-built demo data instantly (no API call). If the user pasted their own text, it calls the Gemini API.

```javascript
function getActiveSample() {
  const text = document.getElementById('notesInput').value.trim();
  for (const [key, val] of Object.entries(SAMPLES)) {
    if (text === val.trim()) return key; // demo mode
  }
  return null; // API mode
}
```

### 2. Prompt Engineering
For custom text, a carefully structured prompt instructs Gemini to return a strict JSON schema containing all three study assets in a **single API call** — no multiple requests needed.

```javascript
const res = await fetch(
  `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${key}`,
  {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      contents: [{ parts: [{ text: prompt }] }],
      generationConfig: { temperature: 0.3, maxOutputTokens: 3000 }
    })
  }
);
```

### 3. JSON Parsing & Rendering
The AI response is sanitized (markdown fences stripped), parsed into a structured object, then rendered across three interactive panels without any page reload.

```json
{
  "summary": {
    "overview": "...",
    "keyPoints": ["...", "...", "..."],
    "importance": "..."
  },
  "flashcards": [{ "question": "...", "answer": "..." }],
  "quiz": [{
    "question": "...",
    "options": ["A) ...", "B) ...", "C) ...", "D) ..."],
    "correct": 0,
    "explanation": "..."
  }]
}
```

### 4. Interactive UI Components

**Flashcards** — CSS 3D flip using `perspective` + `rotateY(180deg)` with `backface-visibility: hidden`. Navigable by click, arrow keys (← →), or dot indicators. Space bar flips the current card.

**Quiz Engine** — Tracks live score in state, disables all options after selection, colour-codes correct (green) and wrong (red) answers, appends an AI-generated explanation, then shows a final results screen with performance message.

---

## 🔑 API Key Setup (for custom text only)

1. Visit [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
2. Sign in with your Google account — **completely free**
3. Click **Create API Key**
4. Paste it into the key field at the bottom of the app

> **Privacy:** Your API key is stored only in your browser's `localStorage`. It is never sent anywhere other than Google's Gemini API endpoint.

---

## ✨ Full Feature List

- ✅ **Hybrid mode** — built-in samples use demo data (no API), custom text calls Gemini API
- ✅ **Zero dependencies** — single HTML file, works offline for demo topics
- ✅ **Prompt engineering** — one API call returns all 3 study assets as structured JSON
- ✅ **3D flashcard flip** — CSS `perspective` + `rotateY`, no JS animation library
- ✅ **Live quiz scoring** — real-time score badge, per-question AI explanation
- ✅ **Keyboard navigation** — ← → to paginate cards, Space to flip
- ✅ **Copy to clipboard** — one-click summary export
- ✅ **localStorage** — API key persists across browser sessions
- ✅ **Graceful error handling** — distinct messages for bad key, quota exceeded, network failure
- ✅ **Fully responsive** — works on mobile and desktop
- ✅ **No white flash** — dark theme forced via `:root` CSS variables, renders instantly

---

## 🎨 Design Notes

- **Forced dark theme** via CSS custom properties — no flash of white background on load, no CDN font delay
- **Gold accent** (`#f0c040`) used consistently for interactive elements, active states, and highlights
- **No external stylesheets or fonts** — zero layout shift, instant first paint
- Custom **CSS design system** using variables for colours, radii, and spacing

---

## 🔮 Possible Extensions

- PDF / DOCX file upload using the FileReader API
- Export flashcards to Anki-compatible `.apkg` format
- Spaced repetition algorithm for smart flashcard review
- Save multiple study sets to `localStorage`
- Session-based progress tracking
- Light/dark theme toggle

---

## 👤 Author

Built by **Bhavya** — a portfolio project demonstrating frontend engineering, AI API integration, and prompt engineering skills.

---

## 📄 License

MIT — free to use, modify, and distribute.
