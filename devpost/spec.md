---
status: approved
project: SignedOff
skill: 4-spec
---
# Technical Specification (Spec) — SignedOff

## 1. Architecture Overview
To guarantee zero network failures during live demo recording and enable instant previewing in both Canvas and any static browser host (GitHub Pages / local file), **SignedOff** is architected as a self-contained browser application (`index.html` / `signedoff.html`) with strict separation between **Rubric Point Detection (`matchRubricPoints`)** and **Deterministic Verdict Derivation (`deriveVerdictAndPointers`)**.

```
[Bundled Sample Diff + Hidden Rubric Store]
                  │
                  ▼
      [User Submits Explanation]
                  │
                  ▼
  1. Rubric Point Matcher (`evaluateRubricCoverage(questionId, answerText)`)
     → Returns ONLY `{ coveredPointIds: string[], missingPointIds: string[] }`
                  │
                  ▼
  2. Deterministic Code Derivation (`deriveQuestionState(question, coverage)`)
     → Computes verdict:
        • all points covered  => "solid"
        • some points covered => "partial"
        • zero points covered => "missed"
     → Maps `missingPointIds` to exact diff line numbers & red-pen margin notes
     → Computes aggregate counter (`solidCount` of 3) & `isUnderstoodUnlocked`
```

## 2. Separation of Concerns: Grader vs. Verdict Derivation
Per the core spec decision:
- **Grader / Matcher Layer (`evaluateRubricCoverage`)**: Inspects the user's answer against the question's rubric criteria and outputs **only** a list of covered rubric point IDs (`coveredPointIds: ["rate_limit", "constant_time"]`). It never decides the final UI state, badge color, or line numbers directly.
- **Deterministic Derivation Layer (`deriveQuestionState` & `deriveExamSummary`)**: Pure functions that take `coveredPointIds` and the static rubric definition to deterministically compute:
  1. `verdict`: `'solid'` when `coveredPointIds.length === rubric.length`, `'partial'` when `coveredPointIds.length > 0`, and `'missed'` when `coveredPointIds.length === 0`.
  2. `activePointers`: The exact diff line numbers (`lineNumber`) and red-pen margin note copy for every rubric point **not** yet in `coveredPointIds`.
  3. `stampUnlocked`: Boolean `true` if and only if all 3 questions have `verdict === 'solid'`.

## 3. Data Model & Bundled Sample Diff (`src/routes/login.ts`)
The bundled sample models an AI coding agent hardening a Node/Express login route (`src/routes/login.ts`, lines 1–28):
- **Diff Changes**:
  - **Lines 5–11**: Adds per-IP sliding-window rate limiting (`loginLimiter.consume(clientIp)`) returning HTTP `429 Too Many Requests` with `Retry-After: 60` before touching the database.
  - **Lines 14–21**: Replaces early `if (!user) return res.status(401)` with a constant-time bcrypt check that falls back to `DUMMY_BCRYPT_HASH` when the email is not found, preventing user-enumeration timing side channels.
  - **Lines 7 & 23–27**: Reads `req.headers['x-forwarded-for']` directly without proxy trust validation and keys the rate limiter only by IP rather than `(ip, email)`, creating a proxy header spoofing / shared-NAT lockout edge case.

### Three Questions & Rubric Schema
| Question ID | Prompt | Pinned Lines | Rubric Points (`id` → required concepts & red-pen pointer line) |
| :--- | :--- | :--- | :--- |
| **`q1` (What changed?)** | What two security mechanisms did the agent add to `/api/login` compared to the old handler? | `L5–L21` | 1. `q1_ratelimit` (Line `8`): Pre-query IP rate limiting (`429` before DB lookup).<br>2. `q1_constant_time` (Line `17`): Always running `bcrypt.compare` with `DUMMY_BCRYPT_HASH` instead of returning early when `user` is null. |
| **`q2` (Why this design?)** | Why does the code run `bcrypt.compare` against `DUMMY_BCRYPT_HASH` even when no user exists in the database? | `L14–L21` | 1. `q2_timing` (Line `17`): Equalizes response latency so attackers cannot measure timing differences.<br>2. `q2_enumeration` (Line `20`): Prevents account/email enumeration (distinguishing valid vs. non-existent emails). |
| **`q3` (What about an edge case?)** | Look closely at line 7 and line 8. How could an attacker bypass the rate limiter—or accidentally lock out legitimate users? | `L7–L11` | 1. `q3_xff_spoof` (Line `7`): Spoofing the `x-forwarded-for` header on cada request bypasses IP rate limits if the proxy isn't trusted.<br>2. `q3_shared_ip` (Line `8`): Keying only on IP can lock out innocent users sharing an office/campus NAT IP. |

## 4. File Structure
```
.
├── .gitignore                 # Excludes /devpost/learner-profile.md and .env*
├── LICENSE                    # MIT License
├── README.md                  # Quickstart, architecture, and Devpost Learn overview
├── index.html                 # Self-contained SignedOff web application
└── devpost/
    ├── scope.md               # Approved scope document (status: approved)
    ├── scope.html             # Visual HTML companion for scope.md
    ├── prd.md                 # Approved product requirements document (status: approved)
    ├── spec.md                # Approved technical specification (status: approved)
    ├── checklist.md           # Build slices, verification log, and Code Tour (status: approved)
    └── app-map.html           # Interactive architecture & code tour companion
```

## 5. Verification Plan
- Validate HTML/JS syntax, handler bindings, element IDs, and Canvas compatibility using `check_web_app.py`.
- Verify all four core user flows:
  1. Initial empty exam-paper frame (`0 of 3 solid`, `Understood — locked`, dimmed placeholders).
  2. Clicking `Load sample diff` populates the 28-line diff, enables inputs, and switches the button to a top-bar `Reset` link.
  3. Submitting a partial answer triggers an amber `Partial` badge and injects a red-pen margin pointer directly into the diff line; revising to cover the missing point switches the badge to green `Solid` and clears the red-pen warning.
  4. Reaching `3 of 3 solid` unlocks the `UNDERSTOOD` rubber stamp and reveals the copyable Markdown sign-off note.
