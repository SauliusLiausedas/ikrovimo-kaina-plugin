# ikrovimo-kaina — Claude Code plugin

Live public EV charging prices in Lithuania, from [ikrovimo-kaina.lt](https://ikrovimo-kaina.lt), inside Claude Code.

Lietuviškai: elektromobilių įkrovimo kainos, stotelės ir tinklai Lietuvoje tiesiai Claude Code.

## Install

```
/plugin marketplace add SauliusLiausedas/ikrovimo-kaina-plugin
/plugin install ikrovimo-kaina@ikrovimo-kaina
```

No API key, no account. The plugin connects to the public, read-only MCP server at `https://ikrovimo-kaina.lt/mcp`.

## What you get

| Component | What it does |
|---|---|
| MCP server `ikrovimo-kaina` | `find_stations`, `cheapest_charging`, `operator_tariffs`, `charging_cost`, `price_changes`, `electricity_price` |
| Skill `ev-charging-lithuania` | Loads automatically for Lithuanian EV charging questions: picks the right tool, maps "greitas"/"fast" to price classes, flags extra fees, keeps wholesale and retail prices apart, credits the data source |
| `/ikrovimo-kaina:pigiausia [miestas \| iš - į] [ac\|dc\|hpc]` | The cheapest charging right now in a city, along a route, or nationwide |

Examples:

```
/ikrovimo-kaina:pigiausia Kaune
/ikrovimo-kaina:pigiausia Vilnius - Klaipėda hpc
Kiek kainuos įkrauti ID.3 nuo 15 iki 80 % Ignitis ON DC stotelėje?
When is electricity cheapest tomorrow for charging at home?
```

## Develop

```
claude plugin validate --strict .
claude --plugin-dir plugins/ikrovimo-kaina
```

## Data and limits

Prices are EUR/kWh incl. VAT, ad hoc (no subscription), Lithuania only. 60 MCP requests per minute per IP; route
searches 10 per minute. Data: Via Lietuva (ev.vialietuva.lt), **CC BY 4.0**; processed by ikrovimo-kaina.lt — keep the
credit when you share results. Server docs: https://ikrovimo-kaina.lt/api#mcp · contact info@ikrovimo-kaina.lt

## License

The plugin (manifests, skills, docs in this repository) is released under the [MIT License](LICENSE).
The charging data it fetches is not covered by that license: it is Via Lietuva open data under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), processed by ikrovimo-kaina.lt.
