<p align="center">
  <a href="https://codewithbehnam.github.io/vegaviz/">
    <img src="docs/gallery-preview.png" alt="vegaviz gallery preview" width="100%">
  </a>
</p>

<h1 align="center">vegaviz</h1>

<p align="center">
  <strong>The only Vega & Vega-Lite chart gallery you need.</strong><br>
  Copy-paste specs into Power BI Deneb or anywhere else.
</p>

<p align="center">
  <a href="https://codewithbehnam.github.io/vegaviz/"><img src="https://img.shields.io/badge/Gallery-Live_Demo-FF6B35?style=for-the-badge" alt="Live Gallery"></a>
  <a href="#charts"><img src="https://img.shields.io/badge/Charts-23+-4ECDC4?style=for-the-badge" alt="23+ Charts"></a>
  <a href="https://github.com/CodeWithBehnam/vegaviz/stargazers"><img src="https://img.shields.io/github/stars/CodeWithBehnam/vegaviz?style=for-the-badge&color=FFE66D" alt="Stars"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-7B68EE?style=for-the-badge" alt="MIT License"></a>
</p>

<p align="center">
  <code>vega-lite</code> · <code>vega</code> · <code>deneb</code> · <code>power-bi</code> · <code>data-visualization</code> · <code>charts</code>
</p>

---

## What is this?

A curated collection of **ready-to-use Vega-Lite and Vega chart specs** designed for:

- **Power BI Deneb** custom visual (primary target)
- **Vega Editor** for standalone exploration
- **Any platform** that renders Vega/Vega-Lite specs

Every chart includes a complete `usermeta` block so you can import it directly as a Deneb template with field mapping.

## Charts

| Category | Charts |
|----------|--------|
| **Bar** | Grouped, Stacked, Diverging |
| **Line** | Multi-Line, Actual vs Target, Cumulative, Sparkline |
| **Area** | Stacked Area |
| **Scatter** | Bubble, Boxplot + Jitter |
| **Donut** | Donut with labels |
| **Heatmap** | Matrix Heatmap, Calendar View |
| **Gantt** | Gantt with today marker |
| **Bullet** | Actual vs Target ranges |
| **KPI** | KPI Card with variance indicator |
| **Dumbbell** | Two-value connected dot comparison |
| **Waffle** | Percentage grid |
| **Waterfall** | Cumulative positive/negative steps |
| **Funnel** | Stage-based narrowing |
| **Combo** | Bar + Line dual axis |
| **Other** | Tadpole, Rank/Bump |

## Quick Start

### In Power BI Deneb

1. Open the [gallery](https://codewithbehnam.github.io/vegaviz/) and find a chart you like
2. Click **Copy Spec** to copy the full JSON
3. In Power BI, add a Deneb visual to your report
4. Paste the spec into the Deneb editor
5. Map your Power BI fields to the `__FieldName__` placeholders

### In Vega Editor

Click **Vega Editor** on any card in the gallery to open it directly in the online editor with live preview.

## Project Structure

```
charts/          Chart specs organized by type (.vl.json / .vg.json)
templates/       Frozen base templates and Power BI theme configs
data/sample/     Sample datasets for testing (sales, timeseries, kpi)
ai_docs/         Deneb API reference for agents and contributors
docs/            Screenshots and assets
```

## Deneb Compatibility

- Targets **Deneb 1.9** (Vega 6.2.0 / Vega-Lite 6.4.1)
- All specs use `"data": {"name": "dataset"}` for Deneb data binding
- Responsive sizing with `"width": "container"` / `"height": "container"`
- Power BI theme colors via `pbiColorNominal`, `pbiColor(index)`, etc.
- Template placeholders use `__FieldName__` pattern for field mapping

## Contributing

1. Pick a chart type from the `charts/` directory (or create a new category)
2. Copy `templates/base/vegalite-base.json` as your starting point
3. Use `__FieldName__` placeholders and `pbiColor` schemes
4. Test in [Vega Editor](https://vega.github.io/editor/) with sample data
5. Submit a PR with your spec

## Star History

<a href="https://star-history.com/#CodeWithBehnam/vegaviz&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=CodeWithBehnam/vegaviz&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=CodeWithBehnam/vegaviz&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=CodeWithBehnam/vegaviz&type=Date" />
 </picture>
</a>

## License

MIT
