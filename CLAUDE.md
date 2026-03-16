# Project: vegaviz

The ultimate open-source Vega & Vega-Lite chart gallery. Copy-paste specs for Power BI Deneb and beyond.

## Stack

- Vega-Lite v5 (primary) / Vega v5 (only when Vega-Lite can't express it)
- JSON spec files (`.vl.json` = Vega-Lite, `.vg.json` = Vega)
- Deneb 1.9 custom visual (Vega 6.2.0 / Vega-Lite 6.4.1 runtime)
- Python 3.12+ with `uv` for data prep notebooks
- GitHub Pages gallery on `gh-pages` branch (single `index.html`)

## Architecture

```
charts/
  bar/              # Grouped, stacked, diverging
  line/             # Line, area, sparkline, actual vs target, cumulative
  scatter/          # Scatter, bubble, boxplot + jitter
  donut/            # Donut and pie
  heatmap/          # Matrix heatmap, calendar view
  gantt/            # Gantt with today marker
  sankey/           # Sankey flow (Vega only)
  treemap/          # Treemap hierarchies
  kpi/              # KPI cards with variance
  waffle/           # Waffle percentage grids
  bullet/           # Actual vs target ranges
  dumbbell/         # Two-value connected dot comparison
  waterfall/        # Cumulative pos/neg steps
  funnel/           # Stage-based narrowing
  combo/            # Bar + line dual axis
  map/              # Geographic, hex-tile
  network/          # Force-directed, org charts (Vega only)
  other/            # Tadpole, rank/bump, anything else
templates/
  base/             # Frozen starter templates -copy, do NOT modify
  themes/           # Power BI theme configs (Segoe UI, pbiColor schemes)
data/sample/        # Sample datasets (sales, timeseries, kpi)
notebooks/          # Python notebooks for data prep
ai_docs/            # Deneb reference for agents -see deneb-reference.md
```

## Commands

```bash
uv run python -m json.tool charts/**/*.json   # Validate JSON syntax
uv run jupyter lab                             # Open notebooks for testing
```

## Constraints

NEVER hardcode data URLs in chart specs -Deneb injects data via `"dataset"`. Sample data goes in `data/` only.
NEVER use Vega when Vega-Lite can express the same chart -Vega-Lite is simpler to maintain and portable.
NEVER use pixel-based sizing -use `"width": "container"` and `"height": "container"` for responsive rendering.
NEVER use data transforms (fold, flatten, aggregate) if cross-filtering is needed -they break `__row__` linkage.
NEVER invent Deneb features -check `ai_docs/deneb-reference.md` for actual API. Do not hallucinate expressions.
NEVER modify files in `templates/base/` -they are frozen boilerplate. Copy them to start new charts.
ALWAYS include `$schema` in every spec -editors and Deneb need it for validation.
ALWAYS include `usermeta` block with dataset field mappings -required for Deneb template import.
ALWAYS use `__FieldName__` placeholder pattern (double underscores) -Deneb substitutes these on import.
ALWAYS use `uv` for Python tasks -pip/poetry are not configured.

## Adding a New Chart

1. Pick the correct category under `charts/`
2. Copy `templates/base/vegalite-base.json` as starting point
3. Set `"data": {"name": "dataset"}`, `"width": "container"`, `"height": "container"`
4. Use `__FieldName__` placeholders for all field references
5. Use `pbiColorNominal` / `pbiColor(index)` for colors -not hardcoded hex
6. Fill in the `usermeta` block: name, description, dataset field mappings, interactivity flags
7. Add sample data to `data/sample/` if a new dataset is needed
8. Test in Vega Editor (https://vega.github.io/editor/) with sample data
9. Add chart entry to `CHARTS` array in `gh-pages` branch `index.html` for gallery display

## Deneb Quick Reference

- Schema URIs use v5 paths: `https://vega.github.io/schema/vega-lite/v5.json`
- Data source: `"data": {"name": "dataset"}` -always
- Field names with spaces: `datum["Field Name"]`
- Expressions: `pbiColor(0)`, `pbiFormat(val, fmt)`, `pbiPatternSVG(id, fg, bg)`
- Schemes: `pbiColorNominal`, `pbiColorOrdinal`, `pbiColorLinear`, `pbiColorDivergent`
- Cross-filter apply/clear: Vega only -not in Vega-Lite
- Row limit: 10,000 default -override in visual settings
- Full reference: `ai_docs/deneb-reference.md`
