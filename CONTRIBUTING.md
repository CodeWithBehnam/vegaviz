# Contributing to vegaviz

Thanks for your interest in contributing! This project aims to be the most comprehensive
Vega & Vega-Lite chart gallery for Power BI Deneb and beyond.

## Ways to Contribute

- **Add a new chart** - the most impactful way to help
- **Improve an existing chart** - better defaults, more interactivity, cleaner spec
- **Fix a bug** - broken JSON, incorrect field mappings, rendering issues
- **Add sample data** - new datasets that enable different chart types
- **Improve docs** - better descriptions, screenshots, usage guides

## Adding a New Chart

### 1. Pick a category

Choose the right directory under `charts/`. If none fits, use `charts/other/` or propose a new category.

### 2. Start from the template

Copy `templates/base/vegalite-base.json` as your starting point. Only use Vega (`templates/base/vega-base.json`) when Vega-Lite cannot express the chart (e.g., force-directed graphs, sankey diagrams).

### 3. Follow these rules

- **Data source**: `"data": {"name": "dataset"}` - always. Deneb injects Power BI fields here.
- **Sizing**: `"width": "container"` and `"height": "container"` for responsive rendering.
- **Placeholders**: Use `__FieldName__` pattern (double underscores) for all field references. Deneb substitutes these when users import the template.
- **Colors**: Use `pbiColorNominal` scheme or `pbiColor(index)` expressions. Avoid hardcoded hex values.
- **usermeta block**: Fill in name, description, dataset field mappings (key, name, type, kind), and interactivity flags. This is required for Deneb template import.
- **Schema**: Include `"$schema": "https://vega.github.io/schema/vega-lite/v5.json"` at the top.

### 4. File naming

- Vega-Lite: `<chart-name>.vl.json`
- Vega: `<chart-name>.vg.json`

Use lowercase kebab-case for file names (e.g., `grouped-bar.vl.json`).

### 5. Test your chart

- Validate JSON syntax: `uv run python -m json.tool charts/<category>/<chart>.vl.json`
- Test in [Vega Editor](https://vega.github.io/editor/) with sample data from `data/sample/`
- If possible, verify in Deneb inside Power BI

### 6. Update the gallery

If you want your chart to appear on the [gallery site](https://codewithbehnam.github.io/vegaviz/), add an entry to the `CHARTS` array in the `gh-pages` branch `index.html`. Include: file path, name, category, description, and accent color.

### 7. Submit a PR

- One chart per PR (unless they are closely related variants)
- Include a brief description of the chart and when it's useful
- Link to any inspiration source if applicable

## Code Style

- JSON files must be valid and properly indented (2 spaces)
- No trailing commas in JSON
- No external data URLs in chart specs
- No pixel-based sizing

## Reporting Issues

Use the issue templates provided:

- **New Chart Request** - suggest a chart type you'd like to see
- **Bug Report** - report broken specs, rendering issues, or gallery problems

## Questions?

Open a [Discussion](https://github.com/CodeWithBehnam/vegaviz/discussions) or file an issue. We're happy to help.
