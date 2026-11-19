# SPEC — Accesso guidato (Guided Access Focus)

Status: DRAFT v0. Not approved. No implementation code starts before the owner approves this spec (WORKFLOW §2).

## 1. Goal
Chrome extension (Manifest V3) for desktop. The user picks one web page to focus on. The extension blocks navigation to other pages. The user cannot disable the block without completing challenges.

Done for v1 (from the Notion task acceptance criteria):
- The user chooses what to block.
- The block stays on until the user completes the challenges.
- An AI proposes the block plan.

Out of scope: OS-level blocking.

## 2. Data
No external data system. State lives in `chrome.storage` (local). Exact keys are defined in Step 1.

## 3. Features
1. **Smart block rules.** A rule targets one of: a whole site, a single video, or search results. Example: if the user blocks YouTube, YouTube videos do not appear in Google search results.
2. **AI block plan.** The AI proposes a list of rules from the user's goal. The user reviews and edits the plan before it starts.
3. **Hard disable.** The user must complete a sequence of challenges to turn the block off.

## 4. Open questions (owner decision needed)
1. AI provider and model for the block plan (for example the Claude API). Who holds the API key? Options: the user enters their own key in the extension options, or a small backend proxy. Recommendation: the user enters their own key. Stored only in `chrome.storage.local`. No backend.
2. Which challenges? Options: typing a long text, a math sequence, a wait timer, a combination. Recommendation: a typing challenge plus a wait timer, with a limited number of retries.
3. Search result filtering: hide results by domain in Google only, or other search engines too? Recommendation: Google only in v1.
4. Is a build tool allowed (for example TypeScript with Vitest for logic tests)? Recommendation: TypeScript strict and Vitest, as development tools only.
5. What happens if the user removes the extension? Chrome allows it. Recommendation: document the limit. Optionally, use the enterprise policy `ExtensionInstallForcelist` later (not v1).

## 5. Build order (proposed)
1. **Step 1 — Scaffold:** MV3 manifest, service worker, options page, storage model, lint/build/test commands. Check: the extension loads in Chrome.
2. **Step 2 — Block engine:** pure logic for rules (site, video, search results) with unit tests, and navigation blocking with `declarativeNetRequest`. Check: a blocked site cannot open.
3. **Step 3 — Focus session:** pick a page, lock the tab, show a block page. Check: other pages are blocked during the session.
4. **Step 4 — Challenges:** challenge engine and UI. Check: the block cannot be turned off without completing them.
5. **Step 5 — AI block plan:** the planner calls the AI, the user reviews the plan. Check: the plan appears and the user can edit it.

## 6. Workflow settings
- **Playbook source/ref:** Simooo1010/claude-project-playbook, commit 3b4b7ae0c292237cc436d6aa58166e90ee377393 (as named in CLAUDE.md; the playbook files in `docs/` of this repo were read).
- **Profile:** Standard.
- **Approval mode:** Strict (default).
- **Owner:** Simone.
- **Test device(s):** desktop Chrome (owner).
- **Allowed external writes:** none.
- **Approved dependencies:** none yet. See open question 4.
- **Production merge:** owner approval required.
- **Project commands:** not defined yet (Step 1).

## 7. Decision log
| Date | Decision | Decided by | § |
|---|---|---|---|
| 2026-11-19 | Spec draft v0 created from the Notion task brief. Not approved. | Claude (worker) | all |
