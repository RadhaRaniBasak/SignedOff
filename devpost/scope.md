---
status: approved
project: SignedOff
skill: 2-scope
---
# Scope — SignedOff (AI Diff Comprehension Check)

## Why It Matters
AI agents make it easy to ship code you can't explain. SignedOff turns "it works" into "I actually understand it" by requiring you to explain the change, the design choice, and the edge case before unlocking a sign-off stamp.

## Who It's For
Developers, students, and builders using AI coding agents (Claude Code, Cursor, Codex, Copilot) who want a fast comprehension check on high-impact diffs before merging or demoing them.

## The Kernel
A focused comprehension check over a single bundled AI-generated diff (an agent hardening a `/api/login` route) with three targeted questions, a hidden multi-point rubric, red-pen line pointers that guide revision when an answer is partial or missed, and a locked **"Understood"** rubber stamp that unlocks only when all three answers reach **Solid**.

## The Core Loop
1. **Open the Exam Paper**: The screen opens on a calm, split-pane "marked exam paper" layout with a `0 of 3 solid` counter, a locked `Understood — locked` dotted stamp slot, an empty diff pane with a centered **Load sample diff** button, and three dimmed question cards (`What changed?`, `Why this design?`, `What about an edge case?`).
2. **Load Bundled Sample Diff**: Clicking **Load sample diff** populates the left pane with a 28-line numbered diff (`src/routes/login.ts` — rate limiting, constant-time password verification, and dummy hash fallback), pins each question to its exact line range, enables the answer boxes, and shrinks the load button into a top-bar **Reset** link for one-click demo retakes.
3. **Explain & Grade (`Partial → Revise → Solid`)**: Submitting an answer evaluates which hidden rubric points are covered. Deterministic code derives `Solid`, `Partial`, or `Missed` and pins red-pen margin notes directly onto the relevant diff lines pointing out what to inspect next.
4. **Unlock the Stamp & Export Sign-Off**: When all three questions reach `Solid` (`3 of 3 solid`), the rubber-stamp **UNDERSTOOD** lands in the top bar and unlocks a copyable Markdown **SignedOff Note** ready to paste into a PR description or commit message.

## Done Looks Like
- Clicking **Load sample diff** reliably loads the login-hardening diff and activates the 3 comprehension cards with zero external API keys or network latency risk.
- Submitting an incomplete answer to Question 1 or Question 2 triggers an amber `Partial` chip and a red-pen margin pointer on the exact diff lines that were missed, then revising the answer upgrades it to a green `Solid` chip in one take.
- Reaching `3 of 3 solid` animates the red rubber-stamp **UNDERSTOOD** badge into the top slot and generates a signed-off comprehension summary with a one-click copy action.

## Boundaries

### Now (Guaranteed Proof-of-Concept Path)
- Bundled sample diff (`src/routes/login.ts` — AI agent hardening a login endpoint) loaded via a primary button.
- Three pre-written comprehension questions (`What changed?`, `Why this design?`, `What about an edge case?`) pinned to specific diff line ranges.
- Hidden rubric matcher where evaluation reports covered rubric points and deterministic code derives `Solid` / `Partial` / `Missed` verdicts and exact line pointers.
- Red-pen margin callouts on the diff lines and a one-click **Reset** link in the top bar for retakes.
- Rubber-stamp **UNDERSTOOD** gate (`3 of 3 solid`) and copyable Markdown sign-off note.

### Later (Stretch Goals)
- Paste your own diff with comprehension questions and rubric points generated on the fly via an LLM API.
- Multiple bundled diff scenarios (e.g., database transaction retry, webhook signature verification) selectable from a drawer.
- CLI pre-push hook or GitHub PR action integration.

### Cut (Deliberately Excluded)
- User accounts, authentication, or cloud database persistence.
- Full Git client, branch management, or multi-file repository tree browsers.
- Open-ended AI chat tutor threads that give away the answer instead of pointing to the code lines.
