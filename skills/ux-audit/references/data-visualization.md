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

## Data Tables

**Core idea:** Tables are the most common data UI in enterprise/admin interfaces. A well-designed table is faster to scan than any chart. Most of the data work in real products lives in tables, not dashboards — treat them as first-class UI.

### Layout Rules

- **Left-align text, right-align numbers.** Numbers right-aligned so decimal points and digit places line up vertically. This is the single most impactful table readability rule.
- **Column headers:** Same alignment as their data column. Semibold weight. Smaller font size or uppercase letter-spacing for visual separation from row data.
- **Row height:** 48-56px for comfortable scanning. 40px for dense/admin views where users are power users and screen real estate matters. Below 40px, rows blur together.
- **Horizontal lines between rows** — subtle, `gray-200` weight. No vertical lines. No zebra striping. Use hover highlight (`gray-50` or `gray-100` background) instead — it's less noisy and gives the user an active reading aid.
- **Cell padding:** 12-16px horizontal. Consistent across all columns. Don't let content touch cell edges.
- **Column width:** Size to content, not evenly distributed. Name columns get more space than status columns. Allow text truncation with tooltips for overflow — never wrap text in data cells.

### Sorting and Filtering

- **Every column with meaningful data should be sortable.** If a user might want to rank by it, let them.
- **Current sort state must be visible:** Arrow icon (up/down) in the active header + a differentiated header style (bold, color, or background). Users need to know what's sorted and in which direction without guessing.
- **Filters: show as chips above the table**, not hidden in a dropdown or sidebar. Active filters must be visible at all times — hidden filters are the #1 cause of "where did my data go?" support tickets.
- **Search:** Full-text search input above the table for tables with 20+ rows. Debounce at 300ms. Highlight matching text in results if feasible.
- **Default sort must be intentional.** Most recent first for time-based data. Alphabetical for name lists. Most important metric descending for performance tables. Never random.

### Selection and Bulk Actions

- **Checkbox column on the left** for multi-select. First column, always. Fixed width (40-48px).
- **Selected count + bulk action bar** appears at the top of the table when 1+ items are selected. Pattern: "3 selected — [Export] [Delete] [Assign]". This bar replaces or overlays the header row.
- **Select all:** Applies to the current filtered/visible view, not all data across all pages. If users need to select all 10,000 records, provide an explicit "Select all 10,000" link after they select the visible page.
- **Deselect all** must be one click. A clear "×" or "Deselect all" in the bulk action bar.
- **Shift+click for range selection.** Desktop users expect this. Don't skip it.

### Pagination vs Infinite Scroll

- **Pagination for data > 50 rows.** Predictable, bookmarkable, and users can orient themselves ("page 3 of 12").
- **Show:** Total count, current page, rows-per-page control (10 / 25 / 50 / 100).
- **Infinite scroll only for feeds/timelines** — chronological, consumption-oriented content. Never for actionable data tables where users need to find, compare, or act on specific rows.
- **URL state:** Current page, sort, and filters should be reflected in the URL. Users share links to specific table views. "I'm looking at page 5 sorted by revenue" should be a shareable URL, not session state.

### Row Actions

- **Inline actions on hover** (edit, delete, view) — max 2-3 visible icons. Right-aligned in a dedicated actions column.
- **Overflow menu (three-dot icon)** for 4+ actions. Opens a dropdown with clear labels, not just icons.
- **Never put critical actions behind a hover state on mobile.** Mobile has no hover. Use a row-click to navigate to a detail page, or show action icons persistently.
- **Destructive actions (delete, archive) need confirmation.** Inline confirmation ("Are you sure?") or undo toast — not a modal dialog for every delete.
- **Row click behavior must be clear.** Either the whole row is clickable (navigates to detail), or only specific elements are clickable. Not both. If the row is clickable, add a subtle hover cursor and visual feedback.

### Responsive Tables

- **Tables > 4 columns on mobile:** Horizontal scroll with the first column (usually name/identifier) sticky. The user always knows which row they're looking at while scrolling right.
- **Alternative: card layout on mobile.** Each row becomes a stacked card with label-value pairs. Better for tables where every column matters equally.
- **Never just let the table overflow and hope users scroll.** They won't discover the hidden columns. Either scroll with a sticky column, reflow to cards, or hide non-essential columns with a "Columns" toggle.
- **Priority columns:** On narrow viewports, show the 2-3 most important columns. Let users toggle others. This requires knowing which columns matter — make it configurable or make a product decision.

### Empty and Loading States

- **Empty state:** Centered message + illustration or icon + CTA. "No results found. Try adjusting your filters." or "No projects yet. Create your first project." Never a blank white rectangle where rows should be.
- **Loading: Skeleton rows matching the table structure.** Match column widths, row heights, and alignment. Not a spinner replacing the whole table — that destroys spatial context and flashes the layout.
- **Partial loading:** If data loads in chunks (pagination), show the skeleton for the next page while keeping current data visible. Don't clear the screen to show a loader.
- **Error state:** "Failed to load data. [Retry]" in the table body area. Preserve any filters/sort state so retry doesn't reset the user's context.

### Table Checklist

- [ ] Text is left-aligned, numbers are right-aligned in every column.
- [ ] Column headers match the alignment of their data and are visually distinct (semibold, smaller size, or uppercase).
- [ ] Row height is 48-56px (standard) or 40px (dense) — not cramped, not wasteful.
- [ ] Horizontal row dividers are subtle (`gray-200`). No vertical lines, no zebra stripes.
- [ ] Sort state is visible: active column header is differentiated and shows direction arrow.
- [ ] Active filters are shown as chips above the table, not hidden in menus.
- [ ] Multi-select uses a checkbox column, and bulk actions appear in a top bar with selected count.
- [ ] Pagination shows total count, current page, and rows-per-page control for tables > 50 rows.
- [ ] Row actions are discoverable: 2-3 inline on hover, overflow menu for more, no hover-dependent actions on mobile.
- [ ] Responsive behavior is handled: sticky first column with horizontal scroll or card layout on mobile.
- [ ] Empty state has a message + CTA — never a blank table body.
- [ ] Loading state uses skeleton rows matching table structure — never a full-table spinner.

**Review question:** Can a user find a specific row, understand its data, and take action on it within 5 seconds?

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

---

## Dashboard Checklist

Run through these when reviewing or building dashboards specifically:

- [ ] Primary metric / KPI is the most visually prominent element on the page.
- [ ] Metrics show context: current value + trend (up/down/flat) + comparison period.
- [ ] Time range selector is visible and defaults to the most useful period.
- [ ] Data density is appropriate: dashboards should be scannable, not require close reading.
- [ ] Empty data states are handled: "No data for this period" not a blank chart.
- [ ] Loading states for each widget independently — don't hold the entire dashboard for one slow query.
- [ ] Filters apply globally and their state is visible. Active filters shown as chips or in a filter bar.
- [ ] Drill-down is available: clicking a metric or chart segment leads to detail data.
- [ ] Responsive: widgets reflow from multi-column to single-column. Charts remain legible at narrow widths.
- [ ] Color is used consistently: green = positive, red = negative, gray = neutral. Never reversed.
- [ ] Data refreshes are indicated: last-updated timestamp, auto-refresh indicator, or manual refresh button.

**Review question:** Glance at this dashboard for 3 seconds. Can you answer the most important question it's meant to answer?
