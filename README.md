# vegaviz

Reusable Vega and Vega-Lite visualization specs for Power BI (Deneb) and beyond.

## Structure

```
charts/          Visualization specs organized by chart type
  bar/           Grouped, stacked, diverging bar charts
  line/          Line, area, actual vs target
  scatter/       Scatter, bubble, strip plots
  donut/         Donut and pie charts
  heatmap/       Heatmaps and matrix displays
  gantt/         Gantt and timeline charts
  sankey/        Sankey flow diagrams
  treemap/       Treemap hierarchies
  kpi/           KPI cards and gauges
  waffle/        Waffle and unit charts
  bullet/        Bullet charts
  dumbbell/      Dumbbell / lollipop comparisons
  waterfall/     Waterfall charts
  funnel/        Funnel charts
  combo/         Multi-layer composite charts
  map/           Geographic visualizations
  network/       Force-directed graphs, org charts
  other/         Anything that doesn't fit above
templates/       Base spec templates and themes
data/sample/     Sample datasets for testing
notebooks/       Python notebooks for data prep
ai_docs/         Deneb reference docs for AI agents
```

## Usage in Deneb (Power BI)

1. Copy the JSON spec from any `charts/` file
2. In Power BI, add a Deneb visual
3. Paste the spec into the editor
4. Map your Power BI fields to the template placeholders (`__FieldName__`)

All specs use `"data": {"name": "dataset"}` and `"width"/"height": "container"` for Deneb compatibility.

## Conventions

- `.vl.json` = Vega-Lite spec
- `.vg.json` = Vega spec (used only when Vega-Lite is insufficient)
- Every spec includes a `usermeta` block for Deneb template import
- Color schemes use Power BI theme tokens (`pbiColorNominal`, etc.)
