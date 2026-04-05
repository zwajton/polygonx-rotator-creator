# ⬡ Rotator Builder

A standalone HTML tool for creating Pokémon GO geofence rotator JSON files. No installation required — just open `rotator-builder.html` in any browser.

---

## Getting Started

1. Open https://zwajton.github.io/polygonx-rotator-creator/
3. Start adding points and export your rotator

---

## Adding Points

You have three ways to add scan points:

**Click the map** — click anywhere on the map to drop a point there.

**Search** — use the search bar in the top header to find a city by name, or type coordinates directly as `lat,lon` to jump there.

**Hotspots** — click the 📍 Hotspots button to open a panel with 68 pre-loaded locations around the world, sorted by timezone. Click any to add it as a point.

**Paste** — paste a list of coordinates (one `lat,lon` per line) into the Paste Coordinates box and click Add or Replace All.

---

## Timing Modes

Switch between the two modes under **Settings → Timing mode**.

### ⏱ Interval mode
All points are visited in order on a repeating loop. Set a fixed number of minutes between each point. Best for local nest scanning where you want continuous sweeps — e.g. 10 points at 4-minute intervals = 40 min per sweep = 36 sweeps per day.

### 🕐 Clock mode (UTC)
Each point gets its own specific time of day in UTC. Best for global multi-city rotators where you want to be in Wellington at 23:00 UTC, Taipei at 04:00 UTC, etc.

When Clock mode is active, each point shows a **Set time** button. The time picker has three input methods:
- **Clock** — drag the clock hand or click a number (hour → minute)
- **Manual** — type the hour (0–23) and minute directly
- **UNIX** — paste a Unix timestamp; the UTC hour:minute is extracted. Hit **Now** to use the current time.

The picker always shows both the UTC time and the approximate local time for that coordinate.

---

## Adjusting Radius

Each point has its own radius (in meters), independent of the others.

**Drag on map** — every point has a small dot on its east edge. Drag it inward or outward to resize the circle visually.

**Sidebar input** — each point card has a number input where you can type an exact radius.

**Default radius** — the *Geofence radius* field in Settings sets the default for newly added points.

---

## Settings

| Field | Description |
|---|---|
| Geofence radius (m) | Default shard radius for new points |
| Interval (min) | Minutes between each point (Interval mode only) |
| Geofence center lat/lon | Center coordinate of the main geofence circle (shown in yellow on map) |
| Geofence boundary radius (m) | Radius of the outer geofence boundary circle |

---

## Import / Export

**Export** — click ⬇ Export to download `rotator.json`, ready to import into PolygonX or similar apps.

**Load JSON** — load an existing rotator JSON to view and edit it. Points, timing, and radius are all restored automatically.

---

## Cooldown Warning

The tool checks the distance between consecutive points and warns you if any step exceeds 1 km, which risks triggering Pokémon GO's travel cooldown.

---

## Interface

| Control | Function |
|---|---|
| 🌙 / ☀️ | Toggle dark / light map and UI theme |
| A- / A+ | Make the interface text smaller or larger |
| 📍 Hotspots | Open the favorite locations panel |
| Search bar | Search for any city or type `lat,lon` to jump there |

---

## Coordinate Format

Coordinates must be in decimal degrees with a dot as the decimal separator:

```
35.640153,139.868965
35.641023,139.866672
```

---

## Notes

- The local time shown for each coordinate is an approximation based on longitude (UTC offset = longitude ÷ 15, rounded). It does not account for DST or political timezone boundaries.
- Internet connection is required for map tiles and city search (Nominatim). The tool itself runs fully offline once loaded.