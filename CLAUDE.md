# Project: vegaviz

Reusable Vega and Vega-Lite visualization specs for Power BI (Deneb), web dashboards, and standalone use.

## Stack

- Vega-Lite (primary) / Vega (when Vega-Lite can't express it)
- JSON spec files
- Power BI + Deneb custom visual for rendering
- Python 3.12+ with `uv` for data prep and testing notebooks

## Architecture

```
charts/
  bar/              # Grouped, stacked, diverging bar charts
  line/             # Line, area, actual vs target
  scatter/          # Scatter, bubble, strip plots
  donut/            # Donut and pie charts
  heatmap/          # Heatmaps and matrix displays
  gantt/            # Gantt and timeline charts
  sankey/           # Sankey flow diagrams (Vega)
  treemap/          # Treemap hierarchies
  kpi/              # KPI cards and gauges
  waffle/           # Waffle and unit charts
  bullet/           # Bullet charts
  dumbbell/         # Dumbbell / lollipop comparisons
  waterfall/        # Waterfall charts
  funnel/           # Funnel charts
  combo/            # Multi-layer composite charts
  map/              # Geographic and choropleth
  network/          # Force-directed graphs, org charts (Vega)
  other/            # Anything that doesn't fit above
templates/
  base/             # Vega-Lite and Vega starter templates
  themes/           # Power BI theme configs (Segoe UI, pbiColor schemes)
data/sample/        # Sample datasets for testing
notebooks/          # Python notebooks for data prep
ai_docs/            # Deneb reference docs (expressions, usermeta, limits)
```

Each chart directory contains:
- `<chart-name>.vl.json` — Vega-Lite spec
- `<chart-name>.vg.json` — Vega spec (only when Vega-Lite is insufficient)
- `README.md` — screenshot, description, Power BI field mapping

## Commands

```bash
uv run python -m json.tool charts/**/*.json   # Validate JSON syntax
uv run jupyter lab                             # Open notebooks for testing
```

## Code Conventions

- Vega-Lite specs use `$schema` pointing to the latest stable schema URL
- All specs must work with Deneb's dataset binding (`"data": {"name": "dataset"}`)
- Use named signals and parameters for interactivity — never hardcode filter values
- Color palettes follow Power BI theme tokens when possible (`pbiColorX`)
- Keep specs self-contained — no external data URLs in final chart specs

## Constraints

NEVER hardcode data URLs in chart specs — Deneb injects data via `"dataset"`. Use sample data only in `data/` for testing.
NEVER use Vega when Vega-Lite can express the same chart — Vega-Lite is simpler to maintain and more portable.
NEVER use pixel-based sizing — use `"width": "container"` and `"height": "container"` for responsive Deneb rendering.
ALWAYS include `$schema` in every spec file — editors and Deneb need it for validation.
ALWAYS test specs with sample data before committing — broken JSON or missing fields silently fail in Deneb.
ALWAYS use `uv` for Python tasks — pip/poetry are not configured.

## Adding a New Chart

1. Pick the correct category directory under `charts/`
2. Create `<chart-name>.vl.json` with `"data": {"name": "dataset"}` as the data source
3. Use `"width": "container"` and `"height": "container"` for sizing
4. Add sample data to `data/` if a new dataset is needed
5. Test in a notebook or Vega Editor (https://vega.github.io/editor/)
6. Add a `README.md` in the chart directory with a screenshot and Power BI field mapping
7. Verify the spec renders in Deneb before committing

## Deneb-Specific Notes

- Deneb 1.9 ships Vega 6.2.0 / Vega-Lite 6.4.1 — schema URIs still use v5 paths
- Data source is always `"data": {"name": "dataset"}` — Deneb injects Power BI fields there
- Use `datum["Field Name"]` syntax for field names with spaces
- Template placeholders use `__FieldName__` pattern (double underscores)
- Custom expressions: `pbiColor(index)`, `pbiFormat(val, fmt)`, `pbiPatternSVG(id, fg, bg)`
- Color schemes: `pbiColorNominal`, `pbiColorOrdinal`, `pbiColorLinear`, `pbiColorDivergent`
- Data transforms (fold, flatten, aggregate) break cross-filtering — avoid if selection needed
- Cross-filter apply/clear is Vega-only — not available in Vega-Lite
- Row limit: 10,000 default — can be overridden in visual settings
- AppSource version blocks external URLs — use data URIs for images
- Every spec needs a `usermeta` block for template import — see `ai_docs/deneb-reference.md`
