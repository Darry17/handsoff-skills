---
name: ux-auditor
description: >
  DESIGNER AUTHORITY SKILL — Validates UX quality of the generated website against
  the original design. Designers have FINAL AUTHORITY on UX decisions. This skill 
  checks usability, user flows, interaction patterns, visual hierarchy, and overall
  user experience quality. Use after fidelity-checker but BEFORE delivery.
version: 0.1.0
agent: designer
reads:
  - generated-code-files
  - raw-design-tree
  - tokens.md
  - user-requirements
writes:
  - ux-issues
depends_on:
  - figma-reader
---

# ux-auditor

## Purpose
Grants designers FINAL AUTHORITY to validate and approve the UX quality of the generated website. This skill ensures the implemented website meets the designer's quality standards for user experience.

## DESIGNER AUTHORITY — MANDATORY

**Designers have FULL AUTHORITY over UX decisions.**
- Designer decisions CANNOT be overridden by developers or QA
- Designer rejection means the work must be redone
- Designer approval is the final gate before delivery

## When this skill runs
After developer code generation completes, typically after fidelity-checker runs.

## Inputs
Generated code, raw-design-tree, tokens.md, and user requirements.

## Process

### 1. Visual Hierarchy Validation
1. Verify visual hierarchy matches design intent
2. Check element prominence and flow
3. Verify information architecture

### 2. User Flow Validation
1. Trace critical user journeys
2. Verify navigation patterns
3. Check user task completion flows

### 3. Interaction Pattern Validation
1. Verify hover states, active states, focus states
2. Check interactive element affordances
3. Verify transitions and animations quality

### 4. Usability Validation
1. Verify content is readable and scannable
2. Check interactive element discoverability
3. Verify error states and feedback

### 5. Design Intent Validation
1. Verify design system is applied consistently
2. Check brand visual identity
3. Verify design polish level

### 6. Accessibility & UX Intersection
1. Verify keyboard navigation works logically
2. Check focus management
3. Verify screen reader flow

## Outputs
ux-issues listed in shared context, with designer AUTHORITY to:
- APPROVE the implementation
- REQUEST CHANGES (must be addressed)
- REJECT the implementation (blocks delivery)

## Designer Authority Rules

### Final Decision Authority
- Designer APROVAL is the final gate before delivery
- Designer REJECTION requires rework
- NO bypass of designer authority

### Designer Can Reject For
- Visual hierarchy not matching design intent
- User flows being broken or confusing
- Missing or incorrect interaction patterns
- Missing hover/active/focus states
- Animations feeling wrong or unnatural
- Content hierarchy being unclear
- Usability issues affecting task completion
- Design polish not meeting standards
- Any UX issue the designer identifies

### Issue Severity
- **BLOCKING** — Must fix before delivery
- **REQUESTED CHANGE** — Should fix for quality
- **RECOMMENDATION** — Consider fixing

## Blocking Criteria
MUST block delivery if designer identifies:
- Broken user flows
- Missing critical interactions
- Usability issues blocking tasks
- Visual hierarchy violations
- Any issue designer marks as BLOCKING

## References
None.