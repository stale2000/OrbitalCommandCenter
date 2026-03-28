# Orbital Command Center Specification

Status: Draft v1

Purpose: Define a fully local, StarCraft-themed AI agent command center that manages multiple concurrent AI agent swarms from a single war-room interface, with explicit support for Codex.

## 1. Product Summary

Orbital Command Center is a local command-and-control system for running, observing, and directing multiple AI agent swarms at once. It is modeled after the structure, fantasy, and strategic language of StarCraft, with race doctrines, workers, units, buildings, strategy presets, resources, tech progression, scouting, and battlefield control used as real system concepts rather than decorative labels.

The system is centered on a single `Command Center` screen where High Command watches swarm activity, receives live messages and alerts, launches new operations, and intervenes in active missions. Swarms are run by local orchestration over one or more agent runners. Codex must be supported as a first-class runner, but the system should not be restricted to Codex alone. The local machine owns orchestration state, mission state, logs, and operator controls.

This specification intentionally does not depend on Linear or any issue-tracker polling model. Work enters the system through operator-created missions, local triggers, or future local integrations.

## 2. Goals and Non-Goals

### 2.1 Goals

- Provide one local war-room surface for multiple simultaneous swarms.
- Support multiple agent runners while guaranteeing strong Codex compatibility.
- Define the three races as distinct swarm doctrines with real behavioral differences.
- Express workers, units, buildings, and strategies as operational system concepts.
- Add real macro systems such as resources, supply, tech, upgrades, and expansions.
- Add real information-war systems such as fog of war, scouting, and detection.
- Define a clear mission lifecycle from launch through victory, failure, retreat, and replay.
- Define learning, persistence, and operator ergonomics for long-running use.
- Keep orchestration state, event streams, memory, and artifacts locally managed.
- Support real-time operator intervention, reinforcement, retasking, and escalation.
- Make strategy presets first-class launch patterns with meaningful tradeoffs.
- Preserve a strong StarCraft identity, including direct asset reuse where desired.

### 2.2 Non-Goals

- Reproducing StarCraft gameplay rules exactly.
- Integrating with Linear in this specification version.
- Requiring a cloud-hosted control plane.
- Designing for only one runner when shared swarm concepts can be expressed generically.
- Designing a complete tech tree for every race in the first version.
- Solving legal or distribution policy for copied assets beyond making the decision explicit.

## 3. Local-First Operating Model

### 3.1 Meaning of "Fully Local"

For this specification, "fully local" means:

- The command center application runs on the local machine.
- Swarm orchestration state is stored locally.
- Event logs, mission records, and artifacts are stored locally.
- Operator controls and intervention happen locally.
- Local services such as MCP servers, memory stores, planners, and artifact indexes are hosted or mounted locally.

This does not automatically imply fully offline model execution. Codex is a required supported runner, and an implementation may rely on Codex runtime services or other compatible runners. The spec therefore distinguishes:

- `Local control plane`: always required.
- `Offline model execution`: optional future capability, not required for v1.

### 3.2 Core Runtime Posture

- The orchestration layer should support more than one agent runner.
- Codex support is mandatory in v1.
- Worker and unit concepts should be modeled independently of any single runner.
- Runner-specific capability differences may influence doctrine recommendations, unit unlocks, or strategy availability.
- The implementation should optimize for clear runner integration boundaries rather than hard-coding the entire system around Codex semantics.

## 4. Strategic Systems

Orbital Command Center should model both `micro` and `macro`. The system should not only launch agents well; it should let High Command manage economy, unlocks, map knowledge, and strategic tempo.

### 4.1 Resources

The system should expose resources as first-class constraints:

- `Minerals`
  General execution capacity. Used for baseline workers, routine units, and common operations.
- `Vespene Gas`
  Premium capability budget. Used for high-context reasoning, advanced specialists, expensive tools, and rare unlocks.
- `Supply`
  Maximum concurrent swarm footprint. Represents active-agent concurrency, context burden, operator attention ceiling, and service saturation limits.
- `Energy`
  Short-burst tactical power for scans, interventions, recalls, and doctrine abilities.

Resources may be literal quotas, scheduling weights, token budgets, operator attention budgets, or local compute allocations depending on implementation.

### 4.2 Tech Tree and Upgrades

The system should distinguish new capability unlocks from broad capability improvements.

- `Tech`
  Unlocks new unit classes, new services, stronger memory modes, deeper autonomy, richer tool access, or additional runner features.
- `Upgrades`
  Improve existing units or systems across a class, such as better summarization, stronger review quality, safer tool use, faster retries, improved memory compression, or better coordination.

### 4.3 Fog of War, Scouting, and Detection

The system should treat unknowns as a first-class gameplay and product concept.

- `Fog of War`
  Unknown repo areas, hidden dependencies, unverified assumptions, unclear requirements, and uncertain blast radius.
- `Scouting`
  Low-cost intelligence gathering before committing a main swarm.
- `Detection`
  The ability to reveal hidden failure modes, stealthy regressions, contradictory outputs, or invisible blockers.

### 4.4 Expansions and Map Control

- `Expansion`
  A deliberate increase in execution capacity, memory depth, tool access, workspace coverage, or service footprint.
- `Map Control`
  The degree to which the system has visibility, influence, and low-friction access across code areas, services, repos, or mission domains.

### 4.5 Build Orders and Control Groups

- `Build Order`
  A recommended or preset opening sequence for structures, unlocks, units, and mission phases.
- `Control Group`
  A reusable commander binding for squads, swarms, services, or mission compositions that can be retasked quickly.

### 4.6 Defensive Structures and Replays

- `Defensive Structures`
  Guardrails such as approvals, branch protections, sandboxes, rollback points, and policy gates.
- `Replay`
  Full mission timeline playback for after-action review, debugging, training, and strategy comparison.

### 4.7 Hero Units, Alliances, and Campaign Progression

- `Hero Unit`
  A persistent high-value agent profile with trusted specialties, mission history, and stronger identity than disposable units.
- `Alliance`
  A formal cooperation model between swarms, including reinforcement, shared scouting, and handoff behavior.
- `Campaign Progression`
  Long-running accumulation of unlocked tech, learned preferences, persistent map control, and commander history across missions.

## 5. Command Fantasy and Operator Role

### 5.1 High Command

The user is `High Command`. High Command is not a passive observer. High Command is expected to:

- select race doctrine for a mission or swarm
- choose strategy presets
- allocate services and capacity
- reinforce, split, merge, or retask squads
- approve sensitive actions when policies require it
- respond to escalations and blocked missions
- review outputs and archive mission results

### 5.2 Product Fantasy

The experience should feel like a live war room rather than a backlog viewer. The command center is expected to communicate pressure, readiness, momentum, and battlefield clarity.

The UI should privilege:

- live mission chatter
- squad composition and movement
- production and unlock status
- building availability
- pressure and threat indicators
- commander interventions

### 5.3 Operator Ergonomics

The command center should remain usable under pressure. The default experience should prioritize:

- what is on fire right now
- which missions are blocked or winning
- what decisions only High Command can make
- which swarms are over-supplied, under-supplied, or starved
- which hidden threats remain unresolved

Advanced details such as full replay timelines, upgrade trees, and deep telemetry should be one layer down, not the default visual focus.

## 6. Race Doctrine Framework

All races must be defined on the same comparison axes so doctrine differences stay mechanically clear.

### 6.1 Comparison Axes

- `Spawn / reinforcement model`
- `Memory / context model`
- `Coordination style`
- `Failure tolerance`
- `Operator attention cost`
- `Best-fit mission categories`
- `Resource profile`
- `Scouting / information style`
- `Tech dependence`

### 6.2 Terran Doctrine

Terran represents structured, modular, tactical swarm operations.

- Spawn / reinforcement model:
  Incremental production with explicit queueing, role assignment, and reinforcement waves.
- Memory / context model:
  Compartmentalized context with clear logs, attachments, checkpoints, and service-backed recall.
- Coordination style:
  Squad-based, tool-driven, and operator-steerable. Terran units are reliable when given clean objectives and supporting infrastructure.
- Failure tolerance:
  Moderate to high. Individual failures are expected and recoverable through fallback tooling and replacement units.
- Operator attention cost:
  Medium to high. Terran rewards active command and tactical adjustments.
- Best-fit mission categories:
  Multi-step engineering work, infrastructure changes, debugging campaigns, review pipelines, and mixed human-agent operations.

Terran identity in system terms:

- favors modular services
- uses explicit unlocks and add-ons
- supports strong observability
- rewards deliberate operator control
- plays strong defensive macro with clean production structure

### 6.3 Zerg Doctrine

Zerg represents adaptive, high-pressure, replication-heavy swarm operations.

- Spawn / reinforcement model:
  Fast burst spawning, rapid specialization, and large waves of low-cost workers or units.
- Memory / context model:
  Lightweight inherited context with aggressive summarization and disposable task-local state.
- Coordination style:
  Pressure-based, parallel, opportunistic, and fast to redirect.
- Failure tolerance:
  Very high at the unit level. The swarm expects individual losses and wins through throughput and adaptation.
- Operator attention cost:
  Low to medium during execution, but high during doctrine selection because strategy choice drives risk sharply.
- Best-fit mission categories:
  Broad search, large fan-out triage, speculative implementation probes, rapid document mining, and time-sensitive pressure plays.

Zerg identity in system terms:

- favors replication over ceremony
- spends agents aggressively
- minimizes startup latency
- prefers momentum over precision
- thrives on map pressure, scouting, and disposable control

### 6.4 Protoss Doctrine

Protoss represents elite, high-context, precision swarm operations.

- Spawn / reinforcement model:
  Smaller number of expensive, higher-capability deployments with stronger prerequisites.
- Memory / context model:
  Deep inherited context, curated shared memory, and stronger continuity between missions.
- Coordination style:
  Intent-driven, synchronized, and high-value. Protoss units should feel deliberate rather than numerous.
- Failure tolerance:
  Moderate. Individual losses are expensive, but their planning depth reduces random churn.
- Operator attention cost:
  Medium. Protoss expects up-front planning and measured interventions rather than constant micro.
- Best-fit mission categories:
  Architecture work, high-stakes refactors, strategic planning, synthesis, and precision execution where context quality matters more than volume.

Protoss identity in system terms:

- favors quality over quantity
- uses stronger shared memory and planning
- invests in unlocks before deployment
- rewards carefully chosen strikes
- converts strong tech and positioning into decisive outcomes

## 7. Race Exemplars

The first version should define doctrine plus exemplars, not a complete exhaustive tech tree.

### 7.1 Workers

Workers are the baseline execution unit for each race.

- `SCV`
  Terran baseline worker. Reliable, tool-oriented, good at explicit instructions, setup tasks, and infrastructure-heavy work.
- `Drone`
  Zerg baseline worker. Cheap, numerous, fast to spawn, useful for fan-out, probing, indexing, and broad task pressure.
- `Probe`
  Protoss baseline worker. Fewer, stronger, context-rich, best suited to deliberate execution and careful handoffs.

### 7.2 Terran Units

- `Marine`
  General-purpose subagent for straightforward execution, patching, and mission reinforcement.
- `Medic`
  Recovery and support subagent focused on summarization, cleanup, retry prep, and damaged mission stabilization.
- `Ghost`
  Precision subagent for surgical edits, sensitive operations, targeted diagnosis, and high-trust tasks.
- `Science Vessel`
  Recon and observability subagent focused on logs, diagnostics, system state, and anomaly detection.
- `Siege Tank`
  Heavy execution subagent for long-form work that benefits from deliberate setup and sustained pressure.

### 7.3 Zerg Units

- `Zergling`
  Extremely cheap, fast probe subagent for quick scans, candidate generation, and pressure swarms.
- `Hydralisk`
  Mid-tier execution subagent for fast implementation or research tasks that need more substance than a Zergling.
- `Mutalisk`
  Fast response subagent optimized for hopping between loosely related objectives and resurfacing findings quickly.
- `Defiler`
  Disruption and strategy subagent for synthesis, swarm reshaping, and tactical redirection under pressure.
- `Queen`
  Support coordinator responsible for reinforcement, spawn amplification, and swarm sustainment.

### 7.4 Protoss Units

- `Zealot`
  Direct action subagent for contained execution tasks requiring discipline and momentum.
- `Dragoon`
  Durable ranged subagent for code comprehension, review, and longer technical engagements.
- `High Templar`
  Deep reasoning subagent for planning, synthesis, architecture, and high-context judgment.
- `Observer`
  Stealth recon subagent for inspection, discovery, dependency tracing, and non-intrusive monitoring.
- `Carrier`
  Capital-class coordinator for orchestrating multiple expensive specialists around a strategic objective.

## 8. Buildings and Services

Buildings are a mixed metaphor in v1. Some are durable backend services. Some are command abstractions that unlock capabilities or shape control.

### 8.1 Terran Buildings

- `Command Center`
  Primary control structure. Hosts mission launch, swarm overview, and command actions.
- `Barracks`
  Worker and basic unit production service.
- `Factory`
  Unlocks heavier execution roles and infrastructure-oriented units.
- `Starport`
  Unlocks remote inspection, oversight, and higher-mobility subagents.
- `Engineering Bay`
  Shared analysis, indexing, and capability upgrade service.
- `Comsat Station`
  Scan service for repo inspection, anomaly sweeps, and mission reconnaissance.

### 8.2 Zerg Buildings

- `Hatchery`
  Primary spawn and swarm staging service.
- `Spawning Pool`
  Early-aggression unlock for fast, low-cost unit flooding. This building is central to `6 Pool` style doctrines.
- `Hydralisk Den`
  Unlocks stronger mid-tier specialized executors.
- `Queens Nest`
  Reinforcement and sustainment coordination service.
- `Evolution Chamber`
  Shared mutation and capability upgrade layer.
- `Creep Network`
  Fast local context propagation and swarm routing fabric.

### 8.3 Protoss Buildings

- `Nexus`
  Primary command and energy structure for high-value deployments.
- `Gateway`
  Basic worker and core unit gateway into active missions.
- `Cybernetics Core`
  Unlocks advanced coordination, shared memory depth, and stronger context inheritance.
- `Robotics Facility`
  Produces recon, artifact, and precision support units.
- `Templar Archives`
  High-context planning, synthesis, and doctrine service.
- `Pylon Network`
  Power and connectivity layer that governs where advanced deployments can operate cleanly.

### 8.4 Generic Building-to-System Mapping

- memory stores
- artifact repositories
- local MCP connectors
- indexing and search services
- planning and summarization services
- capability unlocks
- routing and coordination hubs
- defensive policy gates
- replay and telemetry infrastructure

## 9. Strategy Presets

Strategies are first-class operational templates. They are not flavor-only labels.

Each strategy preset should define:

- intended use
- default race
- required buildings or unlocks
- preferred unit composition
- time-to-first-action profile
- risk profile
- operator burden
- expected outputs
- build order
- scouting expectations
- expansion expectations

### 9.1 Example Presets

- `6 Pool`
  Zerg early-pressure rush. Spawn many cheap agents immediately, accept weak context depth, and attempt rapid wins through speed and volume. Best for broad triage, shallow implementation probes, or finding fast paths under time pressure.
- `Stim Timing Push`
  Terran mid-speed tactical push using upgraded core units and explicit support services. Best for structured execution with fast follow-through.
- `Two-Base Macro`
  Terran or Protoss growth strategy focused on infrastructure first, then stronger sustained deployment. Best for long missions requiring resilience and scale.
- `Carrier Tech`
  Protoss slow-build, high-value strategy that delays broad action in exchange for superior late-stage specialists and strategic coordination.
- `Drop Harass`
  Terran mobility pattern for targeted strikes on isolated objectives, quick patch drops, or contained interventions.
- `Muta Contain`
  Zerg distributed disruption preset used to keep many sub-problems under pressure while the main swarm develops.
- `Templar Strike`
  Protoss precision preset for architecture, synthesis, or other high-context work where fewer strong agents outperform a flood.

## 10. Economy, Intel, and Control Model

These systems should be visible in both the UX and the domain model.

### 10.1 Economy Model

- workers gather or unlock usable execution capacity
- buildings consume resources and enable stronger future options
- units consume supply and mission budget
- expansions increase long-run throughput at the cost of setup time and defense burden

### 10.2 Information Model

- unexplored areas remain under fog of war until scouted
- reconnaissance can happen before or during a mission
- stronger detection reduces hidden regressions, false confidence, and silent failure

### 10.3 Control Model

- High Command may use control groups for fast retasking
- map control should be represented as confidence and reach across code or system territory
- chokepoints should be surfaced as bottlenecks such as fragile modules, approval gates, or scarce experts

### 10.4 Victory and Replay Model

- every mission should define a victory condition, not just activity
- replays should reconstruct events, decisions, unit composition, and major strategic pivots
- after-action review should compare intended strategy against actual outcome

## 11. Mission Lifecycle and Threat Model

### 11.1 Mission Lifecycle

Every mission should move through a visible lifecycle:

- `Planned`
  Mission exists but has not launched.
- `Scouting`
  Reconnaissance is active and fog of war is being reduced.
- `Mobilizing`
  Build order, buildings, workers, and initial units are being prepared.
- `Engaged`
  Main mission execution is underway.
- `Contested`
  The swarm is facing elevated resistance, uncertainty, churn, or conflicting outputs.
- `Reinforcing`
  The system is adding units, services, upgrades, or expansions to sustain the mission.
- `Retreating`
  The swarm is reducing exposure, preserving artifacts, and falling back to a safer posture.
- `Victory Pending`
  The objective appears complete and is awaiting confirmation, acceptance, or packaging.
- `Succeeded`
  Mission victory condition is met and accepted.
- `Failed`
  Mission ended without success and without an immediate recovery path.
- `Archived`
  Mission is complete and stored for replay, learning, and retrieval.

### 11.2 Retreat, Regroup, and Sacrifice Doctrine

The system should distinguish between productive losses and unrecoverable waste.

- `Retreat`
  Pull back active units, preserve artifacts, save context, and reduce spend when risk exceeds expected value.
- `Regroup`
  Pause direct pressure, re-scout, reallocate supply, or switch strategy before re-engaging.
- `Sacrifice`
  Spend low-value units intentionally to reveal threats, probe options, or buy time for a larger strategic goal.

Each race should express this differently:

- Terran favors orderly retreat and defensive regrouping.
- Zerg accepts sacrifice as a normal tactical tool.
- Protoss prefers expensive but deliberate retreat before elite losses compound.

### 11.3 Threat Taxonomy

The system should classify mission resistance into explicit threat types:

- `Ambiguity`
  Requirements are unclear or contradictory.
- `Scale`
  The target surface is too broad for current composition.
- `Complexity`
  Deep dependencies or architecture interactions require stronger reasoning.
- `Hidden Dependency`
  Critical blockers remain under fog of war.
- `Blast Radius`
  Potential damage from action is high.
- `Time Pressure`
  Delay reduces mission value sharply.
- `External Instability`
  Runners, tools, environments, or services are unreliable.
- `Coordination Breakdown`
  Swarm members disagree, overlap, or thrash.

Threat classification should drive scouting, reinforcement, retreat, and doctrine recommendations.

### 11.4 Autonomy Modes

The system should expose explicit commander control modes:

- `Manual`
  High Command approves most meaningful steps.
- `Assisted`
  The system acts on routine work but escalates on strategic pivots or risk.
- `Autonomous`
  The system follows doctrine and strategy presets with minimal interruption.
- `Aggressive`
  The system prioritizes speed and pressure, tolerating higher churn and risk.

## 12. StarCraft Translation Guide

This section exists so implementers know when a term is thematic and when it implies real behavior.

- `Race`
  A real orchestration doctrine with specific tradeoffs and defaults.
- `Worker`
  A baseline agent body for general-purpose execution.
- `Unit`
  A specialized agent role with a profile, cost, and deployment pattern.
- `Building`
  A durable service, unlock, or control structure that changes what swarms can do.
- `Strategy`
  A launchable operational preset that configures swarm composition and posture.
- `Expansion`
  A deliberate increase in local capacity, services, memory depth, or swarm throughput.
- `Rush`
  A high-speed, high-risk early action pattern.
- `Macro`
  A strategy prioritizing infrastructure, capacity, and long-run efficiency.
- `Tech`
  Investment in unlocks, deeper capabilities, or higher-value units before broad action.
- `Supply`
  The concurrency and attention budget available for active agents and swarms.
- `Upgrade`
  A broad improvement that strengthens an existing unit class, service layer, or doctrine capability.
- `Scouting`
  Low-cost information gathering before commitment.
- `Detection`
  The ability to reveal hidden failures, contradictions, or blocked paths.
- `Build Order`
  A recommended opening sequence for production, unlocks, and initial mission moves.
- `Control Group`
  A reusable operator grouping for fast swarm or squad commands.
- `Replay`
  A reconstructed mission timeline used for review and learning.
- `Retreat`
  Controlled reduction of exposure while preserving state and artifacts.
- `Hero Unit`
  A persistent high-value agent identity with specialized trust and history.
- `Alliance`
  A formal cooperation relationship between swarms.

## 13. Command Center Surface

The `Command Center` screen is the single source of truth for active operations.

### 13.1 Always-On Panels

- `Global Message Feed`
  Streams notable agent messages, mission updates, summaries, and alerts from all swarms.
- `Active Swarms`
  Shows each swarm's race, mission, composition, status, pressure level, and current objective.
- `Intervention Queue`
  Lists pauses, approvals, blocked tasks, escalation prompts, and operator decisions waiting to be made.
- `Threat Board`
  Shows active threat classifications, hidden blockers, retreat recommendations, and contested fronts.
- `Building / Service Status`
  Shows enabled infrastructure, unlocks, degraded services, and current dependencies.
- `Economy and Supply`
  Shows available minerals, gas-equivalent premium budget, supply ceiling, and current allocation pressure.
- `Intel / Map Control`
  Shows scouted areas, unresolved fog-of-war zones, chokepoints, and detection coverage.
- `Mission Status`
  Displays objective health, artifacts produced, recent milestones, and end-state readiness.
- `Runtime Health`
  Displays local process health, runner health, event throughput, token usage, and storage status.
- `Commander Outlook`
  Highlights only the top decisions requiring attention now, to preserve operator clarity under load.

### 13.2 Race-Specific Overlays

- doctrine summary and tradeoffs
- unit composition and production controls
- unlocked buildings and upgrade path
- strategy recommendations
- race-specific alert styling and battle chatter
- race-specific economy and scouting cues
- hero-unit status and doctrine-specific retreat cues

### 13.3 Global Actions

- spawn swarm
- reinforce swarm
- split squad
- merge squad
- retask mission
- pause or resume
- escalate to High Command
- archive outputs
- terminate swarm
- assign control group
- trigger scout
- expand capacity
- start upgrade
- review replay
- order retreat
- change autonomy mode
- assign alliance
- promote hero unit

## 14. Core Domain Model

### 14.1 Mission

A mission is the objective package assigned to a swarm.

Fields:

- `id`
- `title`
- `brief`
- `desired_outcome`
- `constraints`
- `priority`
- `victory_condition`
- `lifecycle_state`
- `threat_profile`
- `race_preference`
- `strategy_preset`
- `requested_artifacts`
- `status`

### 14.2 Race Doctrine

Defines race-level behavior.

Fields:

- `race_id`
- `name`
- `spawn_model`
- `memory_model`
- `coordination_style`
- `failure_tolerance`
- `operator_attention_cost`
- `resource_profile`
- `scouting_style`
- `tech_dependence`
- `preferred_mission_types`
- `available_workers`
- `available_units`
- `available_buildings`
- `recommended_strategies`

### 14.3 Worker Agent

Baseline runner-backed executor.

Fields:

- `agent_id`
- `worker_type`
- `race_id`
- `current_assignment`
- `context_budget`
- `supply_cost`
- `status`

### 14.4 Unit Subagent

Specialized runner-backed subagent or agent role.

Fields:

- `unit_id`
- `unit_type`
- `race_id`
- `mission_role`
- `deployment_cost`
- `supply_cost`
- `required_buildings`
- `required_upgrades`
- `tool_profile`
- `status`

### 14.5 Building / Service

Durable capability provider or unlock structure.

Fields:

- `building_id`
- `building_type`
- `race_id`
- `service_kind`
- `status`
- `capabilities`
- `dependencies`
- `upgrade_paths`

### 14.6 Strategy Preset

Launchable swarm pattern.

Fields:

- `strategy_id`
- `name`
- `race_id`
- `required_buildings`
- `default_composition`
- `risk_profile`
- `operator_burden`
- `time_to_first_action`
- `build_order`
- `scouting_plan`
- `expansion_plan`
- `intended_use`

### 14.7 Swarm

A live operational grouping.

Fields:

- `swarm_id`
- `mission_id`
- `race_id`
- `strategy_id`
- `commander_state`
- `autonomy_mode`
- `resource_state`
- `supply_state`
- `map_control_state`
- `allied_swarms`
- `workers`
- `units`
- `hero_units`
- `active_buildings`
- `control_groups`
- `status`
- `alerts`
- `artifacts`

### 14.8 Hero Unit

Persistent elite agent profile.

Fields:

- `hero_id`
- `name`
- `race_id`
- `runner_profile`
- `specialties`
- `trust_level`
- `mission_history`
- `status`

### 14.9 Upgrade

Broad improvement applied to units, structures, or doctrine.

Fields:

- `upgrade_id`
- `name`
- `race_id`
- `target_scope`
- `cost`
- `prerequisites`
- `effect_summary`
- `status`

### 14.10 Recon Report

Structured scouting output.

Fields:

- `report_id`
- `mission_id`
- `swarm_id`
- `source_unit_id`
- `scouted_area`
- `findings`
- `confidence`
- `recommended_actions`

### 14.11 Command Center Event

Normalized event for the global feed.

Fields:

- `event_id`
- `timestamp`
- `swarm_id`
- `source_kind`
- `source_id`
- `severity`
- `event_type`
- `summary`
- `details`
- `requires_operator_action`

### 14.12 Operator Intervention

Explicit commander decision point.

Fields:

- `intervention_id`
- `swarm_id`
- `reason`
- `options`
- `deadline`
- `selected_action`
- `resolution_status`

### 14.13 Replay Record

Persisted mission timeline for after-action review.

Fields:

- `replay_id`
- `mission_id`
- `swarm_id`
- `timeline`
- `strategy_summary`
- `key_turning_points`
- `result`

### 14.14 Campaign Record

Persistent cross-mission strategic state.

Fields:

- `campaign_id`
- `unlocked_tech`
- `completed_missions`
- `persistent_map_control`
- `doctrine_preferences`
- `hero_roster`
- `learned_build_orders`

### 14.15 Persistence Layout

Local storage model for durable state.

Fields:

- `missions_store`
- `replays_store`
- `artifacts_store`
- `telemetry_log`
- `memory_index`
- `campaign_store`

## 15. Runtime Architecture

### 15.1 Main Components

1. `Command Center UI`
   - Presents the war-room surface.
   - Renders events, swarms, buildings, and interventions.
   - Dispatches operator commands.

2. `Swarm Orchestrator`
   - Owns active swarm state.
   - Launches and reinforces missions.
   - Applies race doctrine and strategy presets.
   - Advances mission lifecycle states and handles retreat/regroup behavior.

3. `Agent Runner Layer`
   - Integrates one or more agent runners.
   - Must support Codex in v1.
   - Maps worker and unit definitions to runnable sessions.
   - Streams runtime events back to the command center.

4. `Doctrine Engine`
   - Encodes race differences.
   - Applies spawn, memory, and reinforcement rules.
   - Generates recommendations and strategy constraints.
   - Computes macro tradeoffs such as tech timing, scouting posture, and expansion pressure.

5. `Building / Service Layer`
   - Exposes local memory, planning, search, artifact, and MCP capabilities.
   - Controls capability unlocks and service health.
   - Hosts upgrade paths, defenses, and replay support services.

6. `Artifact and Memory Store`
   - Persists mission outputs, summaries, checkpoints, and local context.
   - Persists recon reports and replay records.
   - Persists campaign data and hero-unit history.

7. `Event Bus`
   - Normalizes all swarm and system activity into one command-center feed.

8. `Policy and Intervention Layer`
   - Decides when the system may proceed automatically versus when High Command must choose.

9. `Intel and Map Layer`
   - Tracks scouted territory, unresolved fog of war, chokepoints, and confidence by domain.

10. `Persistence Layer`
   - Stores missions, artifacts, replays, campaign state, and telemetry locally.
   - Recovers local state after restarts.

11. `Learning Layer`
   - Updates strategy recommendations from outcomes, replays, and commander choices.
   - Learns preferred build orders, effective counters, and doctrine fit over time.

### 15.2 Core Flow

1. High Command creates or selects a mission.
2. The system recommends race doctrines and strategy presets.
3. High Command may scout first to reduce fog of war.
4. High Command launches a swarm.
5. The orchestrator provisions required buildings and services.
6. The selected runner or runners spawn workers and unit subagents.
7. Build orders, upgrades, and expansions shape how the swarm develops.
8. Events stream into the global message feed.
9. The doctrine engine adjusts reinforcement, scouting, and recommendations during execution.
10. High Command intervenes when needed.
11. The swarm may retreat, regroup, or re-engage based on threats and commander policy.
12. The swarm produces artifacts, summaries, recon reports, and mission outcomes.
13. The command center stores a replay, updates campaign records, and archives the mission locally.

## 16. Intervention Contract

The system should request High Command input when:

- a mission is blocked on ambiguity
- a strategy should be changed mid-run
- a sensitive action requires approval
- a core building or service is degraded
- supply or resource pressure requires tradeoffs
- a chokepoint or hidden blocker is detected
- a mission should retreat, regroup, or be sacrificed
- autonomy mode should be raised or lowered
- the swarm wants reinforcement, retreat, merge, or split authorization
- outputs are ready for acceptance or archival

The system may act without intervention when:

- low-risk retries are needed
- disposable units are lost in expected ways
- a strategy preset explicitly authorizes aggressive autonomous behavior
- summaries, checkpoints, or local indexing work are routine
- scheduled scouting or replay capture is routine
- campaign-level learning updates are routine

## 17. Asset Policy

StarCraft assets are allowed in this project.

The spec should assume:

- copied visual, audio, and UI assets may be used directly
- the first implementation may use those assets in the local command-center experience
- packaging and distribution posture must be declared by the implementation

Recommended implementation modes:

- `Local / private mode`
  Asset reuse is unrestricted by the product spec and may fully embrace copied assets.
- `Shared / distributed mode`
  Asset packaging policy must be made explicit before release so implementers can decide whether to ship copied assets, swap to substitutes, or gate distribution.

This specification does not require one distribution policy, but it does require the implementation to avoid leaving the decision implicit.

## 18. Validation Scenarios

### 18.1 Terran Tactical Operation

- High Command launches a Terran swarm for a multi-step engineering mission.
- `Barracks`, `Engineering Bay`, and `Comsat Station` are active.
- `Marine`, `Medic`, and `Science Vessel` units collaborate.
- The operator reroutes one squad after a diagnostic alert.
- The command center shows modular service use, tactical reinforcement, and orderly recovery.

### 18.2 Zerg 6 Pool Rush

- High Command launches a `6 Pool` strategy against a time-sensitive mission.
- `Hatchery` and `Spawning Pool` unlock immediate low-cost pressure.
- A large number of `Drone` and `Zergling` bodies fan out rapidly.
- The swarm accepts high churn and shallow context in exchange for fast results.
- The command center makes the risk and speed tradeoff obvious.

### 18.3 Protoss Precision Strike

- High Command launches a Protoss swarm for a high-context architecture task.
- `Nexus`, `Cybernetics Core`, and `Templar Archives` are active.
- `Probe`, `Observer`, and `High Templar` units perform deep context work.
- Fewer agents run, but each has stronger continuity and reasoning depth.
- The command center reflects a slower start with higher precision.

### 18.4 Multi-Swarm War Room

- Terran, Zerg, and Protoss swarms run concurrently.
- The global feed remains readable and source-attributed.
- High Command can distinguish which doctrine is speaking and acting.
- Global actions and race overlays coexist without ambiguity.

### 18.5 Economy and Supply Validation

- The command center shows resource consumption, premium budget pressure, and supply saturation.
- A strategy with stronger units or faster expansion creates visible tradeoffs.
- High Command can see when macro decisions, not just micro decisions, are limiting outcomes.

### 18.6 Fog of War and Scouting Validation

- A mission starts with partially unknown territory.
- Recon units or scans reduce uncertainty before heavy commitment.
- Detection reveals a hidden blocker or silent regression that would otherwise be missed.

### 18.7 Building and Service Validation

- Local memory, MCP infrastructure, planning services, and artifact stores are represented as buildings or unlock structures.
- Service degradation is visible in the building panel.
- Swarm capabilities change when buildings come online or fail.

### 18.8 Expansion and Upgrade Validation

- The system can expand local capacity or unlock stronger capabilities mid-campaign.
- Upgrades improve unit classes or service layers without requiring a full strategy reset.
- The command center makes tech-vs-pressure tradeoffs legible.

### 18.9 Intervention Validation

- The system pauses for approval on a sensitive action.
- High Command can reinforce, retask, split, merge, or terminate the swarm.
- The intervention queue records both pending and resolved decisions.

### 18.10 Replay and After-Action Validation

- Completed missions can be replayed as a timeline.
- High Command can inspect turning points, recon findings, and intervention history.
- Replay data supports strategy comparison and operator learning.

### 18.11 Mission Lifecycle and Retreat Validation

- A mission moves visibly through planning, scouting, engagement, contest, retreat or victory states.
- The system can recommend regrouping instead of blind continued pressure.
- Retreat preserves key artifacts and context for future re-engagement.

### 18.12 Hero Unit and Campaign Validation

- Persistent hero agents retain specialties and mission history across runs.
- Campaign records accumulate unlocks, learned build orders, and doctrine preferences.
- Long-running use changes recommendations without erasing operator control.

### 18.13 Local Runtime Validation

- The system operates without Linear.
- Mission state, swarm state, logs, and artifacts remain local.
- A temporary runner interruption, including Codex interruption, does not erase local mission history.

### 18.14 Asset Validation

- The UI can render copied StarCraft assets in local mode.
- The implementation clearly labels whether the current build is local/private or shared/distributed.
- Asset handling is explicit rather than implicit.

## 19. Definition of Done for This Spec

This specification is complete when it provides:

- a top-down product definition centered on the command-center fantasy
- mechanically distinct Terran, Zerg, and Protoss doctrines
- concrete worker, unit, building, and strategy exemplars
- real macro systems for resources, supply, tech, upgrades, and expansions
- real intel systems for fog of war, scouting, and detection
- a full mission lifecycle with threat and retreat logic
- a clear war-room UI contract
- a local runtime architecture with mandatory Codex support and room for additional runners
- persistent hero, campaign, and learning concepts
- explicit intervention, event, and domain model contracts
- a translation guide for StarCraft terminology
- replay and after-action review support
- an explicit copied-asset policy
- validation scenarios that prove the model is coherent
