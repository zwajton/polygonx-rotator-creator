# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A **single-file, no-build web app** — a Pokémon GO geofence rotator creator for the PolygonX scanner. The entire application lives in `index.html` (~1,000 lines). There is no package.json, no build step, no TypeScript, no framework. Open `index.html` directly in a browser to run it.

## Running the App

Open `index.html` directly in a browser — no server needed. For the hosted version: https://zwajton.github.io/polygonx-rotator-creator/

## Architecture

All state, logic, and UI live in a single `<script>` block in `index.html`. The structure:

- **Lines 11–195**: Embedded CSS with custom properties for dark/light theming
- **Lines 197–376**: HTML structure (header, sidebar, map container, modals)
- **Lines 378–1024**: All JavaScript

### Core State (lines 455–470)
```js
points[]     // [{lat, lon, radius, minuteOfDay?, name?}]
markers[]    // Leaflet markers (numbered circles)
circles[]    // Leaflet radius visualization circles
handles[]    // Draggable radius handles on map
activeIdx    // Selected point index
timingMode   // 'interval' or 'clock'
```

### Key Functions

| Function | Purpose |
|---|---|
| `render()` | Master render — redraws all map elements and the sidebar list |
| `addPoint(lat, lon, name, radius)` | Append a point to state and re-render |
| `exportJSON()` | Generate PolygonX rotator JSON and trigger browser download |
| `drawClock()` | SVG dual-ring UTC clock in the time picker modal |
| `checkSteps()` | Warn if any step distance exceeds 1 km (cooldown risk) |
| `parseCoords(text)` | Parse `lat,lon` lines from pasted text |

### Timing Modes
- **Interval**: All points share a fixed interval (minutes); the export fills 1440 min/day in a repeating loop
- **Clock (UTC)**: Each point has a specific `minuteOfDay`; the export sorts by time and validates completeness

### Export Format
The exported `rotator.json` follows the PolygonX format:
```json
{
  "latitude": ..., "longitude": ..., "radius": ...,
  "rotator24Shards": [{"latitude": ..., "longitude": ..., "minuteOfDay": ..., "radius": ...}]
}
```

### External Dependencies (CDN only)
- **Leaflet.js 1.9.4** — interactive map
- **Nominatim API** (OpenStreetMap) — city geocoding in the search bar
- **Google Fonts** — Space Mono & Syne

### Patterns to Follow
- State changes → call `render()` to redraw everything (no partial updates)
- Event handlers are inline `onclick` attributes in HTML
- Toast notifications via `showToast(msg)` (auto-hides after 2.8s)
- Theme switching updates CSS custom properties on `:root` and swaps Leaflet tile layers
- Radius handles use spherical geometry (`calcRadiusHandleLatLng`, `calcRadiusFromHandle`)
