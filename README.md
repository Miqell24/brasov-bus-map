# Brașov Public Transport — interactive map

Interactive, poster-grade map of the public transport network of
**Brașov**: Regia Autonomă de Transport Brașov (RATBV)'s buses and trolleybuses — 84 lines drawn along the
real street geometry.

## Live

Not published — this map is built and reviewed locally.

One feed covers everything, split by `route_type` at build time:

| mode | route_type | lines | graph |
|---|---|---|---|
| buses | 3 | city and metropolitan lines, up to Poiana Brașov | OSM roadways |
| trolleybuses | 11 | 1, 2, 3, 6, 7, 8, 10, 31, 33 — drawn green on the bus network | OSM roadways |

Brașov has **no metro**, so the engine's metro treatment stays unused.

Build quirks worth knowing:

* **The feed is not the operator's.** RATBV publishes no GTFS, so the Mobility Database points at a community conversion of the OpenStreetMap route relations (osm2gtfs). Its shapes therefore come from the same OSM geometry the matcher runs against, so a low mean error here means “agrees with OSM”, not “agrees with reality” — and a line missing from OSM is simply missing from the map. Everything else in this family is fed by the operator itself.

* **The seventeen TE lines run once, one way.** *Transport elevi* — the school runs — carry a single trip each and no return, which is why a third of the map's lines show one direction.
* **Line numbers are unique across the modes**, so the line keys are the bare
  numbers printed on the vehicles — none of the mode prefixes the Sofia sibling
  needs. Re-check on every feed refresh.
* **Romanian is written in the Latin alphabet**, so this map runs without the
  second, transliterated label line its Greek, Bulgarian and Serbian siblings
  carry, and the stop names arrive properly cased and accented from the
  operator.
* **The feed's own `route_color` is ignored**, as everywhere in this family:
  colour means the MODE — navy bus, green trolleybus, red tram.

## Pipeline

`npm run download` fetches the GTFS, the OSM roadways and
MapLibre GL. `npm run build` map-matches every line (HMM/Viterbi on the OSM
graphs) and writes GeoJSON to `data/out/`. `npm run serve` hosts the map at
http://localhost:8145.

Data: Regia Autonomă de Transport Brașov (RATBV) · base map © OpenFreeMap / OpenMapTiles / OpenStreetMap
contributors.
