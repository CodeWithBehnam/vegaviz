# Deneb Reference for Chart Authors

## Runtime Versions (Deneb 1.9.0)

- Vega: 6.2.0
- Vega-Lite: 6.4.1
- Schema URIs still use v5: `https://vega.github.io/schema/vega-lite/v5.json`

## Data Binding

All specs MUST use `"data": { "name": "dataset" }`. Deneb injects Power BI fields into this named source.

## System Fields (auto-added to every row)

| Field | Type | Purpose |
|---|---|---|
| `__row__` | integer | Row index for interactivity |
| `__selected__` | boolean | Cross-filter selection state |
| `[measure]__format` | string | Power BI format string |
| `[measure]__formatted` | string | Pre-formatted display value |
| `[measure]__highlight` | number | Highlight value from cross-highlight |

## Template Placeholders

Use `__FieldName__` pattern (double underscores) for template fields. Users map their Power BI fields to these placeholders on import.

## Custom Expression Functions

| Function | Usage |
|---|---|
| `pbiFormat(value, formatString)` | Apply Power BI format strings |
| `pbiFormatAutoUnit(value, formatString)` | Auto-unit formatting (K/M/Bn) |
| `pbiColor(index)` | Theme color by index (0-based) or name |
| `pbiColor('min')` / `'max'` / `'good'` / `'bad'` | Named sentiment colors |
| `pbiPatternSVG(patternId, fg, bg)` | SVG pattern fill (SVG renderer only) |
| `pbiCrossFilterApply(event, expr)` | Cross-filter trigger (Vega only) |
| `pbiCrossFilterClear()` | Clear cross-filter (Vega only) |

## Color Schemes

- `pbiColorNominal` — categorical palette
- `pbiColorOrdinal` — ordinal ramp
- `pbiColorLinear` — continuous gradient (min to max)
- `pbiColorDivergent` — diverging gradient (min to middle to max)

## Sizing

ALWAYS use `"width": "container"` and `"height": "container"` for responsive sizing.

For Vega specs, use signals:
```json
"width": { "signal": "containerSize()[0]" },
"height": { "signal": "containerSize()[1]" }
```

## Limitations

- Row limit: 10,000 default (can be overridden in visual settings)
- AppSource version: no external URLs (data URIs OK)
- Data transforms (fold, flatten, aggregate) break cross-filtering
- Pattern fills: SVG renderer only
- Cross-filter apply/clear: Vega only, not Vega-Lite
- Field names with special chars (`.[]\"`) are replaced with underscores

## usermeta Block

Every template spec needs a `usermeta` block for Deneb import:
```json
{
  "usermeta": {
    "deneb": {
      "build": "1.9.0.0",
      "metaVersion": 1,
      "provider": "vegaLite",
      "providerVersion": "6.4.1"
    },
    "information": {
      "name": "Chart Name",
      "description": "...",
      "author": "...",
      "uuid": "unique-id",
      "generated": "ISO-date"
    },
    "dataset": [
      { "key": "__FieldName__", "name": "Display Name", "description": "", "type": "text|numeric|dateTime|bool", "kind": "column|measure|any" }
    ],
    "interactivity": {
      "tooltip": true,
      "contextMenu": true,
      "selection": false,
      "selectionMode": "simple",
      "highlight": false,
      "dataPointLimit": 50
    }
  }
}
```
