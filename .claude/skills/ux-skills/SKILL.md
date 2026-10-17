---
name: ux-skills
description: Review and improve the user experience of interfaces — web pages, app screens, components, forms, flows, and UI copy. Use when the user asks for a UX review, usability or heuristic evaluation, accessibility check, feedback on a design or screenshot, help with UI copy/microcopy, or how to make a screen or flow easier to use.
---

# UX Skills

Help the user make interfaces easier to understand, faster to use, and accessible to everyone. Be concrete: point at specific elements, explain the problem from the user's point of view, and propose a fix.

## 1. Establish context first

Before judging anything, work out (from the request, code, or screenshot — ask only if it truly can't be inferred):

- **Who** the users are and how familiar they are with the product
- **What task** they're trying to complete on this screen or flow
- **Where** it's used: desktop, mobile, both; any constraints (brand, platform conventions)

## 2. Evaluate against these lenses

**Usability heuristics (Nielsen)**
1. Visibility of system status — loading, saving, success, and error states are shown
2. Match with the real world — the user's language, not internal jargon
3. User control and freedom — undo, cancel, back, easy exits
4. Consistency and standards — same thing looks and behaves the same everywhere
5. Error prevention — constraints, sensible defaults, confirmation for destructive actions
6. Recognition over recall — options visible, no memorizing between screens
7. Flexibility and efficiency — shortcuts for experts without burdening novices
8. Aesthetic and minimalist design — every element earns its place
9. Help users recover from errors — plain-language messages that say what happened and how to fix it
10. Help and documentation — available in context when needed

**Visual hierarchy and layout**
- One clear primary action per screen; secondary actions visibly secondary
- Scannable: headings, grouping, whitespace, alignment
- Related things are close together; unrelated things are separated

**Accessibility (WCAG 2.2 AA)**
- Text contrast ≥ 4.5:1 (≥ 3:1 for large text and UI components)
- Fully keyboard operable, with a visible focus indicator and logical tab order
- Every input has a label; images have meaningful alt text (or empty alt if decorative)
- Color is never the only way information is conveyed
- Touch targets at least 24×24 CSS px (44×44 preferred on mobile)
- Semantic HTML first; ARIA only when native elements can't do the job

**Forms**
- Ask only for what's needed; labels above fields, not placeholder-only
- Validate inline, after the user leaves a field, with specific messages
- Mark optional fields (or required ones — be consistent)

**Copy and microcopy**
- Buttons say what they do ("Save changes", not "Submit" or "OK")
- Short, plain, active voice; front-load the important words
- Empty states explain what goes here and how to start

## 3. Report findings

Group findings by severity and give each one a fix:

- **Critical** — blocks task completion or excludes users (e.g., inaccessible to keyboard users)
- **Major** — causes confusion, errors, or significant slowdown
- **Minor** — polish and consistency

For each finding: **what** (the specific element), **why** it hurts users (cite the heuristic or WCAG criterion), and **fix** (a concrete change — code, copy, or layout). Start with a one-line overall assessment and the top 3 changes that would help most. Note what already works well so it isn't changed by accident.

## 4. When implementing changes

If the user asks you to apply fixes in code, match the project's existing components, design tokens, and styling conventions rather than introducing new patterns. Verify accessibility fixes (contrast values, labels, focus order) rather than assuming them.
