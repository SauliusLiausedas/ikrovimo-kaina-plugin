---
name: pigiausia
description: Show the cheapest public EV charging right now in a Lithuanian city, along a route ("Vilnius - Klaipėda"), or nationwide when no place is given.
argument-hint: "[miestas | iš - į] [ac|dc|hpc]"
disable-model-invocation: true
---

Find the cheapest public EV charging in Lithuania for: `$ARGUMENTS`

1. Parse the arguments:
   - A price class word (`ac`, `dc`, `hpc`) sets `power_class`. With none, use `dc`.
   - Two places joined by `-`, `–`, `→` or "iki" are a route: `route {from, to}`.
   - One place is a `city`. Inflected forms ("Kaune") are fine as-is.
   - Nothing left means the national ranking: pass neither `city` nor `route`.
2. Call `cheapest_charging` once with `limit` 5.
3. Reply in Lithuanian unless the arguments are in another language, following the `ev-charging-lithuania` skill:
   the cheapest station first (name, address, network, price, max kW, free points), then the rest as a short table,
   the `source_url`, and the data credit.
