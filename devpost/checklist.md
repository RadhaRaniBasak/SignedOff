---
status: approved
project: SignedOff
skill: 5-build
mode: fast
---
# Build Checklist — SignedOff

## Build Mode
- **Selected Mode**: `fast` (verified vertical slices, automated syntax & DOM check via `check_web_app.py`, hands-on review of the 1-take video path, and architectural code tour).

## Vertical Build Slices
- [x] **Slice 1 — Marked Exam Paper Shell & Opening Frame (`index.html`)**
  - Top bar with **SignedOff** title, live `0 of 3 solid` counter badge, and locked dotted rubber-stamp slot (`Understood — locked`).
  - Split two-column layout: empty diff panel on the left with centered **Load sample diff** primary button, and three dimmed comprehension cards on the right (`Q1: What changed?`, `Q2: Why this design?`, `Q3: What about an edge case?`).
- [x] **Slice 2 — Bundled Sample Diff Loader & Retake Controls**
  - Clicking **Load sample diff** renders the 28-line numbered unified diff (`src/routes/login.ts`), pins line badges (`L7–20`, `L15–20`, `L7–8 & 25`), activates the answer textareas, and transforms the load button into a top-bar **Reset answers** link and **🎬 Load 1-Take Video Preset** button.
- [x] **Slice 3 — Rubric Point Matcher & Deterministic Verdict Derivation**
  - `evaluateRubricCoverage(questionId, rawAnswer)` reports only covered rubric point IDs (`q1_ratelimit`, `q1_dummy_hash`, `q2_timing`, `q2_enumeration`, `q3_xff_spoof`, `q3_shared_ip`).
  - `deriveQuestionState(question, coveredPointIds)` deterministically computes `solid`, `partial`, or `missed` verdicts and injects red-pen margin callouts directly beneath the exact diff lines for any missing rubric points (`Line 8`, `Line 17`, `Line 18`, `Line 20`, `Line 7`).
- [x] **Slice 4 — Rubber-Stamp Gate Unlock & SignedOff Markdown Export**
  - Reaching `3 of 3 solid` animates the `✓ Understood` rubber stamp into the top bar and opens the **Comprehension Verified** banner with a formatted Markdown sign-off block and one-click clipboard copy.

## Hands-On Checkpoints & Final Review
- [x] **Early Checkpoint (Slices 1–2)**: Verified that the opening frame communicates the entire product before any click, and that loading/resetting the bundled diff transitions smoothly without page reloads.
- [x] **Final Review (Slices 3–4)**: Verified the signature `Partial → Red-Pen Line Pointer → Revise → Understood` video loop on Question 1 (`L17` pointer) and tested clipboard export and static verification (`check_web_app.py` passed with `0 errors, 0 warnings`).

## Code Tour and App Map
- [x] **Trace One Action End-to-End (`index.html`)**:
  1. **User Action (`click` on `Check Answer` — `index.html:L842`)**: Event delegation catches `[data-grade-question]` and invokes `gradeQuestion(questionId)`.
  2. **Rubric Coverage Matcher (`evaluateRubricCoverage` — `index.html:L562`)**: Scans the answer against the question's rubric points and returns *only* the covered rubric IDs (`coveredPointIds`).
  3. **Deterministic Derivation & Red-Pen Rendering (`deriveQuestionState` — `index.html:L579` & `renderDiffPane` — `index.html:L671`)**: Derives `solid` / `partial` / `missed`, attaches red-pen margin callouts to exact diff line numbers for any missing point, and unlocks the `✓ Understood` stamp when `solidCount === 3`.
- [x] **Reusable Takeaway**: Separate fuzzy evaluation ("which rubric concepts did the user mention?") from deterministic state derivation ("is this Solid/Partial/Missed, which line number gets the red-pen pointer, and does the stamp unlock?"). That boundary makes AI-assisted apps predictable and testable.
- [x] **Generated Companion**: `devpost/app-map.html`
