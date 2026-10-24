# Agent Rules — RealSteel

These rules apply to the lead coding agent, any delegated agents, and collaborators performing project work. Read this file before changing anything. The user's explicit later instructions take precedence; otherwise these rules remain in effect across sessions.

## 1. Absolute repository boundary

**The only authorized project repository is https://github.com/jawdataljabi/RealSteel.git.**

- Work on this project only in this repository's verified checkout.
- Commit only to this repository.
- Push only to this repository, and only when the user explicitly requests a push.
- **Never commit or push to anything Shopify-related.** Do not use a Shopify repository, fork, organization, remote, worktree, account integration, or deployment target for this project.
- Do not modify another repository, move this project into a different repository, create an alternate remote/fork, or publish a copy elsewhere without an explicit change of instruction from the user.
- Do not use Shopify internal tools, private code, assets, credentials, or services for this project. The presence of unrelated machine-level tools or Git configuration is not permission to use them.
- Ordinary public documentation research and dependency installation are allowed once appropriate to the authorized phase; they do not authorize modifying other checkouts.
- Keep project source, assets, documentation, scripts, and tracked work in this checkout. Normal package caches or ephemeral tool artifacts are not additional project repositories.

### Repository preflight

Before the first write in a session, and again before every commit or push:

1. Confirm the working directory and `git rev-parse --show-toplevel` resolve to the intended RealSteel checkout.
2. Inspect `git status --short` and preserve existing user/collaborator work.
3. Inspect fetch and effective push destinations. Useful commands include `git remote -v`, `git remote get-url --all origin`, and `git remote get-url --push --all origin`.
4. Check unexpected extra remotes, push URLs, URL rewrite rules, or hooks/configuration that could redirect work. The allowed repository identity is exactly `jawdataljabi/RealSteel` on GitHub, not a similar name or a fork. An equivalent SSH URL is acceptable only if it resolves to that same authorized repository.
5. If anything points somewhere else or is ambiguous, stop before the write/commit/push and explain. Do not silently rewrite remote configuration or alter global Git settings to make an operation succeed.

Do not treat an `origin` label as proof of destination. Do not print credentials while inspecting configuration. Never use a global configuration change to affect unrelated repositories.

## 2. Current authorization: documents only

The user has authorized preparation of:

- `project_details.md`: comprehensive product and technical specification.
- `project_plan.md`: three-level implementation task hierarchy.
- `agent_rules.md`: development and repository rules.
- Repository inspection and checks needed to prepare and publish these documents.

The user has subsequently explicitly requested **one planning-only commit and push of these three documents to the authorized RealSteel repository**. This publication request is permitted and is separate from implementation authorization.

**The user has not yet authorized starting the game implementation.** After publishing the documents, stop. Do not scaffold, install dependencies, build assets, activate a webcam, or provision services. Approval or publication of the documents is not an instruction to start implementation; wait for an explicit go-ahead. Future commits and pushes remain subject to the policies below.

When the user does authorize implementation, record that authorization and update the current-phase notices in all three documents as part of the first scheduled substep. Preserve the repository/commit rules; do not leave obsolete planning-only banners that confuse the next agent.

## 3. Document authority and collaboration

Read all three project documents before taking an implementation task:

- [project_details.md](project_details.md) defines the intended product, concrete defaults, constraints, and acceptance behavior.
- [project_plan.md](project_plan.md) defines task order, progress, and commit boundaries.
- This file defines how work is performed and where it may go.

Distinguish required scope from implementation defaults, initial tuning values, optional sponsor features, and deferred ideas. Do not turn a proposed threshold into a claimed measured result or an optional service into a required dependency.

If documents conflict, use the user's explicit instructions as authority, fix the inconsistency, and explain any material change. Do not silently reinterpret the project to fit a convenient implementation.

A new agent should be able to continue from repository documents and the current diff, without needing the entire chat history. Keep meaningful decisions, checks, blockers, and next steps in the plan's progress log.

## 4. Required development workflow

Once implementation is explicitly authorized:

1. Perform repository/status preflight.
2. Read the active task and its dependencies in the plan.
3. Select the next incomplete second-level substep in numeric order.
4. Implement its third-level children and required integration.
5. Run the relevant agent-owned checks and fix issues discovered within the substep.
6. Inspect the diff, update the plan's checkboxes/notes, and update the specification if a meaningful implementation decision changed.
7. Verify the repository and staged changes again.
8. Make one concise commit for that completed substep.
9. Continue to the next substep unless the user has asked to pause or a real blocker prevents progress.

Do not ask for permission after every small implementation detail once the build is authorized. Conversely, do not use that authorization to expand into unrelated features, services, or repositories.

Follow the plan step by step. Internal vertical slices are useful, but they are not final handoffs. If a dependency makes the order genuinely unsuitable, explain and update the plan rather than silently jumping around.

The plan must retain three task layers: 7–13 general steps, 3–5 substeps under each, and 1–3 subsubsteps under each substep. The initial plan uses 12 general steps, four substeps each, and two or three subsubsteps each. Keep IDs stable where possible; do not casually renumber completed work.

## 5. Commit policy

### Commit boundary

**Commit at the end of every completed second-level substep.**

For example:

- `6` is a general step: not the commit boundary.
- `6.2` is a substep: commit when this is complete.
- `6.2.1`, `6.2.2`, `6.2.3` are subsubsteps: finish them before that commit, rather than making one commit for each leaf.

The initial hierarchy therefore has 48 planned implementation commits, excluding any separately requested planning commit or later user-approved work.

Do not combine several completed substeps into one broad commit. Do not create a stream of microcommits for each file or tiny adjustment. Fix issues found before the boundary as part of the same substep. If a defect in an earlier substep is found later, include a clearly related correction in the active substep or document an explicit repair task/boundary if it is substantial. Do not rewrite history just to make the sequence look perfect.

### Commit messages and attribution

- Use the short substep title as the message, e.g. `Recognize five normal attacks`.
- Omit checkbox markup and task numbering from the title unless a later user preference changes this.
- No co-author trailer.
- No automatically added agent credit or attribution footer.
- No need for a long conventional-commit prefix or essay in the message.
- Use the configured author identity; do not alter global identity or fabricate attribution.

### Commit contents

- Stage the intended paths explicitly and inspect the staged diff.
- Include the relevant implementation, assets, checks, and plan/specification progress updates.
- Do not blindly stage unrelated user changes or tool-generated files.
- Never include secrets, API keys, private webcam recordings, dependency directories, build caches, or unlicensed assets.
- Do not mark a task complete if its required work or checks are still missing.
- Do not amend solely to insert a commit's own hash into its documentation. The Git history and next progress update can record it.

The user has requested a separate planning-only commit and push. That documentation commit is outside the 48 implementation boundaries and does not complete any implementation task.

## 6. Push and Git safety

Commit permission is not push permission. **Do not automatically push at each substep.**

When the user explicitly requests a push:

- Reverify the checkout, effective push URL, and intended branch.
- Push only the intended branch/ref to the authorized RealSteel remote.
- Do not use broad mirror/all-remotes operations.
- Never push to Shopify or another repository, even if another remote already exists.
- Do not force-push, delete remote branches, reset shared history, change branch protection, or publish releases without explicit authorization for that operation.

Preserve user changes. Do not use destructive reset/clean commands to hide problems, discard unrelated work, or force a clean status. Do not switch branches, create managed worktrees, or move commits between checkouts without a clear authorized need. If delegated work requires another checkout, obtain explicit permission first; the default is bounded work in this repository only.

## 7. Agent ownership of building and checking

The user expects the agent to build a large portion, potentially the entire game, and deliver something playable. The user is not interested in running technical tests themselves.

Therefore:

- Own implementation, assets, integration, focused checks, debugging, and preparation of the playable handoff.
- Use internal unit checks, controlled input/pose fixtures, scripted matches, browser inspection, and profiling as appropriate.
- Do not repeatedly hand the user a checklist or require a low-level experiment before continuing the build.
- Do not stop at a webcam experiment, art proof, or two-robot slice and describe it as the intended completed v1.
- Do not outsource all sound/art preparation to the user or leave placeholders while claiming finished content.
- In-game calibration and ordinary first-play feedback are legitimate product interactions, not substitutes for the agent's own engineering work.
- User feedback after the first integrated build should drive incremental improvements and fixes.

### Be honest about verification limits

Do not claim to have observed two real people using the user's webcam unless that observation actually occurred or appropriate permitted footage was examined. Synthetic landmark checks do not establish real-world camera accuracy. A successful typecheck does not prove a fun or complete game. A keyboard match does not prove camera input works.

Report what is implemented, what was actually checked, the environment and outcomes, and what remains unobserved. Do not use the lack of physical access as an excuse to leave the camera pipeline unimplemented or to move every check onto the user.

## 8. Product constraints that must survive implementation

- Two simultaneous players, one laptop camera, both facing the lens.
- Straight punches travel toward the camera in real life. Side-on punching is not an acceptable silent substitution.
- Shared game commands for camera and keyboard; camera recognition does not directly change health.
- Five normals: straight, hook/body blow, uppercut, overhead hammerfist, and knee strike.
- Five original robots, two specials per robot, two finished arenas, plus the documented standard/exhibition/tutorial flows.
- Automatic approach makes onboarding easy; it does not attack automatically or fabricate collisions.
- The judge's short demo is a path through a real fighting game, not justification for a one-punch minigame.
- Original, consistently animated robot art and legitimate asset provenance; no copied franchise content.
- Important gameplay and basic audio work locally. Optional sponsor services cannot block attacks, damage, scores, or rematches.
- Fast reset/rebind is a real product requirement, not a developer workaround requiring a browser reload.
- Precision matters: avoid duplicate attacks, sticky holds, silent player swaps, and unsupported claims of perfect depth sensing.

The user favors hackathon impact over production-scale hardening. Keep architecture lean. Do not use that preference to justify credential leaks, nonconsensual footage handling, or ignoring the repository boundary.

## 9. Scope changes and sponsor integrations

Implement reasonable internal choices without interrupting the user for every constant. Keep initial values configurable and document meaningful changes.

Ask or explain before:

- Removing a required move, robot, arena, control path, or mode.
- Replacing front-facing camera control with another physical setup.
- Changing the agreed visual technique to a substantially different asset pipeline.
- Adding a required backend, account system, paid service, external hardware, or major dependency that changes setup friction.
- Expanding into online multiplayer, finance, blockchain, scraping, or other sponsor-driven side projects.
- Incurring charges, creating accounts, accepting new contractual terms, or publishing project assets/data.

Cartesia and Figma are plausible fits, not permission to claim them without real use. Gemini, Lyria, Mana, Factory, hosting, and other sponsor features are conditional. Verify the current challenge requirements and account access. The user is not entering beginner or solo categories.

Never fabricate tool usage, design history, stage logs, live generation, performance figures, or sponsor eligibility. When an optional feature is unavailable, document the omission and preserve the core game rather than blocking delivery.

## 10. Delegation and collaboration

- Every helper must receive the repository boundary, relevant specification, exact task ID, expected deliverable, and write constraints.
- The lead retains responsibility for integration, verification, progress updates, and the substep commit.
- Helpers should not independently commit or push unless the user explicitly changes this workflow. The default is one lead-controlled commit per substep.
- Avoid concurrent edits to the same files and uncoordinated Git/index operations.
- Do not let a helper create another repository/worktree, use Shopify resources, or publish anything as a shortcut.
- Review delegated changes before accepting them. A helper's claim that something works is not a substitute for inspecting its actual changes and evidence.
- Keep any project instructions/artifacts in the authorized repository; do not place project plans or source in an unrelated checkout.

## 11. Communication and stopping conditions

Be clear and concise in progress reports. State which substep is active, what was completed, what checks actually ran, and any material limitation. Show file paths clearly when discussing changes.

Do not promise unmeasured recognition precision, photorealistic art, guaranteed prize eligibility, or performance on unspecified hardware. Do not describe a target latency as an observed result.

Stop and notify the user when:

- The current phase has not been authorized.
- Repository identity or destination is wrong/ambiguous.
- A proposed operation would touch another or Shopify-related repository.
- User changes conflict with the intended work and cannot be safely preserved.
- A required capability is genuinely blocked and resolving it would materially alter scope or incur cost.
- The user requests a pause or review checkpoint.

The initial document-preparation request ended without a commit or push. The user has now authorized publishing those documents: commit and push only that documentation, report the actual result, and wait for implementation authorization. After an authorized completed v1, the correct behavior is to provide launch instructions and invite ordinary play feedback—not assign a laboratory testing session or continue adding unrelated features.
