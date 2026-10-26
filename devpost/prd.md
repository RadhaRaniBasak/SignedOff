---
status: approved
project: SignedOff
skill: 3-prd
---
# Product Requirements Document (PRD) — SignedOff

## 1. Product Overview
**SignedOff** is an interactive AI diff comprehension exam paper. Before a developer merges or signs off on an AI-generated code change, they load the diff, answer three structured comprehension questions pinned to specific line ranges, follow red-pen margin pointers to revise any partial or missed answers, and unlock an **"Understood"** rubber stamp and exportable sign-off note once all three answers are **Solid**.

## 2. Opening Screen & First Frame
When someone first opens **SignedOff**, they see a calm, empty "exam paper" screen with no sign-in or settings:
- **Top Bar**: Displays the project name (**SignedOff**), a live **`0 of 3 solid`** counter badge, and an empty dotted stamp slot labelled **`Understood — locked`**.
- **Split Main Area**:
  - **Left Pane (Diff Panel)**: Empty initially, featuring a centered **`Load sample diff`** primary button and a one-line caption describing the sample (*"Sample #01: AI agent hardening `/api/login` against brute-force and timing enumeration"*).
  - **Right Pane (Exam Questions)**: Shows the three question cards as dimmed placeholders (`Q1: What changed?`, `Q2: Why this design?`, `Q3: What about an edge case?`) with their target line badges so the entire structure is visible on the very first video frame.

## 3. Core User Journey & Interactive Behavior
1. **Loading the Bundled Sample Diff**:
   - Clicking **`Load sample diff`** populates the left pane with the 28-line numbered unified diff (`src/routes/login.ts`), pins each question card to its corresponding line range (`L5–L12`, `L14–L22`, `L7 & L23–L27`), and enables the answer textareas and grading buttons.
   - The large load button shrinks into a compact **`Reset`** link in the top bar so the presenter can reset state in one click for video retakes.
   - Each question card also includes quick **Demo Fill (`Partial` / `Solid`)** helper pills alongside free-text typing so a 1-to-3 minute demo recording can show the exact `Partial → Revise → Understood` transition smoothly without typing typos on camera.
2. **Grading & The `Partial → Revise → Solid` Loop**:
   - Submitting an answer evaluates which rubric concepts are present and displays a verdict chip:
     - **Red (`Missed`)**: 0 rubric points covered.
     - **Amber (`Partial`)**: 1 rubric point covered, missing the secondary detail.
     - **Green (`Solid`)**: All required rubric points for that question covered.
   - Whenever an answer is **`Partial`** or **`Missed`**, **SignedOff** pins a **red-pen margin annotation** directly under the exact diff line(s) in the left pane (and highlights those lines) with a teacher-style pointer (e.g., *"Red pen → Look at L17–L18: why do we still hash against `DUMMY_BCRYPT_HASH` when `user` is null?"*).
   - Clicking a question card or red-pen pointer scrolls and highlights the linked lines in the diff pane.
3. **Unlocking the `Understood` Rubber Stamp & Sign-Off Note**:
   - As each question reaches **`Solid`**, the top counter advances (`1 of 3 solid` → `2 of 3 solid` → `3 of 3 solid`).
   - At **`3 of 3 solid`**, the dotted slot in the top bar stamps a crimson/forest rubber stamp reading **`UNDERSTOOD • SIGNED OFF`** with a tactile scale-down animation, and reveals the **SignedOff Export Drawer** containing a formatted Markdown comprehension block (diff summary, Q&A takeaways, and timestamped sign-off line) with a **`Copy Sign-Off Note`** button.

## 4. Visual Direction ("Marked Exam Paper")
- **Surface Palette**: Warm archival off-white paper (`#f5f3ed`) with crisp cream sheets (`#fffdf8`), graphite ink (`#1d1b18`), and ruled ledger borders (`#dcd6c8`).
- **Typography**: Editorial serif headings (`Georgia` / `Newsreader` style) paired with crisp monospace code diffs and rubric stamps (`JetBrains Mono` / `ui-monospace`).
- **Semantic Accents**:
  - **Red Pen (`#b91c1c`)**: Margin pointers, line callout arrows, and `Missed` badges.
  - **Amber Ink (`#b45309`)**: `Partial` verdict chips and revision prompts.
  - **Forest Stamp (`#15803d`)**: `Solid` verdict chips and the unlocked `UNDERSTOOD` rubber stamp.
- **Responsive Layout**: Side-by-side split pane on desktop; stacks cleanly on mobile with the diff panel first.

## 5. Edge Cases & States
- **Empty Answer Submission**: Submitting a blank or whitespace-only answer does not crash; it shows a gentle inline prompt asking the learner to write at least a short explanation before grading.
- **Reset / Retake Action**: Clicking **`Reset`** in the top bar clears all answers, verdicts, red-pen margin notes, and the unlocked stamp, returning cleanly to the loaded diff (or back to the initial empty state via **Unload diff**).
- **Stretch Goal Boundary ("Paste Your Own Diff")**: Shown as a clearly labelled **"Later / Stretch: Inspect Custom Diff"** secondary modal/preview tab so reviewers see how the architecture extends while keeping the bundled sample as the rock-solid primary flow.
