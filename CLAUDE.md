# Project: vegaviz

The definitive open-source Vega & Vega-Lite chart gallery for Power BI Deneb and beyond. 275+ specs ready to copy-paste.

## Stack

- Vega-Lite v5 (primary) / Vega v5 (when Vega-Lite can't express it)
- JSON spec files (`.vl.json` = Vega-Lite, `.vg.json` = Vega)
- Deneb 1.9 custom visual (runtime: Vega 6.2.0 / Vega-Lite 6.4.1)
- Python 3.12+ with `uv` for data prep notebooks
- GitHub Pages gallery on `gh-pages` branch (single `index.html`, pako + vega-embed)

## Architecture

```
charts/
  bar/ line/ scatter/ donut/ heatmap/   # Core chart categories
  gantt/ bullet/ kpi/ waffle/ dumbbell/ # Specialized charts
  waterfall/ funnel/ combo/ map/        # More categories
  sankey/ treemap/ network/ other/      # Vega-only and misc
  community/                            # 248 specs from 10 external sources
    bar/ line/ scatter/ ...             # Mirrors core category structure
templates/
  base/           # Frozen starter templates -- copy, do NOT modify
  themes/         # Power BI theme configs (Segoe UI, pbiColor schemes)
data/
  sample/         # Sample datasets (sales, timeseries, kpi)
  hex/            # Hex cartogram coordinates (UK, Wales LAs, Swansea/NPT MSOAs)
ai_docs/          # Deneb API reference for agents -- see deneb-reference.md
notebooks/        # Python notebooks for data prep
docs/             # Gallery screenshot and assets
.github/          # Issue templates (new-chart-request, bug-report)
```

## Commands

```bash
uv run python -m json.tool charts/**/*.json   # Validate JSON syntax
uv run jupyter lab                             # Open notebooks for testing
git checkout gh-pages                          # Switch to gallery branch
```

## Constraints

NEVER hardcode data in specs -- Deneb injects via `"data": {"name": "dataset"}`. Sample data goes in `data/` only.
NEVER use Vega when Vega-Lite can express it -- Vega-Lite is simpler and more portable.
NEVER use pixel sizing -- use `"width": "container"`, `"height": "container"` for responsive rendering.
NEVER use transforms (fold, flatten, aggregate) if cross-filtering is needed -- they break `__row__` linkage.
NEVER invent Deneb features -- check `ai_docs/deneb-reference.md` first. Do not hallucinate expressions.
NEVER modify files in `templates/base/` -- they are frozen boilerplate. Copy them to start new charts.
NEVER use em-dashes in any output -- use regular dashes or double dashes instead.
ALWAYS include `$schema` pointing to v5 -- editors and Deneb need it for validation.
ALWAYS include `usermeta` block with dataset field mappings -- required for Deneb template import.
ALWAYS use `__FieldName__` placeholder pattern (double underscores) -- Deneb substitutes these on import.
ALWAYS use `uv` for Python tasks -- pip/poetry are not configured.
ALWAYS run `git checkout gh-pages` before editing the gallery -- `index.html` only exists on that branch.

## Adding a New Chart

1. Pick the correct category under `charts/`
2. Copy `templates/base/vegalite-base.json` as starting point
3. Set `"data": {"name": "dataset"}`, `"width": "container"`, `"height": "container"`
4. Use `__FieldName__` placeholders for all field references
5. Use `pbiColorNominal` / `pbiColor(index)` for colors -- not hardcoded hex values
6. Fill in `usermeta`: name, description, dataset field mappings, interactivity flags
7. Add sample data to `data/sample/` if a new dataset shape is needed
8. Test in Vega Editor (https://vega.github.io/editor/) with sample data
9. Switch to `gh-pages` branch, add entry to `CHARTS` array in `index.html`
10. Add matching sample data to `SAMPLE_DATA` object if new `dataKey` is needed

## Adding Community Charts

1. Place specs in `charts/community/<category>/` mirroring core structure
2. Set `community: true`, `source`, `sourceUrl`, and `provider` ('vega' or 'vegaLite') in the CHARTS entry
3. Community specs keep their original data bindings -- do not rewrite to match vegaviz conventions

## Gallery (gh-pages branch)

The gallery is a single `index.html` with:
- `CHARTS` array -- one entry per chart with file path, name, category, dataKey, accent color
- `SAMPLE_DATA` object -- keyed sample datasets for preview rendering
- `specToEditorUrl()` -- compresses specs with pako.deflate for Vega Editor links
- `injectData()` -- recursively walks layer/concat/hconcat/vconcat/spec/facet to inject sample data
- `replacePlaceholders()` / `replacePbiColors()` -- resolve Deneb-specific syntax for previews

## Deneb Quick Reference

- Data source: `"data": {"name": "dataset"}` -- always
- Field names with spaces: `datum["Field Name"]`
- Expressions: `pbiColor(0)`, `pbiFormat(val, fmt)`, `pbiPatternSVG(id, fg, bg)`
- Color schemes: `pbiColorNominal`, `pbiColorOrdinal`, `pbiColorLinear`, `pbiColorDivergent`
- Cross-filter: Vega only -- not available in Vega-Lite
- Row limit: 10,000 default -- override in Deneb visual settings
- Full reference: `ai_docs/deneb-reference.md`
