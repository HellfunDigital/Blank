# Pokémon Timekeeper UI Guide

## Running the App Locally
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Start the development server (exposes the UI on port 8000):
   ```bash
   python -c "from app import app; app.run(host='0.0.0.0', port=8000)"
   ```
3. Open `http://localhost:8000` in a browser to interact with the interface.

## Page Map

### 1. Pokédex Shell & Clock Face
The framed header recreates a Pokédex shell with illuminated status lights, a speaker grill, and a glassy display. Inside the clock face the current time, formatted date, active time-slot badge, and quick-throw Poké Ball buttons live on the same surface so you never leave the viewport to act.【F:templates/index.html†L11-L220】【F:templates/index.html†L990-L1055】

### 2. Encounter Viewport
A VHS-styled encounter viewport sits in the center of the clock. When no Pokémon appears the grass patch animates in place, and the placeholder text prompts you to keep watch; when a creature arrives its sprite walks across the feed while the summary panel below the clock updates with temperament and recommended ball guidance.【F:templates/index.html†L185-L362】【F:templates/index.html†L1013-L1055】【F:templates/index.html†L1324-L1402】

### 3. Adventure Modal
Opening the Adventure modal reveals the encounter dossier with slot badge, type chips, lore, temperament, difficulty, and best-ball hints plus mirrored inventory counters and catch controls for accessibility redundancy.【F:templates/index.html†L1064-L1138】【F:templates/index.html†L1324-L1402】【F:templates/index.html†L1418-L1471】

### 4. Collection Modal
The Collection modal surfaces total catches, unique species, shiny counts, and a responsive gallery that lists every recent capture or an empty-state prompt when nothing has been caught yet.【F:templates/index.html†L1143-L1160】【F:templates/index.html†L1531-L1570】

### 5. Log Modal
The Log modal streams the latest twelve events with success, fail, and info accents. A live preview also sits on the clock face so you can monitor the most recent activity without opening the overlay.【F:templates/index.html†L1163-L1176】【F:templates/index.html†L1499-L1528】

## Interaction Model & State
- **Time-based theming** comes from the `TIME_SLOTS` registry, which drives the background gradient, slot badges, and encounter roster filtering.【F:templates/index.html†L1183-L1225】【F:templates/index.html†L1314-L1317】
- **Encounter rotation** pulls from the `ROSTER` catalog keyed by slot, selecting artwork and metadata while falling back to swaying grass whenever no creature strolls by.【F:templates/index.html†L1228-L1258】【F:templates/index.html†L1324-L1402】【F:templates/index.html†L1613-L1634】
- **Inventory accrual** leverages `RESOURCE_SETTINGS` for tick intervals, amounts, and catch probabilities, with timers persisted per resource.【F:templates/index.html†L1261-L1264】【F:templates/index.html†L1418-L1434】
- **Player progress** lives in a single `state` object (inventory, timers, collection, and log) that is loaded from and saved to `localStorage`, ensuring persistence between sessions.【F:templates/index.html†L1271-L1306】

## Timers, Navigation, and Persistence
- The main loop updates the clock, accrues resources, and refreshes countdowns every 10 seconds, while encounter rolls trigger every minute and timer text refreshes every second.【F:templates/index.html†L1720-L1744】
- Modal launch buttons rely on `setupModals`, which manages focus, escape-key handling, backdrop dismissal, and body scroll locking while catch buttons reuse shared handlers for both the clock face and adventure overlay.【F:templates/index.html†L1043-L1055】【F:templates/index.html†L1654-L1660】【F:templates/index.html†L1700-L1718】
- Visibility-change listeners resync timers and the clock when the tab regains focus, preventing stale UI after backgrounding the app.【F:templates/index.html†L1746-L1752】

## Responsive & Accessibility Notes
The stylesheet introduces responsive padding, menu wrapping, and encounter viewport adjustments for smaller screens while marking decorative pieces `aria-hidden`. Modals declare `role="dialog"` with labelled headers, the viewport is `aria-live` for encounter changes, and live data regions (counts, timers, logs, and buttons) receive semantic updates for screen-reader clarity.【F:templates/index.html†L11-L983】【F:templates/index.html†L1013-L1176】【F:templates/index.html†L1499-L1528】【F:templates/index.html†L1700-L1718】

## Testing Checklist
- Confirm the live clock and slot badge update correctly when you alter the system time across slot boundaries.【F:templates/index.html†L1637-L1651】
- Ensure resource counts grow over time (or after tab focus changes) and that the log records each accrual or catch attempt.【F:templates/index.html†L1418-L1471】【F:templates/index.html†L1499-L1528】
- Attempt captures with depleted inventory to verify the disabled state and failure messaging.【F:templates/index.html†L1573-L1610】
