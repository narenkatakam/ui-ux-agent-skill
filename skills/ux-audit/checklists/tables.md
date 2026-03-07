# Tables Checklist

Building tables? Check these before shipping.

---

1. **Column alignment: text left, numbers right, status center.** Numbers right-aligned so digits line up. Text left-aligned for scanning. Status badges/icons center or left. (Principle H: CRAP — Alignment)

2. **Sort by most useful column by default.** Show the current sort column and direction (arrow). Every sortable column header should be clickable. (Principle A: Task-First UX)

3. **Fixed header on scroll.** When the table body scrolls, column headers must stay visible. Without this, users lose context. (Principle G: Cognitive Load)

4. **Row hover highlight.** Subtle background change on hover helps users track across wide tables. Not decorative — functional. (Principle E: Affordance)

5. **Row selection: checkbox column.** If the table supports actions on items: checkbox in the first column, select-all in the header, shift-click for range selection. Bulk action bar appears on selection. (Ref: `ui-states.md`)

6. **Pagination or virtual scrolling for 50+ rows.** Show row count. Page size selector if applicable. Never dump hundreds of rows into the DOM. (Principle G: Cognitive Load)

7. **Search and filter for 15+ items.** Filters show active state as chips or inline labels. "Clear all" option visible. Filter results empty state: "No results match your filters" + clear action. (Principle B: Information Architecture)

8. **Cell truncation with full text on hover.** Long cell content gets ellipsis. Hover reveals full text in a tooltip. Never wrap cells to variable heights — it breaks the grid. (Principle H: CRAP — Repetition)

9. **Empty state.** Table with zero rows shows a centered message with guidance: "No transactions yet" with a link to the relevant action. Not a blank area with headers. (Ref: `ui-states.md`)

10. **Loading state: skeleton rows.** Show 3-5 skeleton rows that preserve the column layout. Not a spinner that replaces the entire table. (Ref: `ui-states.md`)

11. **Responsive strategy.** Wide tables on mobile need a plan: horizontal scroll with a sticky first column, or collapse columns into a card layout. Key columns always visible. (Ref: `responsive-design.md`)

12. **Row actions: consistent placement.** Actions (edit, delete, menu) in the last column, same position in every row. Destructive actions require confirmation. (Principles D, F: Consistency, Error Prevention)

13. **Accessibility: use `<table>` semantics.** `<thead>`, `<tbody>`, `<th scope="col">` for column headers. `aria-sort` on sortable columns. Keyboard-navigable rows if interactive. (Ref: `accessibility.md`)
