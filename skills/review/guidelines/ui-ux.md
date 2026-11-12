# UI, UX and copy baseline

Fallback only: the project's own design system, copy guide and glossary always take precedence over
this file. For anything neither covers, use the [Carbon Design System](https://carbondesignsystem.com/)
and its patterns as the default reference.

Use these rules to judge user-facing strings, states and charts in a diff. Flag a deviation only
when the user would notice it.

## Voice and tone

- Clear and concise: lead with what matters most to the user, in the fewest words that stay precise.
- Personable and empathetic: write like a competent colleague, not a machine or a marketer.
- Confident without boasting; state facts and outcomes, not vague words like "something". No
  figurative language except for emphasis, no jokes in errors or warnings.
- Address the user as "you". Never speak for the user with "I" or "my", except where the user
  grants consent or a permission ("I agree to the terms").
- American English spelling throughout the source language.

## Capitalization and punctuation

- Sentence case for headings, titles, buttons, links, menu items, tabs, labels and list items:
  capitalize the first word and proper nouns only. Flag Title Case UI Strings.
- Capitalize a feature name only when it is a distinct, marketable product; generic features
  (dashboard, navigation, settings) stay lowercase.
- No capital letter after a slash ("and/or", "read/write").
- Periods: use them for full sentences, body text, descriptions and help text below a field.
  Omit them in headings, titles, buttons, labels, menu items, tooltips, toasts, notifications,
  placeholders, checkbox and radio labels, and sentence fragments.
- Question marks: only when the outcome is genuinely open ("Forgot password?"). Reword to an
  affirmative statement or a command where possible.
- Exclamation marks: avoid; never in errors or routine confirmations.
- Colons: introduce a bulleted list with one; do not put a colon after a checkbox or radio group
  label; avoid colons mid-sentence. Lowercase the word after a colon unless it is a proper noun.
- Use the serial (Oxford) comma in running text with three or more items. No commas at the end
  of list items.
- No ampersands in copy; write "and".
- Ellipsis: use the single character, not three periods. Do not use it to trail off a sentence or
  in placeholder text. When truncating, keep the part the user needs (often the start or the end,
  so truncate in the middle).
- Dashes: en dash without spaces for ranges (10–20); em dash only when splitting into two
  sentences would read worse. Hyphenate compound modifiers before a noun ("read-only field").
- Bold sparingly, never as a substitute for a heading. Numbered lists only for steps or priority.

## Headings and actionable language

- Headings tell the user what the section contains or what matters most there. One line, no
  trailing punctuation.
- Drop articles ("the", "a", "an") in labels, titles, buttons and other microcopy. Keep them in
  conversational text such as empty states and onboarding.
- Instructions start with an imperative verb ("Select a report to compare"). Do not use permissive
  phrasing like "You can ...".
- Buttons start with a strong verb and follow verb + noun ("Export report", "Add user"). A bare verb
  is fine only for universal actions (Done, Close, OK, Cancel) or when the surrounding title
  already names the object.
- A button or link label must predict exactly what happens. Flag labels that mislead or that do
  more or less than they say.
- Links: never "click here" or "here". Inside a sentence, link only the words that describe the
  destination, not the whole sentence (this also keeps translations intact). Standalone links
  follow verb + noun without punctuation.
- Menu actions follow verb + noun, or a verb alone if the menu context names the object. Menu
  nouns are short but specific enough to pick the right item.

### Confirmations

- Confirm only irreversible, destructive or costly actions; one task and two actions per dialog.
- Title: a short verb + noun question in sentence case, no articles ("Delete report?").
- Body: one line in plain language saying what will be lost and whether it can be undone. Never
  start with "Are you sure".
- Buttons may be single words because the title gives the context: "Delete" and "Cancel",
  "Discard" and "Keep editing", "Leave" and "Stay". The confirming button repeats the verb of the
  title; avoid generic "Yes"/"No".

## Vocabulary

Use one verb per meaning, everywhere. Flag a diff that uses a verb against its meaning below or
mixes synonyms for the same action.

| Verb | Means | Not to be confused with |
|------|-------|-------------------------|
| Create / New | Make an object from scratch | Add (uses an existing object) |
| Add | Put an existing object into a new context (user to group) | Create |
| Remove | Take out of the current context; the object still exists | Delete |
| Delete | Destroy the object permanently | Remove, Move to trash |
| Move to trash | Soft delete that can be restored | Delete |
| Edit | Allow changing values | Save |
| Save | Persist the changes to an object, whether new or existing. Never Update, Submit or Confirm | Apply, Done |
| Apply | Persist settings without closing the dialog | Save |
| Cancel | Stop the action and close without saving | Close, Discard |
| Close | Close a page, panel or window | Cancel |
| Done / Finish | Leave an editing context / complete the last step of a wizard | Save |
| Reset | Revert to the last saved values | Clear, Restore defaults |
| Clear | Empty fields, selections or content | Reset, Delete |
| Restore defaults | Return to factory settings | Reset |
| Refresh | Reload a view that is out of sync with its source | Reset |
| Import / Export | Bring data in from / write data out to an external format | Upload / Download |
| Upload / Download | Transfer a file to / from a remote system | Import / Export |
| Filter | Narrow a list to matching items | Search, Find |
| Search | Return all matching objects in a set | Filter |
| Run / Start | Initiate a procedure | Create |
| Learn more | Open further contextual information at the end of a hint | "Click here" |

Also use the project's domain terms exactly and consistently; one concept, one word.

## Empty states

- Every list, table, search result, chart or panel that can be empty needs a designed empty
  state; a blank area or a bare "No data" without context is a finding.
- The empty state explains what would appear here and, when useful, why it is empty (no items yet
  vs. no results for the current filter).
- If the user can resolve it, offer the action as a real control (button or link), not as text that
  reads like a call to action but is not interactive.
- Imagery stays neutral, never urgent or alarming. An empty chart hints at what it will show.

## Loading

- Every asynchronous view shows that it is loading; content must not flash blank or jump.
- First load of structured content (lists, cards, tables): a skeleton that resembles the final
  layout. Use the animated variant and match the corner style of the real components.
- Reload of an area that is already visible: keep the content and overlay it with a dimmed
  backdrop and a spinner, rather than replacing it with a skeleton.
- A spinner centered in a container is a first-load fallback where a skeleton does not fit.
- A button that starts processing shows its own loading state and blocks repeated clicks while the
  request runs. Only the button that triggered the action shows it.
- A spinner says nothing about duration; for long operations with known progress, show progress.

## Help and feedback

- Help must be easy to find: in the application bar, plus as the last navigation item before
  "Sign out" (always last). Use the standard help and feedback icons, no variants.
- Inline help tooltips sit behind a help-outline icon and give a brief description only.
- Help text: short, scannable (headings, lists, tables, spacing), plain language without jargon,
  only what typical use needs. Step-by-step instructions show the relevant control or an image.

## Data formats

- Format dates, times and numbers through the locale-aware formatter of the project, never by
  string concatenation or a hard-coded pattern. Never write an ambiguous numeric date such as
  03/12/22 in an exported or shared context.
- Show the year only when it differs from the current one, or in ranges and records where it
  matters.
- Relative time ("3 minutes ago", "yesterday", "tomorrow") for recent and near events; absolute
  date and time when the exact moment matters. Past events show date and time; for the distant
  past the date alone is enough.
- Ranges: en dash without spaces (8:00–10:30, Jan 6–Feb 2); write "from X to Y" and "between X
  and Y" in running text. Show a shared year or AM/PM only once, at the end. Open-ended ranges:
  "100 and up" or "100 or more".
- Durations use one format per context (H:MM:SS, or "2 h 32 min"); do not let "3:15" mean
  hours:minutes in one place and minutes:seconds in another.
- Time zones: name the zone wherever users in several zones see the same timestamp; no leading
  zero on single-digit offsets (UTC+5:00).
- Numbers: numerals in UI; spell out numbers below ten only in running prose where they are not
  data. Group thousands per locale. Do not truncate values without a reason and an indication.
- Units: a space between value and unit (25 kg, 850 °C), unit abbreviations never pluralized,
  decimals instead of fractions, no spaces around a slash in compound units (EUR/kg). Currency per
  locale.

## Layout gaps

Spacing expresses relationships. Use three gap levels, taken from the project's spacing tokens,
never ad hoc values:

| Level | Use when |
|-------|----------|
| Union (smallest) | The elements only make sense together, e.g. country code and phone number, postal code and city |
| Related (default) | Every other case: items within a group, a label and its field, buttons in a toolbar |
| Section (largest) | A new section or group starts and no other separator (line, border) marks it; also to set a destructive action apart from safe ones |

Flag gaps that contradict the relationship, e.g. unrelated groups at the default gap, or a
destructive action placed directly next to the primary one.

## Data visualization

- Accurate first: shapes must be proportional to the values. Bar and area charts start at a zero
  baseline; a truncated axis on a bar chart is a finding.
- Pick the chart by the question it answers:

  | Goal | Prefer | Avoid |
  |------|--------|-------|
  | Change over time, small variations, many series | Line chart | Pie charts |
  | Large variations, comparison, ranking (up to about 4 series) | Bar chart | Several pies side by side |
  | Part-to-whole over time | Stacked area or stacked bar | Overlapping areas with more than two series |

- Color has a purpose: categorical palettes for unrelated categories (applied in their fixed
  order), sequential palettes for magnitude, diverging palettes for deviation from a midpoint.
  Use neutral grays for context and a few highlight colors for the focus. Gradients do not replace
  a sequential palette.
- Status colors keep their meaning (red error or danger, orange serious warning, yellow warning,
  green normal); do not reuse them for categories.
- Never rely on color or icons alone: add labels, markers, line styles or patterns. Charts must
  stay readable in light and dark themes and for color-blind users.
- Same data category, same color across all charts of a view; vary line style, not color, to show
  a different period or a forecast of the same series.
- Text: horizontal labels only (rotate the bars, not the text); a balanced number of axis ticks;
  axis labels name the quantity and its unit; bold only one or two key elements.
- Label directly in simple charts; use a legend for dense ones, placed below the chart on desktop.
- Annotate outliers, thresholds and other noteworthy points; keep key data points on screen.
- Interactions: tooltips reveal details on hover (progressive disclosure); zoom and pan where
  users study detail, with panning restricted to the meaningful axis; filters, tabs or dropdowns
  as data controls. Motion only to connect states, never as decoration.
