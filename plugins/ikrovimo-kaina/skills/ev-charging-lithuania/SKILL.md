---
name: ev-charging-lithuania
description: Answer questions about public electric-car charging in Lithuania — where to charge, the cheapest charging in a city, on a route or nationwide, a network's tariffs and subscriptions, what a charge costs, recent price changes, and Nord Pool electricity prices for home charging. Use for "kur pigiausia įkrauti", "kiek kainuoja įkrauti", Ignitis ON / Eldrive / other Lithuanian charging networks, or any EV charging question set in Lithuania.
---

# EV charging in Lithuania (ikrovimo-kaina.lt)

The `ikrovimo-kaina` MCP server bundled with this plugin serves live prices from the Via Lietuva open data feed,
processed by ikrovimo-kaina.lt. Never quote a charging price from memory — prices change weekly. Call a tool.

## Picking the tool

| The user wants | Tool | Notes |
|---|---|---|
| Stations near a place or in a city | `find_stations` | `near` needs coordinates. Only pass them if you already have them; otherwise use `city`. |
| The cheapest charging in a city / along a drive / nationally | `cheapest_charging` | `route {from, to}` is slower and rate limited (10/min) — use it once per question. |
| One network's prices, fees and plans | `operator_tariffs` | Brand, legal name or slug all work ("Ignitis ON", "eldrive"). |
| What a charge costs | `charging_cost` | Give `kwh`, or `car_battery_kwh` with `from_pct`/`to_pct` (defaults 20→80). |
| Who raised or cut prices | `price_changes` | `since_date` is at most 365 days back, Europe/Vilnius dates. |
| When electricity is cheapest for home charging | `electricity_price` | Wholesale, ct/kWh **excluding** VAT. Tomorrow's prices appear around 14:00 Vilnius time. |

## Price classes

Every charging price is EUR/kWh **including VAT**, ad hoc (no subscription). Map the user's words to a class:

- `ac` — AC, "lėtas", destination/home-style chargers, usually up to 22 kW.
- `dc` — DC fast charging under 150 kW, "greitas".
- `hpc` — high-power DC at 150 kW or more, "itin greitas", "ultra-fast".

If the user says just "greitas" / "fast", ask nothing — call `dc` and mention that `hpc` also exists if prices differ
much. If they name no class and the question is about cost, prefer `dc`, the class most road-trip questions mean.

## Car battery sizes

When the user names a car but not its battery, use its usable capacity if you know it confidently (e.g. VW ID.3 Pro
≈ 58 kWh, Tesla Model 3 Long Range ≈ 75 kWh) and say which value you assumed. If unsure, ask.

## Getting the answer right

- **Extra fees.** Charging prices exclude per-minute, parking and session fees. When a result has `extra_fees: true`,
  say the network may charge more than the kWh price alone.
- **Wholesale is not what drivers pay.** `electricity_price` is the Nord Pool day-ahead market price without VAT,
  network fees or supplier margin. Never compare it with public charging prices as if they were the same thing.
- **Unknown city or operator** comes back as a tool error with a hint. Follow the hint (e.g. retry with the base
  city name) rather than giving up.
- **Coverage is Lithuania only.** For other countries, say this source does not cover them.
- **Live availability** (`evses_available`) is a snapshot; say "reported free now", not "guaranteed free".

## Answering

- Answer in the user's language. Lithuanian users get Lithuanian answers with € and comma decimals (0,25 €/kWh).
- Lead with the answer (the cheapest station, the cost), then 2–5 supporting rows. Don't dump raw JSON.
- Link the `source_url` of the data you used — the station, city or network page.
- End with the credit the data license requires:
  - LT: *Duomenys: Via Lietuva, CC BY 4.0; ikrovimo-kaina.lt*
  - EN: *Data: Via Lietuva, CC BY 4.0; ikrovimo-kaina.lt*
