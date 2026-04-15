# Diyetim — Haftalık Beslenme Planı Takip Uygulaması

A mobile-first PWA for tracking weekly meal plans provided by a registered dietitian. Designed for static hosting (GitHub Pages), offline-capable, Turkish UI.

## How It Works

### 1. Get Your Weekly Plan JSON

Your dietitian provides a PDF each week. Use any AI (Claude, ChatGPT, Gemini) to convert it into structured JSON:

1. Open your preferred AI chat
2. Copy the prompt from `EXTRACTION_PROMPT.md` as a system/initial message
3. Upload your dietitian's PDF
4. Copy the JSON output

### 2. Import into the App

1. Open the app → **Ayarlar** tab
2. Paste the JSON into the text area
3. Tap **Planı Yükle**
4. Switch to **Plan** tab and start assigning meals

### 3. Plan Your Week

- Tap any empty meal slot to see available options
- Each option shows remaining weekly frequency (e.g., 2/3 = 2 left out of 3)
- Options automatically gray out when their weekly cap is reached
- Tap an assigned meal to change it; tap the × to remove it
- Swipe left/right to navigate between days

### 4. Track Nutrition

- **Beslenme** tab shows daily and weekly macro summaries
- All values are AI-estimated based on Turkish portion sizes (±15-20%)

## Deployment

### GitHub Pages (recommended)

```bash
git init
git add .
git commit -m "Initial deploy"
git remote add origin https://github.com/YOUR_USERNAME/diyetim.git
git push -u origin main
```

Then enable GitHub Pages in repo settings (Source: main branch, root folder).

### Local

Just open `index.html` in a browser. Service worker requires HTTPS or localhost.

## File Structure

```
diyetim/
├── index.html          # Full app (single file, ~35KB)
├── sw.js               # Service worker for offline support
├── manifest.json       # PWA manifest
├── sample-plan.json    # Example plan data (from real PDF)
├── EXTRACTION_PROMPT.md # AI prompt for PDF → JSON conversion
└── README.md           # This file
```

## Tech Stack

- Vanilla HTML/CSS/JS (no build step, no framework)
- localStorage for persistence
- Service Worker for offline PWA
- Google Fonts (Nunito)
- Total bundle: ~40KB (excluding font)

## Data Model

Each weekly plan follows this JSON structure:

```json
{
  "week": "2026-W16",
  "dietitian": "Name",
  "slots": [
    {
      "id": "ilk-ogun",
      "label": "İlk Öğün",
      "time": "09:00 - 10:00",
      "options": [
        {
          "id": "beyaz-smoothie",
          "name": "Beyaz Smoothie",
          "maxPerWeek": 2,
          "ingredients": [...],
          "nutrition": { "kcal": 395, "protein": 11, "carb": 52, "fat": 17, "fiber": 4 }
        }
      ]
    }
  ]
}
```

## Limitations

- Nutrition values are estimates (±15-20%). Your dietitian's plan is the authority.
- Data is stored per-device (localStorage). No cross-device sync.
- The `overcapacity` flag on dinner slot means total option caps exceed 7 days — this is intentional flexibility from the dietitian.
