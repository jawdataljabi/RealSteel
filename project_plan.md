# Project Plan — RealSteel

## Status and authorization

**Status: planning-only commit and push authorized; implementation not started or authorized.** No task below is complete merely because this plan exists or is published. Do not install dependencies, scaffold the game, or begin Step 1 until the user explicitly authorizes implementation.

Read [agent_rules.md](agent_rules.md) and [project_details.md](project_details.md) before executing this plan. The only authorized repository is **https://github.com/jawdataljabi/RealSteel.git**.

The intended handoff is the integrated v1 described in the specification: five normal attacks, five robots, two specials each, two arenas, camera and keyboard controls, full match/exhibition flows, polished animation/audio, and fast recovery. Early slices below are internal milestones, not substitute final deliverables.

## How this hierarchy works

- There are **12 general steps**, with **4 substeps per general step**: **48 implementation commit boundaries**.
- Every substep has **2 or 3 subsubsteps**, for **109 subsubsteps total**. There are exactly three task levels; do not create an untracked fourth task hierarchy.
- Example: `6` is a general step; `6.2` is a substep; `6.2.3` is a subsubstep.
- Execute in numeric order. A later task must not be silently skipped or substituted. If a real dependency requires a change, update the plan and explain why before proceeding.
- A checkbox is checked only when the associated work is actually complete. Check a parent only after its children and exit criteria are complete.
- Keep at most one substep active for the lead agent. Parallel helpers, if used, must have bounded, nonconflicting assignments within that substep.
- Verification is part of every substep, even when its leaves do not repeat the word "test." The agent performs those checks; the user is not assigned an engineering test checklist.
- **Commit once at the end of each completed second-level substep**, e.g. `6.2`, after its leaves, checks, and progress updates are complete. Do not commit at every `6.2.x` leaf or wait until all of Step 6 is finished.
- **Use the substep title as the short commit message**, omitting numbering/checkboxes. Example: completing `6.2 Recognize five normal attacks` produces `Recognize five normal attacks`.
- No co-author trailer or automatic attribution footer. No automatic push; pushing requires a separate explicit user request.
- Update this plan's checkboxes and concise verification/progress notes in the same substep commit. A commit hash can be recorded in a subsequent update if useful; do not amend only to make a commit contain its own hash.
- The user has requested a separate planning-only commit and push of these three documents. That documentation commit is outside the 48 implementation commit boundaries and does not authorize starting the plan.
- If a substep is blocked, leave it incomplete and report the concrete blocker. An optional integration can be explicitly omitted with a documented reason; do not pretend it was implemented.

## Active work and handoff notes

- Active substep: **none — awaiting authorization**.
- Completed substeps: **0 / 48**.
- Latest verification: planning-document structure/content checks only; no application exists yet.
- Known environment unknowns: demo laptop/webcam, room/framing, deadline, sponsor access, and exact current sponsor rules.
- Live two-person recognition: **not yet observed or measured**.
- Next action: publish the planning documents as requested, then wait for the user's explicit implementation go-ahead.

When work begins, keep this summary accurate. Preserve important decisions, blockers, and measured limitations in a short chronological notes section at the bottom rather than burying them in chat.

---

## [ ] 1. Establish the application foundation

### [ ] 1.1 Initialize browser project

- [ ] **1.1.1** Record the user's implementation authorization and update the planning-only status notices; create the Vite/TypeScript/Phaser application with a minimal bootable page and strict TypeScript configuration while preserving existing repository content.
- [ ] **1.1.2** Establish reproducible install/dev/build/typecheck scripts, commit the lockfile, document the supported Node/browser baseline, and extend the existing ignore rules for the chosen toolchain without deleting unrelated rules.

**Done when:** the app starts and builds from the documented commands, the browser shows the boot page, and the repository/remote preflight passes. **Commit:** `Initialize browser project`.

### [ ] 1.2 Define modules and content contracts

- [ ] **1.2.1** Establish the simulation, input, vision, gestures, rendering, UI, audio, content, and diagnostics boundaries described in the specification; keep the combat layer independent of Phaser and camera APIs.
- [ ] **1.2.2** Define typed contracts for fighter/move/mode/arena data, input commands, authoritative combat events, pose messages, and session generations; add runtime validation where malformed external/content data could break startup.

**Done when:** the contracts compile and have representative valid/invalid examples without requiring a camera or rendering context. **Commit:** `Define modules and content contracts`.

### [ ] 1.3 Establish local asset loading

- [ ] **1.3.1** Add a local asset manifest, predictable paths, preload progress/error UI, and source-versus-runtime asset organization; do not depend on a CDN for the eventual warmed demo.
- [ ] **1.3.2** Establish asset provenance/license recording and reproducible preparation scripts, including an explicit strategy for model/runtime file versions and generated assets.

**Done when:** a representative local asset loads, a missing asset produces a useful error, and preparation inputs/outputs are documented. **Commit:** `Establish local asset loading`.

### [ ] 1.4 Establish validation harness

- [ ] **1.4.1** Add focused simulation/recognizer test infrastructure, deterministic fixtures, and scripts for typecheck, test, and build.
- [ ] **1.4.2** Add a browser smoke-check path and a small internal diagnostics foundation; document how the agent can run checks without asking the user to execute them.

**Done when:** a unit check and browser boot check run, and their limitations are recorded honestly. **Commit:** `Establish validation harness`.

## [ ] 2. Build simulation and shared input

### [ ] 2.1 Build fixed-step world

- [ ] **2.1.1** Implement the logical arena, fixed 60 Hz simulation, fighter anchors, stage bounds, and separate render interpolation.
- [ ] **2.1.2** Bound catch-up behavior, define simulation pause/hit-stop clocks, and verify equivalent outcomes at different render rates and after a long frame interruption.

**Done when:** the same input sequence yields consistent state independent of rendering cadence, without runaway catch-up. **Commit:** `Build fixed-step world`.

### [ ] 2.2 Unify player inputs

- [ ] **2.2.1** Implement canonical actions, held-state handling, one-shot commands, per-player control source selection, and a bounded input buffer.
- [ ] **2.2.2** Implement the documented keyboard map and operator shortcut precedence, including repeat suppression, text-field focus handling, release on blur, and command-generation invalidation on resets.

**Done when:** both keyboard players can emit all required actions through one adapter-independent command interface without sticky or duplicated inputs. **Commit:** `Unify player inputs`.

### [ ] 2.3 Implement fighter movement

- [ ] **2.3.1** Implement grounded locomotion, legal facing changes, pushboxes, stage clamping, and explicit fighter state transitions.
- [ ] **2.3.2** Implement automatic approach using mutual basic-attack reach and a margin, movement suspension in ineligible states, and keyboard movement override.
- [ ] **2.3.3** Implement simulated jump/gravity/landing and bounded knockback; verify that two approaching fighters cannot cross or oscillate indefinitely at contact.

**Done when:** both fighters move, approach, separate, jump, and land predictably with authoritative collision bodies. **Commit:** `Implement fighter movement`.

### [ ] 2.4 Implement attack lifecycle

- [ ] **2.4.1** Implement startup, active, recovery, interruption, acceptance/rejection, attack IDs, and per-target hit bookkeeping from move definitions.
- [ ] **2.4.2** Emit authoritative, typed combat events for downstream animation/HUD/audio and add a debug view of active state and regions.

**Done when:** a placeholder attack advances through exact ticks, rejects illegal starts, and produces no duplicate target hits. **Commit:** `Implement attack lifecycle`.

## [ ] 3. Complete normal combat and match rules

### [ ] 3.1 Resolve hits and reactions

- [ ] **3.1.1** Implement local-to-world hurtboxes/hit regions, posture-specific regions, overlap checks, health updates, hitstun, knockback, and hit-stop.
- [ ] **3.1.2** Resolve simultaneous interactions fairly and check contact boundaries, stage edges, single-hit deduplication, and harmless decorative geometry.

**Done when:** damage is caused only by legal active contact and does not depend on player iteration order. **Commit:** `Resolve hits and reactions`.

### [ ] 3.2 Add five normal attacks

- [ ] **3.2.1** Implement straight and hook/body-blow definitions with different timing, height, damage, and pushback.
- [ ] **3.2.2** Implement uppercut launch/anti-air geometry and overhead hammerfist with its wind-up, descending region, and overhead block category.
- [ ] **3.2.3** Implement the short-range knee strike and verify all five actions through the shared input path, including whiffs, recovery, buffering, and single-hit rules.

**Done when:** all five normals are playable on both fighters and have genuinely distinct effects rather than cosmetic-only differences. **Commit:** `Add five normal attacks`.

### [ ] 3.3 Add defensive actions

- [ ] **3.3.1** Implement guard, crouching guard, block reactions, chip policy, and the simple configured guard-stamina/break system.
- [ ] **3.3.2** Implement crouched hurtboxes and validate high straight versus crouch, body blows versus crouch, and overhead versus standing/crouching guard.
- [ ] **3.3.3** Define airborne attack eligibility, jump evasion, uppercut interaction, and landing recovery; reject unsupported aerial actions consistently.

**Done when:** defensive choices have the documented outcomes and do not create sticky invulnerability or permanent guard-break loops. **Commit:** `Add defensive actions`.

### [ ] 3.4 Implement rounds and scoring

- [ ] **3.4.1** Implement countdown, fighting, round result, match result, timer, knockout, first-to-two scoring, and standard round resets.
- [ ] **3.4.2** Handle simultaneous KO, timeout health-percentage comparison, drawn-round replay, pause timing, and state cleanup between rounds.

**Done when:** scripted keyboard matches can reach real round/match results and restart without stale attacks or invalid score. **Commit:** `Implement rounds and scoring`.

## [ ] 4. Create original robot art and animation

### [ ] 4.1 Build robot asset pipeline

- [ ] **4.1.1** Define a cohesive metallic visual style, source SVG conventions, rig part names, pivots, layer ordering, and approximately 18–24 parts per robot.
- [ ] **4.1.2** Implement reproducible rasterization/atlas preparation and metadata loading with source provenance; avoid runtime asset generation or independently generated animation frames.

**Done when:** editable source parts reproducibly become correctly attached runtime textures at gameplay resolution. **Commit:** `Build robot asset pipeline`.

### [ ] 4.2 Finish first articulated robot

- [ ] **4.2.1** Draw and assemble the first complete original robot with a readable silhouette, armor details, joints, face/core accents, and correct facing/layer behavior.
- [ ] **4.2.2** Author idle, walk, straight, and hit-reaction clips; inspect the full-size result internally for sliding feet, broken pivots, weak poses, and paper-puppet appearance.

**Done when:** one robot looks intentional and convincing in motion; this internal milestone does not require user art approval to continue. **Commit:** `Finish first articulated robot`.

### [ ] 4.3 Animate complete normal moveset

- [ ] **4.3.1** Author hook, uppercut, hammerfist, and knee poses with distinct anticipation, impact, and recovery, tied to combat ticks.
- [ ] **4.3.2** Author guard, crouch, jump/landing, guard break, KO, victory, and reusable special-charge poses.
- [ ] **4.3.3** Connect animation markers/effect origins and verify visual contact against authored hit regions, including facing flips, modified timing, and simulated root motion.

**Done when:** all current combat states have appropriate animation and visual contact agrees with simulation. **Commit:** `Animate complete normal moveset`.

### [ ] 4.4 Produce five robot designs

- [ ] **4.4.1** Produce the remaining four robots with distinct geometry, proportions, silhouettes, materials, and portraits/component records—not palette swaps.
- [ ] **4.4.2** Adapt shared clips and hurtboxes to each rig, add character-specific stance/accent motion, and inspect every normal on all five designs.

**Done when:** all five robots render and animate correctly from both sides, with proportions and collisions that agree. **Commit:** `Produce five robot designs`.

## [ ] 5. Integrate local camera and two-player tracking

### [ ] 5.1 Integrate pose worker

- [ ] **5.1.1** Add pinned local MediaPipe model/runtime assets and a camera permission/startup path with actual negotiated dimensions and useful error states.
- [ ] **5.1.2** Implement video inference through the supported worker/delegate path with at most one in-flight job, newest-frame replacement, timestamps, sequence numbers, and stale-result rejection.
- [ ] **5.1.3** Implement worker/frame resource cleanup and a labeled keyboard fallback; inspect actual capability/performance rather than assuming GPU or worker availability.

**Done when:** the app can obtain camera poses locally on an available supported setup, or reports a precise environment limitation without crashing; fixtures do not count as proof of live camera accuracy. **Commit:** `Integrate pose worker`.

### [ ] 5.2 Lock two player identities

- [ ] **5.2.1** Implement raw/mirrored coordinate transforms, anatomical handedness handling, visible player zones, and initial slot acquisition.
- [ ] **5.2.2** Implement two-person matching using torso continuity, scale, velocity, and zone plausibility; exercise reordered detections, missing people, bystanders, and ambiguous crossing fixtures.

**Done when:** model array order cannot swap P1/P2 and ambiguous identity produces a recovery state rather than a silent relabel. **Commit:** `Lock two player identities`.

### [ ] 5.3 Build player calibration

- [ ] **5.3.1** Implement guided framing/neutral capture, required-joint readiness, body-scale/guard baselines, and brief practice-punch adaptation as in-game onboarding.
- [ ] **5.3.2** Persist calibration only for the current assigned player/session, expose clear progress and framing hints, and distinguish same-player rematch from new-player calibration.

**Done when:** calibration produces valid per-player normalized references and refuses missing/invalid measurements without assigning the user an engineering test task. **Commit:** `Build player calibration`.

### [ ] 5.4 Handle tracking interruptions

- [ ] **5.4.1** Implement confidence gating, held-input release, candidate disarming, sustained-loss pause, and stable reacquisition/countdown.
- [ ] **5.4.2** Handle camera disconnect, session replacement, scene transitions, and assignment generations while keeping ordinary rematches from reopening the camera or model.

**Done when:** loss/recovery cannot produce phantom attacks, identity theft between slots, or stale worker results entering a new session. **Commit:** `Handle tracking interruptions`.

## [ ] 6. Implement the full gesture vocabulary

### [ ] 6.1 Build gesture feature pipeline

- [ ] **6.1.1** Compute normalized body features, time-based velocities, bounded motion history, modest smoothing, confidence masks, and configurable thresholds.
- [ ] **6.1.2** Implement reusable candidate/commit/rearm state machines, explicit recognition-versus-acceptance reporting, and debug traces from replayable pose fixtures.

**Done when:** gesture logic is independently executable and does not depend on raw pixel thresholds or an assumed camera FPS. **Commit:** `Build gesture feature pipeline`.

### [ ] 6.2 Recognize five normal attacks

- [ ] **6.2.1** Implement front-facing straight, compact hook, and uppercut recognition with direction/start-pose arbitration; inferred depth is supporting evidence, not a trusted depth sensor.
- [ ] **6.2.2** Implement one-arm overhead hammerfist and unilateral knee strike, including exclusions for uppercut return, jump, crouch, and two-hand special motion.
- [ ] **6.2.3** Wire all five recognizers to shared commands and exercise left/right arms, simultaneous players, retraction requirements, held extensions, ambiguous motion, and tracking gaps.

**Done when:** one intentional strike produces at most one normal attack classification and all five commands can reach real combat through the camera adapter. Document remaining real-camera accuracy limits. **Commit:** `Recognize five normal attacks`.

### [ ] 6.3 Recognize defense and jumping

- [ ] **6.3.1** Implement stable guard and crouch states with entry/release events, hysteresis, confidence handling, and attack precedence.
- [ ] **6.3.2** Implement jump recognition using body rise and available leg/foot evidence; distinguish knee lifts/camera shifts and show insufficient-framing guidance for unsupported observations.

**Done when:** guard/crouch do not stick, crouch holds do not spam events, and knee raises do not inherently trigger jumps. **Commit:** `Recognize defense and jumping`.

### [ ] 6.4 Implement special gesture commands

- [ ] **6.4.1** Implement the two-hands-up dwell/arm/lower-to-fire sequence, immediate charge feedback, timeout/cancel behavior, and suppression of conflicting arm attacks.
- [ ] **6.4.2** Implement double-crouch-release then straight within a timed command buffer, consuming the final punch once; support direct keyboard special commands.
- [ ] **6.4.3** Wire selected exhibition easy-slot behavior if included, and verify meter/recovery rejection, tracking interruption, session reset, and no special-to-normal duplicate emission.

**Done when:** both special commands are available from gestures without firing accidentally during ordinary hammerfists or carrying charge into a new round. **Commit:** `Implement special gesture commands`.

## [ ] 7. Complete roster behavior and ten specials

### [ ] 7.1 Define five robot playstyles

- [ ] **7.1.1** Finalize data definitions for Foundry-9, Relay-7, Needle-2, Cinder-4, and Coil-5 with distinct stats, proportions, normal timing/reach, and two named special slots each.
- [ ] **7.1.2** Connect definitions to selection, rigs, collision, and shared move logic; validate all content references and avoid per-character copies of the entire combat engine.

**Done when:** selecting a robot changes its actual playstyle as well as its appearance, and every character exposes the required move inventory. **Commit:** `Define five robot playstyles`.

### [ ] 7.2 Build melee special attacks

- [ ] **7.2.1** Implement Foundry-9's Armored Ram and Relay-7's Rocket Straight with bounded simulated travel, interruption/armor rules, and collision-safe stopping.
- [ ] **7.2.2** Implement Needle-2's Slipstream Rush and Turbine Flurry with afterimages, explicit hit counts/intervals, and finite cumulative damage.
- [ ] **7.2.3** Implement Coil-5's Magnetic Rise with rising collision, electrical presentation, and correct landing/recovery.

**Done when:** these five specials have complete animation, gameplay, meter hooks, event/effect origins, and no unlimited travel or stun loops. **Commit:** `Build melee special attacks`.

### [ ] 7.3 Build ranged and area specials

- [ ] **7.3.1** Implement Relay-7's Optic Burst and Cinder-4's Core Lance with readable beam origins, charge tells, active durations, and bounded per-target damage.
- [ ] **7.3.2** Implement Foundry-9's Seismic Drive and Coil-5's Arc Pulse with distinct grounded/local-area geometry and appropriate evasion/guard behavior.
- [ ] **7.3.3** Implement Cinder-4's Mortar Bloom with visible arc, explosion, per-target hit policy, finite lifetime, and reset cleanup.

**Done when:** the other five specials are complete and their collision behavior matches the visible beam, wave, pulse, or explosion. **Commit:** `Build ranged and area specials`.

### [ ] 7.4 Tune meter and matchups

- [ ] **7.4.1** Complete special meter gains/costs, accepted-move debit timing, interruption rules, and per-mode damage budgets across all ten specials.
- [ ] **7.4.2** Exercise all robots and special pairs internally; adjust attack reach, auto-approach separation, recovery, guard damage, launch loops, and hit-stop without erasing archetype differences.

**Done when:** each robot has usable normals and two usable specials, no known severe infinite loop, and visible/consistent resource rules. **Commit:** `Tune meter and matchups`.

## [ ] 8. Finish arenas and game presentation

### [ ] 8.1 Create two finished arenas

- [ ] **8.1.1** Build the Foundry Pit with layered industrial depth, floor clarity, restrained ambient motion, and readable contrast for every robot.
- [ ] **8.1.2** Build the Storm Deck with a distinct skyline/palette/weather treatment using the same clear flat combat geometry and bounded effects.

**Done when:** both arenas are selectable, complete, and performant; no procedural map generator is required. **Commit:** `Create two finished arenas`.

### [ ] 8.2 Build combat HUD

- [ ] **8.2.1** Implement health/trailing damage, portraits/names, round score, timer, special meter, guard stamina, and special charge/readiness prompts.
- [ ] **8.2.2** Integrate a clean mirrored camera inset with P1/P2 labels, readiness, short recognized-action feedback, and a separate optional debug skeleton overlay.

**Done when:** damage, identity, charge, and match state are readable during real action without the camera/debug UI dominating the scene. **Commit:** `Build combat HUD`.

### [ ] 8.3 Polish combat spectacle

- [ ] **8.3.1** Add differentiated hit sparks, trails, damage reactions, special emphasis, controlled shake, and KO presentation driven by authoritative events.
- [ ] **8.3.2** Verify effects timing, target/origin placement, layer ordering, visibility, particle caps, and reduced-shake/flash behavior across every robot and stage.

**Done when:** basic hits feel substantial and specials visibly exceed them without hiding the fight or inventing impacts. **Commit:** `Polish combat spectacle`.

### [ ] 8.4 Build menus and operator UI

- [ ] **8.4.1** Build title, mode selection, five-character selection, arena selection, controls/settings, loading/error views, and readable result screens.
- [ ] **8.4.2** Build clickable pause/rematch/new-player/swap/exhibition controls with corresponding shortcut hints and responsive layout/focus behavior.

**Done when:** all required screens and operator affordances exist with clear state transitions ready for full flow integration. **Commit:** `Build menus and operator UI`.

## [ ] 9. Connect complete play and recovery flows

### [ ] 9.1 Connect standard match flow

- [ ] **9.1.1** Connect selection, input source, camera setup/calibration, ready countdown, first-to-two play, results, and rematch into one continuous user path.
- [ ] **9.1.2** Preserve useful local settings and selections while keeping session/player state correct; verify back navigation, keyboard-only play, and camera re-entry.

**Done when:** a standard match can be started, completed, and replayed without developer intervention or browser reload. **Commit:** `Connect standard match flow`.

### [ ] 9.2 Build exhibition preset

- [ ] **9.2.1** Implement labeled single-round exhibition with near-range starts, full special meter, shared normalized HP/damage rules, and short results/countdown.
- [ ] **9.2.2** Verify the intended basic-hit-to-special finish using actual collision and disclosed damage, plus missed/blocked special behavior and immediate access to the selected showcase robot.

**Done when:** the judge path is short and repeatable without hidden auto-hits, auto-attacks, or a fabricated winner. **Commit:** `Build exhibition preset`.

### [ ] 9.3 Add optional interactive tutorial

- [ ] **9.3.1** Implement a skippable sequence teaching straight, an alternate normal, guard, automatic approach, and the easy special through the real input/combat system.
- [ ] **9.3.2** Add a compact reference for all five normals and advanced commands; distinguish tutorial completion from required camera readiness.

**Done when:** a new player can learn the basics quickly, skip the tutorial, and still access all moves without a long prerequisite sequence. **Commit:** `Add optional interactive tutorial`.

### [ ] 9.4 Finish reset and player handoff

- [ ] **9.4.1** Implement quick same-player rematch that clears combat/projectile/input/gesture state but retains camera, worker, assets, assignments, and calibration.
- [ ] **9.4.2** Implement new-player rebind and explicit assignment swap with correct calibration/session handling and clear ready feedback.
- [ ] **9.4.3** Implement exhibition restore, pause/resume, and repeated lifecycle checks; measure same-player turnaround without claiming a target as an observed result.

**Done when:** the operator can recover from a KO, new judge, wrong assignment, or interrupted session quickly without stale actions or a reload. **Commit:** `Finish reset and player handoff`.

## [ ] 10. Complete audio and optional voice

### [ ] 10.1 Add combat sound library

- [ ] **10.1.1** Source or create licensed/original whoosh, metal impacts, block, movement, landing, charge, discharge, KO, and UI sounds; record provenance and preload/decode them locally.
- [ ] **10.1.2** Route sounds from combat events and animation markers with appropriate variation, voice caps, and no damage sound on a whiff.

**Done when:** movement and all attacks have suitable local audio with correctly differentiated hit/block/miss behavior. **Commit:** `Add combat sound library`.

### [ ] 10.2 Add retro music and mixer

- [ ] **10.2.1** Integrate a coherent licensed/original instrumental retro combat loop, victory sting, and appropriate menu treatment; inspect loop seams.
- [ ] **10.2.2** Implement separate music/SFX/voice levels, mute, audio unlock/resume, event-driven ducking, and persisted settings without clipping or excessive overlap.

**Done when:** basic game audio works after one clear user interaction and never requires a streaming service. **Commit:** `Add retro music and mixer`.

### [ ] 10.3 Add nonblocking announcer

- [ ] **10.3.1** Implement a local/cached announcer interface, short ready/special/KO cues, captions, priority, and stale-line expiry.
- [ ] **10.3.2** If approved and credentials are available, integrate Cartesia for useful hands-free guidance/personalized setup lines through a minimal credential adapter; otherwise explicitly document the omission and retain local behavior.
- [ ] **10.3.3** Verify delayed/failed voice requests, mute, round transitions, and commentary cancellation without blocking combat or misrepresenting cached output as live generation.

**Done when:** announcing is useful and brief, optional network failure cannot break play, and any claimed sponsor use is real. **Commit:** `Add nonblocking announcer`.

### [ ] 10.4 Polish audiovisual readability

- [ ] **10.4.1** Align heavy-hit/special sounds, hit-stop, health loss, camera emphasis, and charge cues; adjust the mix for laptop playback and spoken pitching.
- [ ] **10.4.2** Finish reduced-shake/flash settings, captions, color-independent player labels, and a convenient presentation mute; verify that accessibility settings do not change combat outcomes.

**Done when:** the game remains legible and understandable with reduced effects or muted voice, and audio does not overwhelm the presenter. **Commit:** `Polish audiovisual readability`.

## [ ] 11. Validate reliability and performance

### [ ] 11.1 Verify combat and content coverage

- [ ] **11.1.1** Run systematic agent-owned checks covering all five normals on all five robots, all ten specials, defensive categories, meter, simultaneous hits, draws, and standard/exhibition scoring.
- [ ] **11.1.2** Exercise the playable keyboard game and visual region overlays, fix discovered contact/animation/balance regressions, and retain focused regression checks for repaired defects.

**Done when:** required combat/content is complete and known severe engine/content failures are fixed or reported as explicit blockers, not buried in a completion claim. **Commit:** `Verify combat and content coverage`.

### [ ] 11.2 Verify camera control behavior

- [ ] **11.2.1** Exercise timestamped fixtures for all gestures, confidence loss, reordered detections, player ambiguity, simultaneous activity, special arbitration, and reset/rebind sequences.
- [ ] **11.2.2** Use live camera observation or representative permitted footage if actually available; distinguish it from fixtures and do not require the user to run an engineering test session.
- [ ] **11.2.3** Fix reproducible recognition/integration faults and document implemented behavior, measured conditions, and remaining real-world uncertainty separately.

**Done when:** the camera path is integrated and internally checked, with an honest evidence record rather than unsupported precision claims. **Commit:** `Verify camera control behavior`.

### [ ] 11.3 Profile and bound runtime work

- [ ] **11.3.1** Measure render/simulation/inference timing where available, frame age, input acceptance delay, memory/resource growth, and repeated-rematch behavior; separate attack startup from recognition delay.
- [ ] **11.3.2** Tune resolution/model sampling, particle/audio/projectile caps, allocations, and cleanup; ensure the newest-frame policy and configurable quality levels preserve responsiveness.

**Done when:** performance figures identify the actual environment and workload, and known unbounded work/resource leaks are removed. **Commit:** `Profile and bound runtime work`.

### [ ] 11.4 Harden browser lifecycle

- [ ] **11.4.1** Exercise camera denial/disconnect, unavailable model/assets, worker failure, network loss for optional features, tab blur, resizing, and repeated scene transitions.
- [ ] **11.4.2** Fix recoverable failures, add clear fallback/retry messages, verify camera/audio teardown, and confirm no accidental recording/upload or exposed credentials.

**Done when:** expected demo interruptions lead to useful recovery rather than blank screens, phantom inputs, or a broken match. **Commit:** `Harden browser lifecycle`.

## [ ] 12. Prepare the complete playable handoff

### [ ] 12.1 Complete end-to-end play pass

- [ ] **12.1.1** Run full standard, exhibition, tutorial, rematch, and new-player UI flows using agent-controlled inputs; inspect all five robots, both arenas, and the major normal/special presentation paths.
- [ ] **12.1.2** Rehearse the three-minute sequence through the actual app, remove unnecessary navigation/delays, and fix remaining high-impact gameplay/visual/audio issues without silently cutting required features.

**Done when:** the whole application is coherent and playable, not merely a collection of separately passing modules. **Commit:** `Complete end-to-end play pass`.

### [ ] 12.2 Package reproducible local demo

- [ ] **12.2.1** Verify the production build and fresh documented install/start path, local model/assets, and warmed-network-independent core play; identify any initial download requirements honestly.
- [ ] **12.2.2** Update the README and concise operator guide with setup, framing, controls, modes, audio unlock, reset/rebind shortcuts, optional service configuration, and troubleshooting.

**Done when:** a collaborator can launch the actual game from repository instructions without reconstructing the chat or guessing missing asset steps. **Commit:** `Package reproducible local demo`.

### [ ] 12.3 Document actual sponsor work

- [ ] **12.3.1** Record only the integrations/workflow tools actually used, verify applicable challenge requirements where possible, and capture genuine provenance/stage-log material if a workflow entry is pursued.
- [ ] **12.3.2** Prepare a concise factual sponsor/demo explanation and document omitted conditional features; do not fabricate Figma/agent usage, claim MediaPipe as Gemini API use, or add unrelated scope to chase a prize.

**Done when:** sponsor claims match the delivered project and missing access/eligibility is explicitly distinguished from a completed integration. **Commit:** `Document actual sponsor work`.

### [ ] 12.4 Deliver playable v1

- [ ] **12.4.1** Run final relevant checks, inspect the diff and asset inventory, confirm the authorized repository/remotes, and resolve any remaining required-scope blocker or report it candidly.
- [ ] **12.4.2** Reconcile this plan and the specification with what was actually built; summarize measured behavior, known limitations, and any unobserved target-hardware conditions.
- [ ] **12.4.3** Provide a concise launch/handoff message inviting ordinary play feedback rather than assigning a testing checklist; stop for feedback and do not push without explicit permission.

**Done when:** the user receives an integrated game they can play, accurate instructions, and an honest status report. If required features are missing, do not check this substep as completed v1. **Commit:** `Deliver playable v1`.

---

## Progress, verification, and decision log

Planning-only entry: repository identity was inspected and matches `jawdataljabi/RealSteel`; the initial checkout contained a README and ignore file. No implementation, dependency installation, commit, or push was performed while initially preparing these documents.

Publication authorization: the user subsequently requested a commit and push of these three documents only. Update the authorization notices, publish the documents to the verified RealSteel repository, and stop. All 48 implementation substeps remain unstarted.

After authorization, append concise entries here as work proceeds:

```text
Substep: <e.g. 6.2>
Result: <what actually changed>
Checks: <commands/observations actually performed and their outcomes>
Limitations or decisions: <only material information for the next agent>
Next: <next numeric substep or explicit blocker>
```

This log is not a fourth task level. Keep it factual, short, and committed with the associated completed substep. Do not mark a future task complete based solely on a plan, mock, or unsupported claim.
