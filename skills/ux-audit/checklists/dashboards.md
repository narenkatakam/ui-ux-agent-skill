# Dashboards Checklist

Building dashboards? Check these before shipping.

---

1. **Lead with the most important metric.** The number the user checks first goes top-left, largest. Everything else is supporting context. Ask: "If the user had 5 seconds, what's the one thing they need to see?" (Principle A: Task-First UX)

2. **Group related metrics.** Revenue metrics together, usage metrics together, health metrics together. Group by user mental model, not by data source. (Principle B: Information Architecture)

3. **Time range selector: prominent and consistent.** One global time selector that affects all widgets, placed top-right. Per-widget overrides only if there's a genuine use case. Show the active range clearly. (Principle D: Consistency)

4. **Charts follow data-viz rules.** Label axes. Start y-axis at zero for bar charts. Limit to 5-7 colors max. No 3D charts. No pie charts for more than 5 slices. (Ref: `data-visualization.md`)

5. **Every chart is legible without interaction.** If a chart requires hover to understand what the data means, it's missing labels. Hover adds detail — it shouldn't be required for comprehension. (Principle E: Affordance)

6. **Loading: skeleton preserving layout.** Dashboard loads in sections. Each widget shows a skeleton that matches its final shape. Not a full-page spinner that blocks everything. (Ref: `ui-states.md`)

7. **Empty/zero-data state for new users.** A new account with no data shouldn't see a wall of flat-line charts. Show guidance: "Connect your data source to see analytics here." (Ref: `ui-states.md`)

8. **Filters: visible active state + clear-all.** If the dashboard supports filtering, active filters show as chips or inline badges. A "clear all filters" option is always accessible. (Principle C: Feedback)

9. **Responsive: key metrics stay above the fold.** On mobile, stack widgets vertically. Top metrics (KPI cards) remain visible without scrolling. Charts can be simplified or collapsed on small screens. (Ref: `responsive-design.md`)

10. **Last updated indicator + manual refresh.** Show when data was last refreshed. Provide a refresh button for on-demand updates. If data is real-time, indicate it. Users need to trust the numbers. (Principle C: Feedback)

11. **Export and drill-down.** Users will want to investigate or share what they see. Provide CSV/PDF export for data views and click-through from summary metrics to detail tables. (Principle A: Task-First UX)

12. **Contrast and color restraint.** One accent color for the most important metric. Supporting charts use muted palette. Don't make every widget a different color — it creates visual chaos. (Principle L: Color Systems, Ref: `data-visualization.md`)

13. **Accessibility: data tables as alternative.** For users who can't interpret charts, provide a tabular data view or ensure chart data is available to screen readers via `aria-label` or data tables. (Ref: `accessibility.md`)
