# Forms Checklist

Building forms? Check these before shipping.

---

1. **Every field has a visible `<label>`.** Not just a placeholder — placeholders disappear on focus and fail accessibility. Labels are permanent. (Ref: `forms.md`, `accessibility.md`)

2. **Required fields are marked.** Asterisk or "(required)" text, plus `required` or `aria-required` in the markup. If most fields are required, mark the optional ones instead. (Ref: `forms.md`)

3. **Field types match the data.** Date picker for dates, `type="email"` for email, `type="tel"` for phone, number input for numbers. Native field types give you free validation and mobile keyboard optimization. (Principle F: Error Prevention)

4. **Format hints shown before the user types.** "MM/DD/YYYY" below the date field. "Must be 8+ characters" below the password field. Prevent errors, don't punish them. (Principle F: Error Prevention)

5. **Inline validation on blur.** Validate when the user leaves the field — not on every keystroke (annoying) and not only on submit (too late). Error messages appear adjacent to the field. (Ref: `forms.md`)

6. **Error messages are specific and actionable.** "Email must include @" not "Invalid input." "Password must be at least 8 characters" not "Too short." (Principle C: Feedback)

7. **Error summary for long forms.** Forms with 5+ fields: show an error summary at the top that links to each errored field. The summary appears on submit. (Ref: `forms.md`)

8. **Sensible defaults pre-filled.** Detect timezone, country, currency where possible. Pre-select the most common option. Reduce decisions. (Principle G: Cognitive Load)

9. **Group related fields.** Use fieldsets, section headings, or visual dividers. "Personal info" / "Billing address" / "Payment." Don't present 15 fields as a flat list. (Principle B: Information Architecture)

10. **Submit button labeled with the action.** "Create project" not "Submit." "Save changes" not "OK." Disable until required fields are valid. Show loading state on click. (See: `checklists/buttons.md`)

11. **Multi-step forms: progress indicator + back navigation.** Show which step the user is on and how many remain. Back button preserves entered data. Draft auto-save for long forms. (Ref: `forms.md`)

12. **Success confirmation: what was saved + what's next.** "Project created. You can now invite team members." Not just a green checkmark. (Principle C: Feedback)

13. **Tab order matches visual order.** No `tabindex` hacks. Fields flow naturally left-to-right, top-to-bottom. Test by tabbing through the entire form. (Ref: `accessibility.md`)

14. **Autofill works.** Correct `name` and `autocomplete` attributes on all fields. Users expect browsers to fill their name, email, and address. Don't break it. (Ref: `forms.md`)
