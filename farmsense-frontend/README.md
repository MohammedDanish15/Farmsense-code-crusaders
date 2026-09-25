# FarmSense — Frontend

React (Vite) frontend for the FarmSense hackathon project (PS-01, AgriTech track).
Consumes the FastAPI backend at `http://localhost:8000/api`.

## Run it

```bash
npm install
npm run dev
```

Open the URL Vite prints (default: http://localhost:5173).

> The backend base URL lives in **one place**: `src/config.js`
> (`API_BASE_URL`). Change it there before the demo if the backend moves.

## What's inside

| Screen / file | What it does |
|---|---|
| `src/screens/Onboarding.jsx` | Profile form (geolocation with manual override, crop, planting date, height, watering, irrigation, fertilizer) → `POST /farmer/profile`, stores `farmer_id` in `localStorage` |
| `src/screens/PhotoUpload.jsx` | Photo upload (`multipart/form-data`, field `photo`) → `POST /farmer/{id}/photo`; on failure or by choice, the four-option manual fallback → `POST /farmer/{id}/leaf-condition`. Never blocks the flow. |
| `src/screens/Recommendation.jsx` | `GET /farmer/{id}/recommendation` — action + time window hero, plain-language message, animated confidence bar, consequence-if-ignored, conditional fertilizer/pest tips, "Why?" expandable with reasoning factors, SMS button → `POST /farmer/{id}/sms`. Environment summary strip → `GET /farmer/{id}/environment` (context only, fails silently). |
| `src/screens/History.jsx` | `GET /farmer/{id}/history` — soil moisture line chart (recharts) + chronological past-advice list |

## Endpoint contract used

- `POST /api/farmer/profile`
- `POST /api/farmer/{farmer_id}/photo` (multipart field: `photo`)
- `POST /api/farmer/{farmer_id}/leaf-condition` — `{ "condition": "normal" | "spotted" | "yellowing" | "wilting" }`
- `GET  /api/farmer/{farmer_id}/recommendation`
- `POST /api/farmer/{farmer_id}/sms`
- `GET  /api/farmer/{farmer_id}/history`
- `GET  /api/farmer/{farmer_id}/environment`

## Notes

- The app tolerates minor shape variations (e.g. `confidence` vs `confidence_pct`, trend as numbers or objects) so it works against mock and real backend data alike.
- "Start over" in the header clears the stored farmer id and returns to onboarding.
- Mobile-first, glassmorphism UI, reduced-motion friendly.
