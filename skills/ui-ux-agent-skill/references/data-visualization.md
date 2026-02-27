# Data Visualization

Practical data visualization rules for generating well-crafted UI code. Every chart, dashboard, and metric display exists to help someone make a decision faster.

---

## First Principle: Data-Ink Ratio

**Core idea:** Maximize the share of ink (pixels) used to present actual data. Minimize everything else. This is Edward Tufte's foundational rule.

- Every pixel on screen should serve the data. If it doesn't encode information, it's a candidate for removal.
- Chart junk — 3D effects, gradient fills, decorative gridlines, drop shadows on bars — actively harms comprehension. It does not make charts "look better." It makes them harder to read.

**Remove these by default:**

- Background colors behind chart areas
- Borders and boxes around charts
- Gridlines beyond the minimum needed for reference (light horizontal lines only, if any)
- Legends that repeat what axis labels already say
- Redundant data labels (if the axis tells you the value, don't also label every bar)
- Decorative icons or illustrations inside the chart area

**Review question:** If you deleted an element, would the chart lose any information? If no, delete it.

---

## Chart Type Selection

**Core idea:** The chart type is determined by the relationship you're showing, not by what looks interesting.

This is the most common mistake in data visualization — picking a chart type before understanding the question.

| Relationship | Chart type | Notes |
|---|---|---|
| Comparison (this vs that) | Bar chart | Horizontal for many items or long labels, vertical for few |
| Trend over time | Line chart | Area chart only for cumulative/stacked values |
| Part of a whole | Stacked bar or treemap | Pie chart only for 2-3 segments |
| Distribution | Histogram or box plot | Histogram for single variable, box plot for comparing distributions |
| Correlation | Scatter plot | Add a trend line if the correlation matters |
| Composition over time | Stacked area or stacked bar | Stacked area for continuous, stacked bar for discrete periods |
| Single KPI | Big number + trend indicator | Sparkline or arrow + percentage change |
| Geographic | Choropleth or bubble map | Choropleth for density/rates, bubble for discrete values at locations |

**Hard rules:**

- Never use a pie chart with more than 3-4 slices. Human brains cannot accurately compare slice angles beyond that. A bar chart always works better.
- Never use 3D charts. Ever. They distort spatial perception and make values unreadable.
- Never use dual y-axes. They mislead by implying correlation between unrelated scales. Use two separate charts instead.
- Prefer horizontal bar charts when category labels are longer than 3-4 words.
- Default to line charts for time series. Bar charts for time series only when showing discrete, non-continuous periods.

**Review question:** Can you state in one sentence what comparison or relationship this chart reveals? If not, the chart type is wrong.

---

## Dashboard Design

**Core idea:** A dashboard answers a question. If you can't name the question, you don't have a dashboard — you have a data dump.

- Start every dashboard with: **"What decision should the user make here?"** Design backward from that.
- Top-level KPIs: **3-5 maximum.** More than that and nothing stands out. If everything is important, nothing is.
- Time range and filters must be **visible and persistent** — not hidden behind a menu or hamburger icon.
- Every metric needs **context**: current value + at least one comparison (previous period, target, benchmark). A number without context is meaningless.
- Provide **drill-down paths** — clicking any metric should reveal its breakdown. Surface-level numbers invite surface-level decisions.

**Layout pattern:**

| Position | Content |
|---|---|
| Top row | KPI cards (big number + trend + comparison) |
| Middle | Charts answering the dashboard's key questions |
| Bottom | Detail tables or secondary breakdowns |

**Rules:**

- Don't show data for the sake of showing data. Every chart must answer a specific sub-question that supports the dashboard's main question.
- Group related metrics visually. Use whitespace and subtle section dividers, not heavy borders.
- Real-time dashboards need visible "last updated" timestamps. Stale data presented as current is worse than no data.
- Loading states matter — skeleton screens for charts, not spinners that block the whole page.

**Review question:** Can a new user answer the dashboard's key question within 5 seconds of looking at it?

---

## Axes and Labels

**Core idea:** Axes are the coordinate system for understanding the data. Get them wrong and every reading is wrong.

- **Always label axes.** Include units. "Revenue" is incomplete. "Revenue (USD, thousands)" is useful.
- **Start y-axis at zero for bar charts.** Truncated axes exaggerate differences and mislead. A bar that's twice as tall should represent twice the value.
- **Line charts can start at non-zero** if the meaningful range is narrow (e.g., stock prices between $98-$102). The shape of the trend matters more than the absolute position.
- **Use human-readable numbers:** "1.2M" not "1,200,000". "45K" not "45,000". The chart is for comprehension, not precision — tooltips handle exact values.

**Formatting rules:**

| Element | Rule |
|---|---|
| Large numbers | Abbreviate: K, M, B |
| Dates | Consistent, unambiguous format: "Jan 15" not "1/15" (locale-dependent) |
| Label orientation | Horizontal text only. Rotate labels only as a last resort — if you need to rotate, you probably need a horizontal bar chart instead |
| Tick marks | Reduce to the minimum needed for orientation. 4-6 ticks on an axis is usually enough |

**Review question:** Can someone unfamiliar with the dataset read the axis labels and understand what's being measured, including units and scale?

---

## Color in Visualization

**Core idea:** Color encodes meaning. Use it intentionally or it becomes noise.

| Data type | Color strategy | Example |
|---|---|---|
| Sequential (low to high) | Single hue, light to dark | Light blue to dark blue for intensity |
| Diverging (above/below a midpoint) | Two hues with neutral center | Blue — white — red for above/below target |
| Categorical (distinct groups) | Distinct hues | Max 6-8 before they blur together |

**Rules:**

- **Highlight what matters, gray out the rest.** If one series is the story, make it the only colored element. Everything else gets `#D1D5DB` or similar neutral gray.
- **Always test with color blindness simulation.** ~8% of men have some form of color vision deficiency. Never use red-green as the only differentiator.
- **Add redundant encoding** alongside color: patterns, labels, direct annotations, or different line styles (solid, dashed, dotted).
- **Consistent meaning across a dashboard.** If blue means "actual" in one chart, it means "actual" in every chart. Don't reuse colors for different meanings.
- **Avoid rainbow palettes.** They have no perceptual ordering (is green "more" than yellow?) and are indistinguishable in grayscale.
- **Semantic colors:** red for negative/danger, green for positive/success, amber for warning — but only when the data has that valence. Don't color revenue red just because it's a warm color.

**Review question:** Print the chart in grayscale. Can you still distinguish all the data series?

---

## Interaction

**Core idea:** Interaction reveals detail on demand without cluttering the default view.

- **Tooltips on hover:** Show exact values, formatted with units. But never rely on hover for essential information — mobile devices have no hover state.
- **Click to filter/drill down:** Clicking a bar, segment, or legend item should filter or navigate to more detail.
- **Hover highlight with context:** Don't just highlight the bar — show the specific data point value, comparison to average, or percentage of total.
- **Crosshair or reference line** for time series: helps users track values across multiple series at the same point in time.
- **Time range controls:** Allow users to zoom into specific periods. Preset ranges (7d, 30d, 90d, 1y) plus custom date picker.
- **Transitions, not jumps:** When data updates or filters change, animate smoothly. Sudden jumps break the user's spatial map of the data.

**Mobile considerations:**

- Replace hover interactions with tap-to-reveal tooltips.
- Make touch targets large enough (minimum 44px).
- Consider swipe gestures for time range navigation.
- Simplify dashboards for mobile — fewer charts, bigger text, vertical scroll.

**Review question:** Does every interactive element work on both mouse and touch? Is essential information visible without any interaction?

---

## Numbers and Formatting

**Core idea:** Inconsistent number formatting creates cognitive friction. The user should never have to mentally convert between formats.

| Type | Formatting rule |
|---|---|
| Currency | 2 decimal places, currency symbol, abbreviated for large values ($1.2M) |
| Percentages | 0-1 decimal places. 73%, not 73.2847% |
| Counts | Whole numbers. Abbreviate above 10K |
| Rates/ratios | 1-2 decimal places |
| Dates | ISO 8601 for data, human-readable for display |

**Rules:**

- **Consistent formatting across all charts.** Don't show "1,234" in one chart and "1234" in another.
- **Trend indicators are more useful than numbers alone.** Arrow up/down + percentage change communicates direction and magnitude instantly.
- **Sparklines for inline trends:** Small, no axes, just the shape of the trend. Useful in tables and KPI cards.
- **Significant digits matter:** Showing $1,234,567.89 in a chart comparing billions is false precision. Show $1.2B.
- **Negative numbers:** Use minus sign and red color together, never color alone.

**Review question:** Are all numbers in the dashboard formatted the same way for the same type of data?

---

## Common Anti-Patterns

Things that signal amateur data visualization:

- **Wall of charts with no narrative** — "dashboard as data dump." Every chart should support a story.
- **Pie charts with 8+ slices** — unreadable. Use a horizontal bar chart.
- **3D charts of any kind** — distorted, unreadable, never justified.
- **Dual y-axes** — implies false correlation. Use two separate charts.
- **Truncated y-axis on bar charts** — exaggerates small differences into dramatic visual gaps.
- **Rainbow color palettes** — no perceptual ordering, indistinguishable in grayscale.
- **Charts without axis labels** — the viewer is guessing what they're looking at.
- **Auto-refreshing charts that jump/shift layout** — use smooth transitions or update indicators.
- **Showing raw data without aggregation or context** — 10,000 data points on a scatter plot is not a visualization, it's a mess. Aggregate, sample, or add density encoding.
- **Decorative chart borders, backgrounds, and shadows** — pure noise.
- **Using area charts for non-cumulative single series** — the filled area implies volume/quantity that isn't there.

---

## Testing Checklist

Run through these before shipping any data visualization:

- [ ] Can the user answer the dashboard's key question within 5 seconds?
- [ ] Does each chart have a clear title that states what it shows (not how it shows it)?
- [ ] Are axes labeled with units?
- [ ] Is the y-axis starting at zero for all bar charts?
- [ ] Is the color palette distinguishable in grayscale?
- [ ] Is the color palette accessible for color-blind users (no red-green only)?
- [ ] Do tooltips/interactions work on both mouse and touch?
- [ ] Is there a drill-down path for every key metric?
- [ ] Are numbers formatted consistently across all charts?
- [ ] Are large numbers abbreviated (K, M, B)?
- [ ] Does every chart answer a specific, statable question?
- [ ] Is chart junk eliminated (no 3D, no gradients, no decorative gridlines)?
- [ ] Do time-based charts have clear, unambiguous date formatting?
- [ ] Are loading and empty states handled (skeleton screens, "no data" messages)?
