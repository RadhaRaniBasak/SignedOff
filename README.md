# SignedOff — AI Diff Comprehension Check

**AI agents make it easy to ship code you can't explain.** **SignedOff** turns *"it works"* into *"I actually understand it"* through a calm, marked-exam-paper workflow: load an AI-generated diff, answer three pinned comprehension questions (`What changed?`, `Why this design?`, `What about an edge case?`), follow red-pen margin pointers on any partial answer, and unlock the **UNDERSTOOD** rubber stamp and copyable sign-off note once all three answers reach **Solid**.

Built for the Devpost **Build With AI: Basics** hackathon using the [Devpost Learn Skill Pack](https://github.com/challengepost/learn-ai-basics) (`1-start` → `2-scope` → `3-prd` → `4-spec` → `5-build` → `6-ship`).

## Quickstart (Zero Dependencies)

Open `index.html` directly in any browser, or serve the folder locally:

```bash
npx serve .
```

No API keys, database setup, or build steps are required for the guaranteed demo path.

## How the 1-Take Demo Loop Works

1. **Opening Frame**: Opens on a split-pane marked exam paper (`0 of 3 solid`, `Understood — locked`, centered **Load sample diff** button, and three dimmed question cards).
2. **Load Bundled Sample Diff**: Click **Load sample diff** to load `src/routes/login.ts` (28 lines showing an AI agent adding IP rate limiting and constant-time dummy-hash password comparison) and pin each question to its exact line range.
3. **Partial → Red-Pen Line Pointer → Revise → Solid**:
   - Click **🎬 Load 1-Take Video Preset** (or click **Try Partial** on Q1) and check the answer.
   - Q1 receives an amber **Partial (1/2)** chip and pins a **red-pen margin pointer directly onto Line 17** (`DUMMY_BCRYPT_HASH`).
   - Click **Revise to Solid** on Q1 (or add the constant-time dummy hash explanation) and click **Re-grade Answer**.
4. **Unlock the Stamp & Copy Sign-Off Note**: Reaching **3 of 3 solid** lands the green **✓ UNDERSTOOD** rubber stamp in the top bar and reveals a formatted Markdown **SignedOff Note** ready to paste into a pull request or commit message.

## Planning & Build Artifacts (`devpost/`)

- [`devpost/scope.md`](devpost/scope.md) & [`devpost/scope.html`](devpost/scope.html) — Problem statement, core loop, and Now / Later / Cut boundaries (`status: approved`).
- [`devpost/prd.md`](devpost/prd.md) — Product requirements, opening screen layout, marked exam-paper visual aesthetic, and edge cases (`status: approved`).
- [`devpost/spec.md`](devpost/spec.md) — Technical architecture separating rubric point detection (`evaluateRubricCoverage`) from deterministic verdict and line-pointer derivation (`deriveQuestionState`) (`status: approved`).
- [`devpost/checklist.md`](devpost/checklist.md) & [`devpost/app-map.html`](devpost/app-map.html) — Verified vertical build slices, hands-on checkpoints, and code tour (`status: approved`).

## License

MIT
