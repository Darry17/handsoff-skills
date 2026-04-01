---
file: output-contract.md
version: 0.1.0
purpose: Defines what QA and SEO expect from developer output
---

# Output contract

The developer must satisfy this contract before QA runs.
QA will reject any output that violates these rules.

## Required per component file
- [ ] File named exactly as specified in the component tree
- [ ] Component name matches component tree exactly (case-sensitive)
- [ ] All color, spacing, and typography values sourced from tokens.md
- [ ] Mobile-first responsive styles applied
- [ ] All interactive elements have accessible labels
- [ ] Images have alt attributes (empty string only if decorative)
- [ ] Heading hierarchy is correct within the component

## Required per page/section file
- [ ] Exactly one h1 per page
- [ ] Landmark regions present: main, nav, header, footer
- [ ] Skip-to-content link present
- [ ] Page title set

## Prohibited patterns
- [ ] No raw hex values not defined in tokens.md
- [ ] No inline styles overriding token values
- [ ] No empty href attributes
- [ ] No missing form labels
- [ ] No positive tabIndex values
- [ ] No deprecated HTML elements (center, font, marquee)
