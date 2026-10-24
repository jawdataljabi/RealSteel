# Project Details — RealSteel

## 1. Purpose, authority, and current status

This document is the comprehensive product and technical specification for a local, camera-controlled, two-player robot fighting game. It is intended to let a new collaborator or coding agent understand the project without access to the original planning conversation.

**Current status: planning documents only. Implementation has not been authorized yet.** The user has explicitly authorized a planning-only commit and push of these three documents to the authorized repository. This does not authorize scaffolding, dependency installation, assets, or game implementation; wait for a separate go-ahead to begin the plan.

The project lives exclusively in the GitHub repository **https://github.com/jawdataljabi/RealSteel.git**. No project work, commits, or pushes may target a Shopify-related repository or any other repository. Read [agent_rules.md](agent_rules.md) before making changes. Execute the three-level task hierarchy in [project_plan.md](project_plan.md) after authorization.

### 1.1 How to interpret this specification

- **Required v1 scope** is the agreed product, not a list of optional aspirations.
- **Implementation defaults** are concrete design decisions chosen to make the project buildable. They may be tuned without asking the user about every constant, provided the required behavior is preserved and documentation stays accurate.
- **Initial tuning values** are proposed starting points, not experimentally verified measurements or final balance guarantees.
- **Conditional enhancements** require the relevant access, credentials, or explicit scope approval. They must not block the core game.
- **Deferred features** are not commitments for the first handoff.
- If a genuine constraint threatens required scope, explain it rather than silently removing features.
- Explicit later user decisions override this document; update the affected documents together.

### 1.2 The user's development expectation

The coding agent is expected to implement a large portion, potentially all, of the game. The user is not a test operator or project manager for individual implementation experiments. The agent owns implementation, asset preparation, incremental checks, debugging, and integration.

The first intended handoff is an integrated game the user can actually play. Their subsequent play sessions will drive incremental improvements. Do not hand off a pose-tracking sandbox, keyboard-only skeleton, unanimated prototype, or a set of disconnected features and call it the completed v1.

The agent may use internal milestones and automated checks, but must not repeatedly require the user to validate those milestones before continuing. Runtime calibration is part of playing the game; an engineering test checklist is not a user prerequisite.

## 2. Product vision and demo priorities

### 2.1 Core experience

Two people stand side by side in front of one laptop webcam, both facing the camera. Each controls an original fighting robot in a side-view arena. Real-world gestures become conventional game inputs, which trigger authored robot animations and deterministic combat rules.

The game is inspired by cinematic robot boxing and arcade fighting games. Do not use copyrighted character names, copied robot designs, franchise logos, ripped animations, or recognizable franchise music. The repository name is not a license to reproduce another property's assets. A final public-facing title is not yet chosen.

The player is a controller, not a motion-capture puppet. The robot does not reproduce every body-joint angle. A recognized uppercut triggers the robot's designed uppercut, with its own timing, reach, hitbox, and recovery.

**Physical camera orientation is non-negotiable:** a real straight punch travels toward the camera lens. Do not quietly require the players to turn sideways to make recognition easier.

### 2.2 Three memorable moments

1. A judge moves and sees their own robot respond.
2. Different normal attacks visibly hit an opponent, with readable health loss and satisfying feedback.
3. The judge deliberately performs a special gesture and unleashes a spectacular attack, potentially finishing the fight.

Fast player acquisition, rematches, and recovery support these moments. A three-minute demonstration should not be consumed by loading, settings, menus, or instructions.

### 2.3 Fast onboarding is not shallow gameplay

The game must have five normal attacks, meaningful defensive interactions, distinct robots, and specials. The demo showcases a subset of that depth. Do not simplify the actual product into a one-punch minigame merely because the pitch is short.

### 2.4 Required v1 feature inventory

- One laptop, one camera, two simultaneous local players.
- Browser-based application with no required remote inference or gameplay backend.
- Stable player assignments, visible camera readiness, calibration, and recovery from tracking loss.
- Camera and keyboard input adapters driving the same game commands.
- Five normal attacks: straight, hook/body blow, uppercut, overhead hammerfist, and knee strike.
- Guarding, crouching, jumping, facing, grounded movement, and automatic approach.
- Five original robots with distinct silhouettes and playstyles.
- Two specials per robot: ten character-specific specials in total.
- Two finished arenas. A third is an optional content expansion.
- Health, attack timing, hitboxes, hurtboxes, hitstun, knockback, blocking, knockout, and a round timer.
- Standard first-to-two-round-wins matches and a clearly identified exhibition preset.
- Optional short tutorial, character selection, arena selection, results, and rematch flow.
- Finished articulated robot animation, HUD, impact effects, special effects, combat sounds, and retro music.
- Operator controls for pause, quick reset, new players, and assignment swapping.
- Agent-owned diagnostics and validation, plus clear run instructions and honest known limitations.

### 2.5 Explicit non-goals for v1

- Training or fine-tuning a new vision model.
- Cloud or LLM-based classification of every webcam frame.
- Online multiplayer, matchmaking, accounts, payments, progression, or persistent player profiles.
- Perfect recognition of arbitrary boxing techniques or unrestricted player movement.
- Side-on camera placement, wearable sensors, or additional hardware as a core requirement.
- Full-body avatar retargeting or photorealistic movie-quality characters.
- Required physical walking/marching recognition. Automatic approach supersedes that earlier idea for v1.
- Gesture-controlled menus; ordinary mouse/keyboard menus are acceptable.
- Requiring sponsor services for combat, damage, scoring, or basic audio.
- Tournament-grade balance, rollback networking, a large combo tree, or elaborate infrastructure.

## 3. Physical setup and assumptions

### 3.1 Player positioning

- Players stand in marked left/right regions of a mirrored camera preview.
- Both face the laptop, not each other, and perform non-contact gestures.
- They remain in their assigned physical zones. Crossing over, swapping places without operator action, and substantial overlap are unsupported for v1.
- Provide enough separation for compact gestures without striking the other player. Do not interpret the original suggestion of a ruler or two of separation as a verified safe distance.
- The setup guide should illustrate a compact hook, a raised knee without kicking, and sufficient overhead clearance for hammerfists and specials.
- Keep the background reasonably uncluttered and both people sufficiently lit.

### 3.2 Framing requirements

The full feature set needs both players' heads, shoulders, arms, hips, knees, and preferably feet visible. The image must also allow hands above head height for the easy special gesture.

- Punches, hooks, uppercuts, hammerfists, and raised-hands specials depend primarily on upper-body visibility.
- Knee strikes require hips and knees; feet are not strictly required for that move.
- Crouch uses hip descent and knee configuration.
- Jump recognition benefits strongly from full-body visibility and should use foot/leg evidence when available.
- A partial-body frame must produce a clear framing warning for unavailable actions, not fabricated landmarks or silent claims of full support.
- The desired distance depends on camera field of view, mounting angle, body sizes, and room geometry. Do not promise a universal distance in meters.

### 3.3 Unresolved environmental information

The exact demo laptop, operating system, webcam, venue lighting, available floor space, team size, and deadline have not yet been supplied. Default to a current desktop Chromium browser. Keep the implementation adjustable rather than pretending performance is already established on a known machine.

These are material unknowns, not instructions to stop every implementation step for user testing. Ask only when a specific decision is genuinely blocked.

## 4. Architecture and technology

### 4.1 Recommended stack

| Concern | Default |
| --- | --- |
| Language | TypeScript with strict checking |
| Development/build | Vite and a committed npm lockfile |
| Game rendering | Phaser with WebGL preferred |
| Simulation | Plain TypeScript, fixed 60 Hz, independent of scene rendering |
| Pose estimation | MediaPipe Tasks Vision Pose Landmarker, VIDEO mode, maximum two poses |
| Inference execution | Dedicated worker where supported; benchmark the actual worker/delegate path |
| Artwork | Original modular SVG source, built into PNG textures/atlas and pivot metadata |
| Audio | Local decoded audio through Phaser/Web Audio |
| Unit/integration checks | Lightweight TypeScript test runner, normally Vitest |
| Browser checks | Playwright or equivalent browser automation when available |
| Required backend | None |
| Optional voice backend | Minimal local/server-side credential adapter only if an approved live sponsor feature needs it |

Avoid introducing React, a database, an ECS framework, a general-purpose editor, or a network service unless a concrete need outweighs its cost. Standard HTML/CSS overlays can handle settings and accessible menus around the game canvas.

OpenCV is not the primary recognition solution. The requirement is pretrained body landmarks, not custom image-processing rules alone. MoveNet MultiPose is a candidate fallback if actual MediaPipe behavior justifies a change; do not implement and ship two pose stacks speculatively.

### 4.2 Logical data flow

```text
Webcam -> newest-frame scheduler -> pose worker -> timestamped landmarks
       -> confidence filtering -> player assignment -> calibrated body features
       -> per-player gesture state machines -> input commands

Keyboard ------------------------------------------------> input commands

Input commands -> fixed-step combat simulation -> authoritative game events
              -> renderer / animation / HUD / sound / optional commentary
```

No renderer, voice service, pose model, or LLM may independently decide health changes or winners.

### 4.3 Proposed module boundaries

```text
src/
  app/             startup, scenes, application lifecycle, mode transitions
  game/            fixed-step state, fighters, combat, collision, rounds
  input/           canonical commands, keyboard and camera adapters, buffering
  vision/          camera capture, worker protocol, assignment, calibration
  gestures/        body features, temporal recognizers, special command parser
  render/          robot rigs, animation playback, arena layers, particles
  ui/              menus, HUD, camera inset, tutorial, operator controls
  audio/           mixer, sound routing, music, announcer abstraction
  content/         robot, move, arena, mode, and tuning definitions
  diagnostics/     timing metrics, debug overlays, replayable input/pose fixtures
assets/
  source/          original editable robot/stage artwork and animation sources
public/
  assets/          prepared runtime textures, audio, models, fonts, manifests
scripts/           reproducible asset preparation and supporting utilities
tests/             game, recognizer, lifecycle, and browser checks
```

This is a proposed organization, not permission to create it during the planning-only phase. Adapt directory names if necessary while preserving separation of responsibilities.

### 4.4 Data contracts

Use typed, inspectable data rather than scene-specific magic values.

- **Pose frame:** sequence number, monotonic capture/inference timestamps, camera-session identifier, image dimensions, detected people, normalized/world landmarks, and per-landmark confidence/visibility.
- **Assigned player pose:** stable logical player slot, assignment generation, body center, confidence, relevant landmarks, and calibration reference.
- **Input command:** logical player ID, action, pressed/released or one-shot semantics, source, timestamp, and input/session generation.
- **Fighter definition:** content ID, display name, rig, proportions, health, movement settings, damage/timing modifiers, hurtboxes, normal move references, and two special references.
- **Move definition:** ID, startup/active/recovery ticks, animation, permitted states, hit regions, damage, block category, hitstun, knockback, meter behavior, and per-target hit policy.
- **Combat event:** unique event/attack ID, source/target, tick, damage and block result, hit position, remaining health, and cosmetic/audio tags.
- **Mode definition:** scoring, timer, health/stat normalization, starting meter, automatic approach, and rematch policy.

Keep combat events factual. Renderers and commentary may decorate them but must not reinterpret results.

## 5. Simulation and input rules

### 5.1 Coordinate and timing conventions

- Begin with a logical 1280 x 720 viewport and a consistent ground line around y = 610. These are implementation defaults, not CSS dimensions.
- Scale and letterbox presentation as needed. Do not derive combat distances from a resized DOM element each frame.
- Fighter position refers to a documented ground/foot anchor. Facing transforms local hit regions into world coordinates.
- Advance combat at 60 fixed ticks per second. Animation phases are tied to simulation ticks.
- Render independently, interpolating presentation if useful.
- Bound catch-up work. A background-tab stall must not simulate seconds of unseen attacks or unleash queued gestures when focus returns.
- Pause on focus loss or a substantial lifecycle interruption; release held inputs.
- Jump height, gravity, landing, root motion, stage bounds, and pushbox resolution belong to simulation, not visual-only tweens.
- Hit-stop intentionally freezes selected combat/animation time; inference can continue, but buffering must remain bounded.

### 5.2 Canonical controls

Required one-shot actions: `STRAIGHT`, `HOOK`, `UPPERCUT`, `HAMMERFIST`, `KNEE`, `JUMP`, `SPECIAL_1`, `SPECIAL_2`.

Required held/state actions: `GUARD`, `CROUCH`, and keyboard horizontal movement. Operator controls are separate from fighter commands.

Camera and keyboard inputs must use the same move validation, resource costs, timing, damage, and collision rules. No camera-only direct damage path is allowed.

Only legal actions enter the current fighter state. Initially allow a short configurable input buffer of approximately 100–150 ms for a deliberate attack near the end of recovery. Do not accumulate a queue of repeated attacks. Reset or discard old commands on pause, KO, round change, player rebind, and mode changes.

Each player has an explicit control source selection. Camera and keyboard together may be enabled for debugging, but must not accidentally double-fire the same action. A recognized gesture can be shown as recognized-but-unavailable when the robot is stunned or still recovering.

### 5.3 Initial keyboard map

| Action | P1 | P2 |
| --- | --- | --- |
| Move left/right | A / D | Left / Right arrows |
| Jump | W | Up arrow |
| Crouch | S | Down arrow |
| Guard | E | O |
| Straight | F | J |
| Hook | G | K |
| Uppercut | H | L |
| Hammerfist | T | I |
| Knee | V | M |
| Special 1 | Q | U |
| Special 2 | R | P |

These mappings are implementation defaults and should be visible in a controls panel. Suppress repeat events for one-shot actions. Do not intercept gameplay shortcuts while the user is editing a text field. Commodity keyboard ghosting is a known hardware limitation; do not mistake it for vision performance.

Proposed operator shortcuts: Escape for pause, Shift+Enter for quick rematch, Shift+C for new-player calibration, Shift+X for assignment swap, and Shift+D for exhibition reset. Provide clickable equivalents. Operator shortcuts take precedence and must not also trigger fighter actions.

### 5.4 Automatic approach

Automatic approach is the primary camera movement scheme for v1.

1. Determine the opponent and face them while the current state permits a facing update.
2. Determine a target separation at which both basic straight attacks can connect against their hurtboxes, with a small margin.
3. When an eligible grounded fighter is idle outside that separation, move toward the opponent at its configured walk speed.
4. Stop before crossing the target distance or violating pushboxes. Account for both fighters moving during the same tick.
5. Suspend approach while attacking, guarding, crouching, charging a special, airborne, stunned, knocked out, paused, or lacking valid tracking for a camera-controlled slot. Keyboard-only players do not require camera tracking.
6. Resume after recovery/landing when legal. This closes space after normal knockback.

Do not freeze an airborne opponent in place or drag a defender into a hit. Never autogenerate an attack. A punch may contain a modest simulated forward step, but not a teleport or a secretly extended full-screen hitbox.

Exhibition rounds should begin near meaningful attack range. Do not make the judge's first punch miss simply because the fighters spawned at opposite edges. Keyboard manual movement overrides automatic approach for that player while held; camera users are not required to physically walk.

### 5.5 Fighter and match states

At minimum distinguish idle/approaching, crouching, guarding, airborne, attack startup, attack active, attack recovery, hitstun, special charge, knocked out, and paused behavior. Use explicit precedence rather than independent animation flags that can contradict one another.

Match lifecycle: loading -> selection/setup -> calibration -> ready/countdown -> fighting -> round result -> next round or match result. Tutorial and exhibition are explicit flows using the same engine.

## 6. Combat: five normal attacks and defenses

### 6.1 Normal attack specification

All five moves are required in v1 for every robot. Their command vocabulary is shared, while artwork, proportions, timing modifiers, reach, and damage vary by robot.

| Move | Physical gesture | Robot action and tactical role | Initial base timing at 60 Hz | Initial base damage |
| --- | --- | --- | --- | --- |
| Straight | Deliberate extension toward the lens | Fast high punch; short recovery; quick pressure | 5 startup / 3 active / 10 recovery | 10 |
| Hook / body blow | Compact horizontal strike across the torso | Heavier body-level arc; more pushback; connects with crouching bodies | 9 / 4 / 17 | 14 |
| Uppercut | Upward strike starting below chest level | Short-range rising punch; launch/anti-air role; punishable recovery | 8 / 5 / 23 | 16 |
| Overhead hammerfist | One fist raised above the head, then deliberate downward strike | Heavy descending hit; defeats crouching guard; standing guard works | 18 / 5 / 25 | 22 |
| Knee strike | One knee rises sharply toward the torso while upright | Very short-range body hit; brief stagger and little pushback | 6 / 4 / 14 | 9 |

These are starting balance values. Apply modifiers as integer simulation ticks, and make animation follow the resulting timings. Do not preserve a visual impact frame that no longer matches the active hitbox after retuning.

The knee requires no leg extension or contact with the other person. The hammerfist requires a true overhead wind-up; returning an uppercut to guard must not become a hammerfist.

### 6.2 Hit regions and attack outcomes

- Use explicit fighter-local hurtboxes and attack regions, transformed by position/facing.
- Separate pushboxes from hurtboxes. Decorative pipes, glow, trails, or shoulder exhaust are not automatically damageable.
- Hurtboxes change in authored ways for crouch, airborne poses, and selected attacks.
- Grounded crouch should avoid a correctly placed high straight, but not every attack.
- Overhead hammerfist regions must descend far enough to hit a crouching body; its overhead block category is meaningful.
- Uppercut geometry must actually cover useful rising/airborne space.
- A single-hit attack may damage a target only once per attack instance, even if overlap lasts several ticks.
- Multihit specials must declare a bounded hit count, minimum hit interval, and total damage budget.
- Evaluate simultaneous active attacks consistently from the same simulation snapshot. Do not let array iteration order give P1 an advantage.
- Health never becomes NaN or exceeds its maximum. Knockback cannot push a fighter outside stage bounds.
- Damage is applied on confirmed collision, not on gesture detection or animation start.

### 6.3 Defensive behavior

**Guard:** deliberately raise both hands near the face and hold briefly. Distinguish this face-level guard from the lower chest-level neutral/ready stance used for punching calibration; otherwise automatic approach would remain disabled whenever a player was merely ready to fight. In the game guard reduces or blocks eligible frontal hits while suppressing automatic approach. A successful normal block initially causes no health damage; specials may cause a small configured chip amount. Avoid invisible unblockable exceptions.

**Standing versus crouching guard:** the same guard input combined with crouch selects crouching guard. Hammerfist defeats crouching guard but not standing guard. Hook and knee are body strikes that require guarding rather than merely ducking. Jumping out of reach and uppercut anti-air are real geometry interactions.

**Guard stamina implementation default:** use a small, readable guard gauge to prevent holding guard forever from trivializing the demo. Start at 100; consume move-defined guard damage on block; regenerate only after leaving guard and a short delay. At zero, apply a brief guard-break stagger. Starting defaults may be 15–30 guard damage for normals, 35 for a special, 25 stamina per second recovery, and a 45-tick break. Keep all values configurable. The breaking hit is resolved consistently; do not apply both a full block and duplicate full damage.

**Crouch:** lower the authored body hurtbox and play a crouched pose. Crouch does not imply invulnerability or automatic blocking.

**Jump:** apply a simulated jump arc and landing recovery as appropriate. It is not a purely visual vertical offset. Establish a simple airborne action policy: jumping is available, guard is grounded, knee and hammerfist start grounded, and unsupported airborne attack commands are rejected/buffered rather than inventing extra moves. Broader aerial combos are deferred.

### 6.4 Hit feel

Confirmed hits produce a coordinated victim reaction, metal impact sound, hit spark, short hit-stop, bounded knockback, and readable health loss. Heavy normals and specials use stronger effects than jabs. Do not add camera shake to every movement or overwhelm the bodies with particles.

Use a trailing damage segment in health bars so a large special's effect is legible. Distinguish blocked hits visually and audibly. Keep hit-stop short enough that the game does not feel unresponsive; initial ranges might be 2–4 ticks for a normal and 5–8 for a heavy impact, subject to tuning.

## 7. Roster and ten special moves

### 7.1 Character design principles

- Five different silhouettes, not five palette swaps.
- Shared rig topology is allowed; geometry, proportions, stance, timing, and effects must carry identity.
- Visual bulk must agree with hurtboxes and movement. A huge robot cannot use an unexplained tiny body collider.
- Normals share input meanings across characters.
- Specials should be easy to explain and visibly different.
- Names below are original working names, not a guarantee of trademark clearance or an instruction to copy any existing character with a similar name.

### 7.2 Initial roster

| ID / working name | Identity and visual direction | Initial standard stats | Special 1 | Special 2 |
| --- | --- | --- | --- | --- |
| `foundry9` / Foundry-9 | Tank; broad chest, furnace core, piston arms, heavy feet | 130 HP; slow walk; strong damage; slower recovery | Seismic Drive: ground shockwave | Armored Ram: bounded armored forward charge |
| `relay7` / Relay-7 | Balanced; athletic industrial frame, clean armor, bright visor | 100 HP; baseline speed, damage, recovery | Optic Burst: horizontal eye beam | Rocket Straight: powered advancing punch |
| `needle2` / Needle-2 | Speedster; narrow torso, long light limbs, turbine accents | 80 HP; fast movement/recovery; lower individual damage | Slipstream Rush: fast visible dash strike with afterimages | Turbine Flurry: bounded rapid-punch sequence |
| `cinder4` / Cinder-4 | Artillery; asymmetrical launcher shoulder, reinforced core | 95 HP; deliberate movement; longer special recovery | Mortar Bloom: lobbed bomb with landing explosion | Core Lance: charged long beam |
| `coil5` / Coil-5 | Electrical brawler; exposed coils, compact strong arms, arc effects | 105 HP; close-range pressure; moderate speed | Arc Pulse: short-radius electrical burst | Magnetic Rise: electrified rising launcher |

Initial movement/damage modifiers can start around 0.8–1.25 times the baseline. Use a single content schema and avoid creating five unrelated fighter classes.

### 7.3 Special implementation rules

- **Ground shockwave:** moves along the floor, uses defined ground hit geometry, and can be evaded by a sufficiently high jump.
- **Armored charge:** movement is simulated and bounded; temporary armor is explicit damage reduction/interruption resistance, not unexplained invulnerability.
- **Eye/core beams:** use a clear charge tell, readable beam origin, finite active duration, and a defined collision segment/region. Bright visuals do not imply repeated unbounded damage.
- **Powered punch/dash:** stops at valid contact or its travel limit. Afterimages are cosmetic; they do not create extra fighters or hitboxes. No phasing through pushboxes without an explicit mechanic.
- **Flurry:** declare its limited number of hits and cumulative damage. No permanent stun loop.
- **Bomb:** has a visible arc, travel time, landing/explosion lifetime, and per-target explosion hit policy. Remove it on reset or scene exit.
- **Electrical pulse:** local radius and finite lifetime; no hidden full-stage hit.
- **Rising special:** uses the same airborne/landing system as other simulated motion and returns the robot to a legal grounded state.

A good marquee special has a readable charge, distinctive silhouette/pose, brief anticipation, decisive impact, and unmistakable health change. Do not extend unskippable cinematics so long that they consume the pitch.

### 7.4 Meter and activation

Implementation default: one shared 0–100 special meter per fighter; each special costs 50. Standard rounds start at zero; successful normals, blocked attacks, and taking damage can grant small configured amounts. Debit meter only when the move is accepted by combat, not when a tentative gesture is first observed.

Exhibition starts with a full meter, allowing immediate demonstration of either special. A new exhibition round refills it. No network call is needed to activate or render a special.

Two activation paths are required:

1. **Easy theatrical command:** raise both hands above the head, hold approximately 0.5 seconds to arm, then lower them within a configurable window to fire Special 1. Display charge progress and `LOWER HANDS TO FIRE`. Do not fire merely because one arm passes overhead during a hammerfist.
2. **Advanced sequence:** two distinct crouch-and-release events followed by a straight within an initial total window around 1.8 seconds activates Special 2. Holding one crouch does not count twice. The final punch is consumed by the special command, not also emitted as a straight.

Keyboard controls directly invoke either special. Exhibition may expose a visible choice of which special occupies the easy gesture slot; if used, show the selected special clearly. Standard default remains easy gesture -> Special 1, crouch sequence -> Special 2.

Once armed, the two-hand special state owns the relevant arm motion; lowering both hands must not generate two hammerfists. Interrupted charge, insufficient meter, tracking loss, and timeout need visible and deterministic outcomes. Never retain a charge across a round reset.

## 8. Computer vision and player identity

### 8.1 Local model behavior and limits

Start with MediaPipe Pose Landmarker configured for video and a maximum of two poses. It returns body landmarks, including inferred depth/world coordinates. Those depth estimates are not true depth-camera measurements and are not a sufficient punch detector on their own.

Multi-pose output order is not a player identity contract. Detection confidence, temporal continuity, and per-player assignment are separate concerns.

Pin model/package versions and bundle the model and required runtime files locally. A warm local installation should not need CDN access to play. Camera access requires an appropriate browser context: localhost for development or HTTPS for a hosted build.

### 8.2 Frame processing

- Request an appropriate webcam resolution; start around 1280 x 720 when available, but inspect actual negotiated dimensions.
- Run inference at a sustainable rate independent of rendering. Prefer approximately 20–30 pose updates per second; reduce the sampling rate or model cost if necessary instead of blocking the game.
- Permit at most one inference job in flight and a newest-frame replacement slot. Drop old frames; do not build a latency queue.
- Include capture and completion timestamps, sequence numbers, and camera-session generation in worker messages.
- Reject stale/out-of-order results and results from an old camera session.
- Release transferred image resources and terminate unused workers/tracks on actual teardown.
- Keep inference alive through ordinary rematches; do not reload the model every round.
- Verify worker, GPU/delegate, and browser compatibility rather than assuming that worker use alone guarantees acceleration.

### 8.3 Mirroring and coordinates

Use a mirrored preview because it is easier for people to align themselves. Maintain an explicit transformation between raw camera coordinates, mirrored UI coordinates, and anatomical left/right landmark indices.

Assign P1/P2 according to the visible marked zones. Apply mirroring exactly once. Camera handedness and the robot's facing direction are independent: a physical left-arm punch can trigger the appropriate attack regardless of which side of the arena the robot occupies.

### 8.4 Assignment and tracking

During setup, acquire two people in the two zones and lock their logical slots. Use torso/hip centers, body scale, velocity continuity, and zone plausibility to match subsequent detections. For two players, explicitly evaluate the possible matchings rather than trusting array order.

- A brief missing detection should not give that slot to the other person.
- Bystanders outside player zones should not become fighters.
- Crossing or ambiguity should cause a visible recovery state, not silent swaps.
- Store a calibration/assignment generation so old gesture history cannot leak into a new player.
- Provide an explicit swap operation when the operator wants it.
- If whole-frame multipose quality proves insufficient, two region-cropped trackers are a possible fallback to evaluate. They add inference cost and are not automatically the preferred solution.

### 8.5 Calibration as part of the product

The setup screen should guide the players through framing and a brief neutral/ready pose with hands around chest level, below the deliberate face-level guard. Record body scale, shoulder width, hip height, ready wrist positions, and confidence baselines. Use a couple of guided practice punches if needed for adapting extension thresholds; these are playable onboarding, not an engineering test assignment.

Target a new-player setup on the order of 5–8 seconds when both people are already framed, but do not fake a successful calibration to meet that target. Same-player rematches retain calibration. A new human requires new calibration; recognizing a body is not the same as calibrating that person.

Normalize gesture features by shoulder width/torso size and use time-based velocities. Do not hard-code thresholds in raw image pixels or number of frames at an assumed webcam FPS.

### 8.6 Tracking loss policy

Starting defaults, all configurable:

- Low-confidence required joints stop new gesture emission immediately for the affected recognizer.
- A short gap, roughly 150 ms, releases held inputs and disarms incomplete gestures rather than fabricating movement.
- Sustained loss or identity ambiguity, roughly 750 ms, pauses the match and identifies the affected player.
- Reacquisition requires stable assignment, reset gesture state, and a short ready period/countdown. Never unleash attacks accumulated during a gap.

One player's visibility problem must not relabel the other player. A new-player reset clears calibration explicitly; a brief same-player reacquisition may preserve it if identity is credible.

### 8.7 Honest precision and latency reporting

Target 60 FPS game rendering, roughly 20–30 pose updates per second when feasible, and a responsive control path around 100–200 ms from a sufficiently distinguishable gesture to visible reaction. These are goals, not established measurements.

Separately track capture/inference age, pose-processing cost, recognition decision time, input acceptance, and animation onset. Attack startup intentionally adds time before contact. Do not call model inference duration the entire input latency, and do not claim timing from physical gesture onset unless actual footage or instrumentation supports it.

A synthetic landmark test proves recognizer logic, not accuracy on two real people. The agent must report what it actually verified and what remains dependent on hardware/lighting/body variation.

## 9. Gesture recognizers

### 9.1 Shared recognition approach

Use a small vocabulary of intentional gestures with temporal state machines, not frame-by-frame posture labels or arbitrary unrestricted motion understanding.

Each recognizer should define relevant joints, minimum confidence, starting posture, normalized motion features, commitment threshold, release/rearm condition, and timeout. Use modest smoothing and separate enter/exit thresholds to avoid jitter without making attacks sluggish.

Illustrative lifecycle: ready -> candidate -> committed/emitted -> returning -> ready. Thresholds and timeouts live in configuration, not scattered constants.

### 9.2 Move-specific signals

| Action | Primary evidence | Important exclusions |
| --- | --- | --- |
| Straight | Forward arm extension pattern, wrist/elbow change relative to torso, supporting inferred depth, short-term velocity | Do not interpret every wrist movement as forward motion; reject strong hook/uppercut signatures |
| Hook | Predominantly horizontal wrist arc across the torso with an intentional compact strike | Ignore casual arm repositioning and the lateral part of an uppercut |
| Uppercut | Low/chest-level starting hand and upward striking trajectory | Do not emit a straight first; arm return is not hammerfist |
| Hammerfist | One-arm overhead wind-up followed by a committed downward trajectory | Exclude two-arm special charge/release and ordinary uppercut recovery |
| Knee | Unilateral knee rise relative to its hip, upright torso, supporting stance leg | Exclude both-leg jump motion, crouch, and a camera-frame shift |
| Crouch | Sustained relative hip descent and bent-knee configuration | Holding crouch creates one entry event, not a repeated combo |
| Jump | Whole-body upward trajectory relative to calibrated floor/body reference, supporting leg/foot evidence | A raised knee alone is not a jump; camera shake is not a jump |
| Guard | Both hands deliberately raised into a stable face-level guard region without strike evidence | Lower chest-level ready hands are not guard; an attacking arm leaving guard must not create sticky simultaneous block |
| Easy special | Both wrists confidently above the head, dwell, then intentional lowering | Require two arms and charge ownership; no accidental hammerfists |

### 9.3 Arbitration and fairness

- A physical strike yields one primary normal classification, not multiple overlapping commands.
- Apply explicit direction/start-pose evidence and confidence margins when selecting between candidates.
- Two hands moving together can be ordinary guard adjustment; do not always infer a special.
- A knee and an arm gesture may occur together, but the combat state still determines which legal action can begin.
- Require reset/retraction before rearming an arm. Holding extension cannot repeatedly attack.
- Separate recognition from legal move acceptance. Rejected actions must not mutate combat state or meter.
- Same rules and default thresholds apply to both players, adapted only to their calibration.
- New round, rebind, or pause invalidates candidate histories and combo buffers as appropriate.

### 9.4 Diagnostics useful to the agent

Provide a hidden/debug overlay with landmarks, logical player assignments, key confidences, recognizer state, accepted/rejected action, and recent timing. The normal camera inset should stay clean, with player-colored borders, readiness, and a brief recognized-action label.

Support replayable timestamped landmark fixtures for recognizer checks. Do not record or upload webcam footage by default. Any user-provided recording used for diagnosis requires their permission and should not be committed casually.

## 10. Robot artwork and animation production

### 10.1 Chosen visual technique

Use original 2D articulated metallic robot art with dimensional shading. This is sometimes called a 2.5D look, but the planned runtime renderer does not require 3D models or real-time 3D lighting.

The style should be industrial, detailed, readable, and deliberately cohesive: shaped armor, exposed joints, panel edges, bolts, restrained highlights, luminous cores, and strong silhouettes. It is not intended to be photorealistic film rendering.

### 10.2 Asset construction

Draw approximately 18–24 reusable parts per robot as editable SVG artwork: head, torso, pelvis, shoulder plates, upper/lower arms, fists, thighs, shins, feet, and selected accent/glow layers.

Rasterize at an appropriate high source resolution during asset preparation. Pack textures into an atlas when useful. Preserve named pivots and attachment metadata. Keep source art and reproducible generation steps in the repository; do not depend on a proprietary editor file with no export path.

A part is drawn once and reused across poses. Extra fist orientations, opened panels, or damaged parts can be replacement sprites. Do not paint or independently generate a full character image for every rendered frame.

### 10.3 Rig and animation

A documented hierarchy attaches torso and legs to the root/pelvis, head/arms to torso, and forearms/fists to their parent joints. Specify pivot positions and layer ordering, including front/back limbs.

Author key poses and time curves for idle, approach/walk, crouch, guard, jump/landing, five normals, hit reaction, special charge, each special, KO, and victory. Interpolate between poses at rendering time. Use held impact poses and purposeful anticipation rather than applying identical sine-wave motion to every part.

Shared base clips are acceptable, but adjust stance, limb reach, recovery, and accent animation to each robot. Keep root movement under combat simulation control. Facing flips must preserve appropriate limb order and effects origins.

The straight's example 5/3/10-tick timing corresponds to anticipation/extension, actual impact, and recovery. Visual contact and active collision windows must agree. Cosmetic squash/stretch or glow must not secretly change damage geometry.

### 10.4 Internal quality milestone

First finish one robot at gameplay size with idle, walk, straight, and hit reaction. Inspect it internally in motion before reproducing the pipeline for all five. This is an agent-owned milestone, not a requirement that the user review an art prototype before work can continue.

Avoid the paper-puppet look through well-chosen pivots, joint overlap, meaningful torso involvement, varied poses, feet that contact the ground, and carefully timed impact. Effects cannot conceal fundamentally weak anatomy or animation.

Image generation may help with concepts, portraits, or backgrounds if permitted and available. It is not the primary source of independently generated combat frames because temporal/design consistency is unreliable. No generative image API is needed at runtime.

### 10.5 Asset provenance

Maintain an asset manifest recording source, license/permission, attribution, and whether an asset is original, generated, or third-party. Do not claim third-party or generated work was hand-drawn. Do not download/rip assets from commercial fighting games.

## 11. Arenas, HUD, and presentation

### 11.1 Two finished stages

1. **Foundry Pit:** furnace glow, dark steel, gantries, distant industrial motion, floor markings, restrained sparks/smoke.
2. **Storm Deck:** elevated industrial rooftop, skyline depth, warning lights, rain/storm accents that do not obscure fighters.

Both use layered parallax backgrounds, a clear fighting floor, and enough contrast to distinguish all five robots. Stage geometry should initially be the same flat combat space; visual diversity does not require procedural generation or gameplay hazards.

A scrapyard stage is optional after the required two are finished. Do not spend the main effort on a map generator.

### 11.2 HUD

- Large, readable P1/P2 health bars with trailing damage indicators.
- Robot names/portraits, round markers, central timer, special meter, and discreet guard stamina.
- Clear charge/special availability and an unobtrusive special-gesture prompt in exhibition.
- Brief move labels or tutorial guidance without filling the arena with debug text.
- Camera inset with mirrored view, player-colored zones/borders, tracking readiness, and optional minimal skeleton overlay.
- Distinguish player identity through labels/icons as well as color.

The inset demonstrates the technical connection to spectators but must not dominate the fighting scene.

### 11.3 Effects and transitions

Use impact sparks, metal fragments, energy trails, bounded camera shake, charge glow, short special emphasis, and clear KO presentation. Add a reduced-shake/flash option. Keep menus and ready/result transitions fast and skippable where appropriate.

## 12. Modes, onboarding, and operator flow

### 12.1 Main flow

```text
Boot/preload -> title -> Standard / Exhibition / Tutorial
             -> robot selection -> arena selection -> input/camera setup
             -> calibration -> ready -> match -> results -> rematch
```

Remember last chosen robots, arena, volume, and reasonable local settings. Do not require an account. Menus use mouse/keyboard; gesture menu navigation is not required.

### 12.2 Standard mode

- First to two round wins, ordinarily described as best of three.
- Initial timer: 60 seconds per round.
- Character-specific stats and normal meter progression.
- Reset health/position/round-local state between rounds; preserve match score.
- On timeout, compare remaining health percentage so different maximum HP values are handled deliberately.
- Simultaneous KO or equal timeout result is a draw; replay the round without awarding a win. This may require more than three played rounds, which is preferable to arbitrary P1 advantage.
- Pause/charging rules must treat the timer consistently. Pausing stops it; ordinary attack startup does not.

### 12.3 Exhibition mode

- Explicitly labeled short, single-round demonstration preset.
- Automatic approach, near-range starts, full starting special meter, fast results/rematch.
- Initial timer: 45 seconds; operator can restart without waiting for it.
- Normalize maximum HP to 100 and disable standard damage multipliers by default so the showcase is predictable; preserve robot identity through speed, reach, animation, and special behavior.
- Basic straight uses 10 damage. Give the showcased Special 1 a total unblocked damage budget around 60, including all its ticks/hits. Four straights plus that special can therefore KO a full-health opponent if all connect.
- These are visible/shared mode rules, not secret guaranteed hits or a scripted winner. Blocking, missed attacks, and actual geometry still matter.
- Special 2 can have its own explicitly configured demonstration budget. Do not make every projectile tick apply the full budget.

### 12.4 Optional tutorial

Keep it brief and interactive. Teach straight, one alternate normal, guard, automatic approach, and the easy special, with optional prompts for the remaining moves. Allow skipping immediately. Show a small move reference for all five normals rather than requiring a long certification sequence.

Tutorial completion is not required for normal play. Camera setup still runs when necessary even if the tutorial is skipped.

### 12.5 Fast reset versus new players

**Quick rematch:** retain camera stream, worker, model, assets, player slots, and calibration. Reset fighters, health, meter, projectiles, timers, round state as appropriate, pending commands, recognizer latches, and special/combo history.

**New players:** pause, invalidate old assignments and calibration, reacquire the two marked zones, guide neutral/practice calibration, then return to ready. Keep the application/model loaded.

**Swap assignments:** explicit operator action, accompanied by correct label/calibration reassignment and cleared input histories. Never confuse swapping robot slots with swapping anatomical left/right landmarks.

**Exhibition reset:** restore the known demonstration preset without reloading the browser.

Target roughly three seconds from KO through a brief result and countdown to a same-player rematch, gated on tracking readiness. It is a target, not permission to begin while the judge is adjusting position.

### 12.6 Suggested three-minute presentation

| Time | Experience |
| --- | --- |
| 0:00–0:15 | Explain two people, one camera, body-controlled robot combat; show inset |
| 0:15–0:35 | Judge enters zone, quick guided calibration/practice |
| 0:35–1:10 | Several normal attacks, visibly different moves, clear health reduction |
| 1:10–1:40 | Teach easy special; charge, fire, large damage/KO |
| 1:40–1:50 | Fast rematch demonstrates repeatability |
| 1:50–2:35 | Another exchange or another robot's special |
| 2:35–3:00 | Explain local pose tracking, input translation, and close |

Before the pitch, preload assets/models, grant camera permission, unlock browser audio with a click, choose the exhibition preset, and verify framing. This is operator preparation, not a hidden prerecorded demonstration.

## 13. Audio

### 13.1 Combat sounds

Use a concise, consistent library: punch whoosh, light metal hit, heavy metal hit, guard clang, servo/footstep, jump/landing, special charge/release, KO, and menu confirmation.

Prefer vetted CC0 or properly licensed metallic impacts/foley. Simple lasers, charge tones, and UI effects can be synthesized originally. Record provenance. Decode local audio before play.

- Whoosh follows attack startup.
- Impact follows confirmed contact.
- Block clang follows a block event.
- Footsteps follow authored footfall markers, not every update.
- Limited pitch variation reduces repetition.
- Cap overlapping voices and avoid clipping.

### 13.2 Retro music

Ship at least one coherent instrumental retro/chiptune/industrial-electronic combat loop and a victory sting. A menu loop can be a variation or separate short track. Use properly licensed music or a simple original composition; no recognizable franchise themes.

Music is local and loop boundaries should be checked. More tracks are optional. Do not depend on a live music-generation service for the baseline experience.

### 13.3 Mixer and announcer

Separate music, SFX, and announcer volume. Provide mute and readable captions for important spoken instructions. Resume/unlock the browser audio context from an explicit interaction.

Briefly duck music under important voice lines and major impacts without letting the mix pump constantly. Announcer lines must not overlap the presenter or queue up long after events.

A local announcer implementation can play fixed ready, special-ready, and KO cues. Cartesia is the strongest proposed optional sponsor integration: cached fixed lines, personalized setup introductions, hands-free instructions, and sparse factual match reactions. Do not require an LLM merely to choose a known line from a game event.

If live TTS is used, generation is asynchronous and nonblocking. Expire stale lines, cache reusable ones, and fall back to local audio/captions. Use a minimal credential adapter rather than exposing or committing a long-lived key. No voice request changes game outcome or holds up a round transition.

## 14. Sponsor strategy and scope protection

Sponsor integration choices are not all approved commitments. The game comes first. Prefer one useful product integration and optionally one honest development-workflow entry.

| Candidate | Natural contribution | Status / limitation |
| --- | --- | --- |
| Cartesia | Responsive announcer, hands-free onboarding, personalized introductions | Preferred optional product integration; requires access and confirmation of challenge criteria |
| Figma | HUD, calibration flow, robot component design, design-to-code workflow | Good workflow fit if actually used; retain real artifacts and stage log |
| Gemini | Short factual post-fight line from game telemetry, optionally spoken by announcer | Optional extension, not required combat intelligence; do not oversell a trivial wrapper |
| Lyria via Gemini API | Optional generated/adaptive music | Experimental/network risk; local music remains baseline; verify prize eligibility |
| Mana / Factory | Genuine planning, implementation, or testing workflow contribution | Only if the required tool/platform is actually used and its current rules fit |
| Convex / Tiger Data | Tournament/telemetry features | Deferred unless specifically requested; not needed for local matches |
| Vultr / GoDaddy | Deployment/domain | Optional, subject to actual event requirements |

Do not add finance, blockchain, scraping, security, or agent orchestration features merely to mention a sponsor. Do not claim beginner or solo eligibility; the user has explicitly excluded those categories. General overall, creative, and hacker-choice categories are natural targets, not guaranteed outcomes.

Using MediaPipe does not constitute use of the Gemini API. Using a coding assistant does not automatically qualify for an SDK-specific or public-benefit prize. Do not reframe the game as medical rehabilitation without real work and evidence.

The supplied Figma brief currently differs in categories/amounts from the pasted prize list. Confirm applicable event rules before claiming eligibility or promising deliverables. It mentions a maximum-three-minute video, an honest stage log, Figma Slides, accessible Figma files, and education verification. Keep workflow records only for work actually performed.

## 15. Reliability, privacy, and performance

### 15.1 Reliability before infrastructure

The user prioritizes an impressive hackathon v1 over production hardening. Avoid overengineering authentication, deployment architecture, or persistence. Basic credential hygiene, asset permissions, camera consent, and avoiding unrelated repositories still apply.

Required failure handling includes denied/unavailable camera, unsupported worker/model setup, missing asset, tracking ambiguity, tab blur, disconnect, expired optional voice request, and repeated scene transitions. Provide a useful error and keyboard option rather than a blank screen. Keyboard fallback must be labeled honestly, not presented as successful camera control.

### 15.2 Keep the hot path local

Combat, pose estimation, action selection, essential sound, and rendering run locally. Optional network features may fail without breaking play. A warmed local installation should be able to play without venue internet. Initial dependency installation and any licensed asset/model download may require network access.

### 15.3 Performance discipline

Bound particles, projectiles, input buffers, audio voices, and inference work. Avoid per-frame asset generation and unbounded allocations. Release scene resources. Keep resolution/model/particle settings adjustable. Report measured performance on the actual environment, not guessed hardware capabilities.

### 15.4 Privacy

Do not record or upload webcam footage by default. Diagnostics should prefer derived landmarks and timing. Any optional recording/export must be explicit and clearly described. Do not commit API keys, private recordings, personal data, or credentials.

## 16. Agent-owned verification and delivery criteria

### 16.1 What the agent should verify internally

- Type checking, build, and relevant focused checks.
- Fixed-step behavior independent of render rate.
- Every normal attack's legal states, active frames, damage, block category, recovery, and single-hit policy.
- All ten specials, bounded multihits, costs, projectiles, landing, and cleanup.
- Both-player fairness, simultaneous hits/KO, timeout, round scoring, and match completion.
- Automatic approach with different character sizes, knockback, stage boundaries, and movement overrides.
- All input adapters reaching the same commands and not duplicating actions.
- Recognizer temporal fixtures, resets, confidence loss, candidate arbitration, combo consumption, and reordered detections.
- User flows: selection, tutorial, calibration, pause, new player, swap, repeated rematch, and return to exhibition.
- Visual collision/animation agreement, all five silhouettes, both arenas, audio sync, and no broken assets.
- Resource behavior across repeated resets and longer sessions.

These checks belong to the agent. Do not assign the user a twenty-punch experiment or repeated low-level validation before continuing implementation.

### 16.2 What cannot be inferred from synthetic checks

Actual two-person front-facing recognition on the user's camera depends on real framing, lighting, body shape, motion, and hardware. Do not claim it was verified unless actual observation or representative permitted footage supports that claim. A fixture replay is not a live camera test.

The user's first play session can reveal recognition tuning needs, but must not reveal that most of the game is still missing. Maintain a clear distinction between implemented functionality, agent-verified behavior, and hardware-dependent behavior awaiting real play feedback.

### 16.3 Completed v1 handoff

A handoff should include a runnable integrated game, predictable start/build commands, local model/assets, controls, all required content/modes, fast reset, visible camera readiness, concise known limitations, and a short demonstration guide. Report outstanding blockers honestly. Do not replace working content with mockups while describing it as complete.

The step plan and rules govern how this is built and committed. No game implementation begins until the user explicitly authorizes it.

## 17. Reference documentation

Use current official documentation when implementation reaches these integrations; API versions and sponsor rules may change.

- MediaPipe web Pose Landmarker: https://ai.google.dev/edge/mediapipe/solutions/vision/pose_landmarker/web_js
- MoveNet reference/fallback investigation: https://github.com/tensorflow/tfjs-models/blob/master/pose-detection/src/movenet/README.md
- Phaser documentation: https://docs.phaser.io/
- Cartesia documentation: https://docs.cartesia.ai/build-with-cartesia/capability-guides/tts
- Lyria RealTime: https://ai.google.dev/gemini-api/docs/realtime-music-generation
- Supplied Figma brief: https://docs.google.com/document/d/1HjzThDGveSLiilIdEPEHq12ZVLZ5eS0T8CQC5_m_-rg/edit
- Event prize page to verify: https://hive.hackberkeley.org/prizes

These references are research sources, not an instruction to use every service or to perform any work outside the authorized repository.
