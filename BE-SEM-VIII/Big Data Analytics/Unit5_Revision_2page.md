# Unit 5 — Big Data Visualization (2-page revision)

Purpose: cover challenges, key techniques, tool highlights, chart selection guidance, and quick how-tos for Tableau and Google Charts.

1. Challenges & how to mitigate
- Challenges: volume, velocity, variety, visual clutter, scalability, data quality, and human perception limits.
- Mitigations: aggregation (group by time windows), sampling (random/stratified), incremental rendering, progressive disclosure (show summary then drill-down), dimensionality reduction (PCA/t-SNE), and interactive filtering.

2. Choosing the right visualization
- Temporal data: line charts, area charts, sparklines; use aggregation (hour/day/month) for large series.
- Comparison: bar/column charts, grouped bars; normalize when scales differ.
- Distribution: histograms, box plots, violin plots; show outliers separately when needed.
- Relationships: scatter plots, bubble charts, correlation heatmaps; add regression line when helpful.
- Hierarchical & part-to-whole: treemap, sunburst; avoid too many leaf nodes.
- Geospatial: choropleth for density, symbol maps for precise locations; use map tiling for performance.

3. Tableau quick commands & features
- Workflow: Connect Data -> Prepare (clean/blend) -> Create Worksheets -> Build Dashboard -> Publish.
- Key features: calculated fields, parameters, LOD expressions, filters/actions, show/hide containers, map layers, forecasting, clustering.
- Exam cue: list 3 advanced features (LOD, parameters, forecasting) and a short use-case for each.

4. Google Charts — quick how-to
- Steps: load library via `https://www.gstatic.com/charts/loader.js`, prepare `DataTable`, set `options`, create chart object (e.g., `new google.visualization.LineChart(...)`) and `draw()`.
- Good for embedding interactive charts in web pages; limited customization vs D3.js.

5. Dashboard design principles
- Keep top-level summary visible, place most important KPI top-left, use filters for interactivity, minimize color palette (2–3 colors), use tooltips for details, avoid 3D charts for accuracy.

6. Quick exam answers (structure)
- Define the concept (1–2 lines), list challenges (3–4 bullets), mitigation techniques (3–4 bullets), and give a short example or tool mapping (Tableau/Google Charts) for 2–3 marks.
