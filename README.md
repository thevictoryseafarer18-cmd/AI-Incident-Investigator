# 🔍 AI Incident Investigator

> Correlates scattered logs, bug reports, and deployment changes into a timeline, ranked root-cause hypotheses with evidence — and clear next debugging steps.

A browser-only MVP prototype built in 60 minutes. No backend, no API keys, fully explainable.

![Status](https://img.shields.io/badge/status-MVP%20Prototype-6366f1?style=flat-square)
![Tech](https://img.shields.io/badge/tech-HTML%20%2B%20CSS%20%2B%20JS-06b6d4?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-10b981?style=flat-square)

---

## ✨ Features

- 📥 **Accepts evidence** — error logs, bug reports, and deployment notes (paste your own or load sample)
- 🔗 **Identifies related errors** — groups similar messages into clusters by signature
- 🕒 **Generates a timeline** — all events merged & sorted by time (deploy / warn / error / alert)
- 💡 **Suggests root-cause hypotheses** — ranked by evidence strength, each with *Evidence for / against / How to test*
- 🛠 **Recommends debugging steps** — ordered checklist based on the top hypothesis
- ✅ **Facts vs Hypotheses** — confirmed facts (with source) are strictly separated from scored inferences
- 🧾 **Reasoning trace** — machine-readable summary of what the engine saw and scored
- ⬇ **Export** — download incident report as Markdown or copy a summary

## 🚀 Live Demo

Open `index.html` in any browser — it runs entirely client-side.

> **GitHub Pages:** Settings → Pages → Deploy from branch → `main` / root. Your site will be live at `https://<username>.github.io/<repo>/`

## 🧪 Try It (30 seconds)

1. Click **✨ Load sample dataset** (pre-loaded)
2. Click **▶ Analyze incident**
3. Read: Timeline → Hypotheses → Debugging Steps
4. **Explainability test:** Edit the deploy notes (remove “Redis pool”) and re-analyze — confidence for Hypothesis #1 drops from HIGH to LOW.

## 📊 Sample Dataset

**Incident:** Checkout 500s after `v2.4.1`

| Source | Key detail |
|---|---|
| Logs (14 events) | Deploy 14:18:44Z → Redis warnings → timeout errors 14:19:15Z → DB pool exhausted → alert 18% |
| Bug report | ~20% checkout failures since 14:19, 340 failed in 6 min, payment gateway healthy |
| Deploy notes | Redis pool cut **50 → 10**, timeout 2000ms, new inventory-check logic |

**Engine output:** Deploy → first error = **0.5 min**, dominant cluster = Redis timeout, overlap = Redis → **HIGH confidence** hypothesis: *“Deployment reduced Redis pool → timeouts → checkout 500s”*

## 🏗 Architecture

```
[Browser UI — single index.html]
  ├─ 3 inputs: Logs | Bug Report | Deploy Notes
  ├─ Rule-based Correlation Engine (JavaScript, client-side)
  │    1. Parse timestamps / service / level / message (regex)
  │    2. Group errors by signature (normalize trace_id, numbers)
  │    3. Merge into timeline, check deploy proximity (≤5 min)
  │    4. Score hypotheses: timing + keyword overlap + frequency
  └─ Explainable Results
       Confirmed Facts | Timeline | Error Clusters | Hypotheses | Steps
```

### How the engine decides (transparent rules)

- **Temporal proximity:** deploy within 5 min before first error → strong signal (+3)
- **Keyword overlap:** same component (Redis / DB / payment) in deploy + errors → boosts score (+3)
- **Pool/config change:** explicit reduction matching error type → +2
- **Frequency:** dominant error cluster → +1
- **Warning precursor:** cache/pool warnings before errors → +1
- **Cascade check:** downstream errors *after* upstream are treated as effect, not cause
- **Confidence:** High ≥7 · Medium 4–6 · Low <4

## 📁 Project Structure

```
ai-incident-investigator/
├── index.html      # Entire app (UI + engine + sample data) — no build step
├── README.md       # This file
├── LICENSE         # MIT
└── .gitignore
```

## 🛠 Tech Stack

- **Frontend:** Single-file HTML + CSS + vanilla JavaScript (zero dependencies)
- **No backend / no database / no API keys** — runs offline in the browser
- **Future:** Swap the rule engine for an LLM while keeping the evidence-linking UI

## 🔮 Roadmap

- [ ] File upload (`.log` drag-and-drop)
- [ ] Second sample incident (e.g. DB migration failure)
- [ ] LLM-powered hypothesis generation with citation
- [ ] Dark mode
- [ ] Shareable incident links / permalink

## 🤝 Contributing

Issues and PRs welcome. Keep the core principle: **every claim must link to evidence; facts ≠ hypotheses.**

## 📄 License

MIT — see [LICENSE](LICENSE) for details.

---

*Built as a 60-minute MVP prototype. Rule-based engine runs entirely in your browser.*
