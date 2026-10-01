# Handoff — making the harness project-neutral

Session date: 2026-10-01. Nothing has been edited yet; this session was analysis only.

## Goal

This folder is the user's reusable Claude configuration for Unity projects. It came from a
previous project and must become neutral: copyable into ANY Unity project without leftovers.

## Findings (audit of every file)

### 1. Leftover content from the previous project(s)

- `docs/agents/SCOPE_AND_AUTHORING.md` — the most affected file. Rules are generic, examples are not:
  - Part 1, rules 1, 2, 4, 5, 8 and 9: Release 3/4, ChattingSpot, WatchingSpot, Slots, NPCs, District, Routes,
    AreaLeans, `IGesturePlayer`/`GestureLogPlayer`, `CityBox.Sandbox`, `Chat_SqNE`, "Square", "waiting for
    someone", Populate.
  - Part 3 is built on `BehaviourSettings`, `CrowdSettings`, `NPCTemplate`, AmbientLife, Clustering. Point 7
    ("generated get-or-create… additive ensure-pass") assumes the old content generator. Point 9 (two-asset
    ceiling) is about those two specific assets.
  - Rule 3: "that is what the project is selling".
- `.claude/skills/tdd-unity/PORTABILITY-CRITERIA.md` — examples "drawn from this codebase" (a tactical/enemy
  game): `EnemySpawner.cs:46`, `MyLogger`, `TacticalGameManager.cs:28`, `TimerManager`, `"TacticalGame"` action
  map, `QuadTreeGrid`, `EnemyModel`/`EnemyPresenter`, `PopulationDensityController`, `ITacticalGroundStrategy`,
  `BG3TacticalGroundController.cs:100`, `m_enemyPrefab`, `ActiveEnemies`. Also asserts project facts: no
  `.asmdef` today, Test Framework installed, `Utils/` files as candidates.
- `.claude/skills/tdd-unity/SKILL.md` — line 22 domain words "(Ground, Grid, Cell, Character,
  Register/Unregister)"; Blocker example uses `QuadTreeGrid`/`EnemyModel`; invocation written as
  `/check-tdd-unity` but the skill is named `tdd-unity`.
- `docs/howTo/assetsPipeline.md` — "character assets … from Mixamo".
- `.claude/skills/to-doc-diagram/SKILL.md` — example path `…\AmbientLife\Documentation~\BehaviourSystemDocumentation.svg` (minor).
- `.claude/CODING_STANDARDS.md` — method-order example `QuadTreeGrid` / `SetMinCellSize` (minor).
- `CLAUDE.md` line 45 — "the District"; line 43 "seed → edit → transcribe-back loop" is the old authoring
  workflow (`SCOPE_AND_AUTHORING.md` Part 2 is now only a placeholder).

### 2. "Evaluated assignment" framing (old project was judged by an evaluator)

- `CLAUDE.md` — "especially the evaluator" (line 14), "read by the person judging my work" (line 61).
- `.claude/QUALITY.md` — "This is how this project will be evaluated"; "Art pipeline fluency" is an assessment criterion.
- `SCOPE_AND_AUTHORING.md` rule 7 and `docs/objectives/outOfScope.md` — "Report material", "the Report's
  'what the next one costs' section".
- `.claude/skills/implement-release/SKILL.md` — "update … IndicationsForEvaluators.md" (file does not exist).

### 3. Dangling / inconsistent references

- `docs/objectives/destinationSpec.md` — points to `.scratch/destination-spec/DECISIONS.md` and says "that log
  predates this revision and has not been reconciled" (old history).
- `.claude/agents/game-designer.md` — Destination Spec at `docs/gameDesign`; real file is `docs/objectives/destinationSpec.md`.
- `.claude/agents/qa.md` — "batch-mode compile and -runTests, per the project CLAUDE.md", but CLAUDE.md says "To be defined".
- `.claude/skills/review-release-plan/SKILL.md` — typo `SCORE_AND_AUTHORING.md`.
- `.claude/skills/implement-release/SKILL.md` — expects a final "Human review" ticket defined nowhere;
  description copy-pasted from `implement`.
- `docs/objectives/outOfScope.md` — link `../CONTEXT.md` should be `../../CONTEXT.md`.

### Side note (not project-specific)

`CODING_STANDARDS.md` contradicts itself: `moveSpeed` / `_cachedTransform` examples break the `m_` rule;
"Tags and Layers" says use constants but shows a string literal.

### Already neutral

`ISSUE_TRACKER.md`, `DOMAIN.md`, `CONTEXT.md`, `GLOSSARY.md`, the agent context files, and skills code-review,
codebase-design, domain-modeling, grilling, grill-with-docs, handoff, implement, improve-codebase-architecture,
prototype, research, to-spec, to-tickets, wayfinder, read-scripts, check-code-todo.
Per-project assumptions (fine, just verify each time): CLAUDE.md Input System section (default
`InputSystem_Actions` asset) and Git LFS setup.

## Decision in progress — `SCOPE_AND_AUTHORING.md` examples

User asked whether examples are needed. Agreed answer: yes for rules 1, 2, 4, 5, 8, 9 and Part 3; rules 3, 6,
7, 11 are fine without. The old feature list is NOT needed: each current example already carries its lesson,
it only needs re-skinning into a neutral setting.

Proposed approach (awaiting user's go-ahead):

1. One small fictional game, in a box at the top labelled "illustrative only — not this project".
2. Every example drawn from that one game (consistent, no scattered nouns).
3. Example names in lowercase, never capitalised like glossary terms (avoid the "District" leak repeating).
4. Suggested genre: tower defense or shop/management sim — user may pick.
5. In the same pass, delete project-bound parts rather than re-skin them: rule 3 "what the project is
   selling", rule 7 "Report material", Part 3 point 7 generator assumption, Part 3 point 9 two-asset rule.

## Next steps

1. Ask the user to confirm the approach above and the genre, then rewrite `SCOPE_AND_AUTHORING.md`.
2. Then, with user approval, work through the other findings (sections 1–3 above).
3. Ask before any git action (folder is not a git repo at the time of writing).

## Suggested skills for the next session

- None required. `/grilling` if the user wants to stress-test the fictional example game first.
