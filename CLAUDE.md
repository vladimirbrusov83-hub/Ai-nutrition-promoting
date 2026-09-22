# CLAUDE.md — AI Nutrition Prompt Builder

## What this project is

A single `index.html` file. No build step, no dependencies, no framework. Open in browser and it works. Deployed via GitHub Pages or any static host.

The tool builds AI prompts for nutrition plans — users fill out a form, a prompt assembles live, they copy-paste it into ChatGPT/Claude/Gemini.

## File structure

```
index.html   — everything: HTML, CSS, JS in one file (~950 lines)
README.md    — project documentation
CLAUDE.md    — this file
```

## Design system

Light "academic" theme shared with the six knowledge hubs (Hypertrophy, Powerlifting, First Meet, Food Tracking, Health, Rucking). Keep it matching them: no dark backgrounds, no glows, no pill buttons.

```
--bg:    #fdfcfa   page background
--bg2:   #f6f4ef   header bar, section headers, footer
--border:#e2ded4   hairline rules and card borders
--text:  #15171b   primary text
--muted: #6a6f77   labels, secondary UI
--accent:#8c3a1c   sienna (same as Food Tracking): buttons, section numbers, focus
```

Fonts (Google Fonts): `Source Serif 4` for headings (sentence case), `DM Sans` for body and small tracked-caps labels. The prompt output box uses the system monospace.

Header: filled accent "Training hub" button linking to https://brusovcoach.org, then the page title.

## Mobile breakpoints

- `≤860px` — single column, prompt panel static, mobile "View your prompt" button visible
- `≤600px` — medical conditions grid → 1 column
- `≤480px` — field-row grids stack, allergy grid → 1 column, tighter padding
- All inputs must stay at `font-size: 1rem` minimum (iOS Safari zoom prevention)
- Touch targets: 44px minimum height on interactive elements at mobile sizes

## Key JS functions

| Function | Purpose |
|---|---|
| `getData()` | Reads all form fields, returns flat data object |
| `buildPrompt()` | Builds prompt string, updates DOM, calls `saveState()` |
| `copyPrompt()` | Clipboard copy with 2.5s "Copied!" feedback |
| `resetForm()` | confirm() → clear fields + localStorage → restore defaults |
| `saveState()` | Serializes all fields to localStorage key `nutrition-prompt-state` |
| `restoreState()` | Deserializes and restores all fields on page load |
| `setUnit(u)` | Switches imperial/metric, updates height fields and labels |

## Form sections

1. **Your Stats** — age, sex, weight, height, body fat, activity level, medical context
2. **Your Goal** — primary goal, calorie preference
3. **Diet & Preferences** — diet type, allergies (8 checkboxes), avoid/enjoy free text
4. **Meal Structure** — meals slider, timing, cook time, skill, budget, notes
5. **What to Generate** — 6 output checkboxes (macros, week plan, recipes, grocery, prep, swap)

## Prompt output rules

- Output numbering: always use `outputs.length + 1` **before** `.push()` — never hardcode "1."
- Medical context: only injected when at least one condition is selected; always includes the `⚠` adjustment line
- Culinary coherence rule is always in FORMAT REQUIREMENTS
- Macros format rule: only injected when `outMacros` or `outWeek` is selected
- Avoid-foods rule: only injected when the avoid field is non-empty
- Fallback: if all output checkboxes unchecked, PLEASE PROVIDE gets a generic full-plan request

## localStorage

Key: `nutrition-prompt-state`

Saved fields: all text inputs, all selects, all checkboxes (output + allergy + medical), meals slider, unit toggle. Saved on every `buildPrompt()` call. Cleared on `resetForm()`.

## What to avoid

- Do not add new external dependencies or CDN scripts
- Do not split into multiple files
- Do not add a build step
- Do not change the color palette or fonts
- Do not add comments explaining what code does — only add comments for non-obvious WHY
- Do not add features beyond what is requested
