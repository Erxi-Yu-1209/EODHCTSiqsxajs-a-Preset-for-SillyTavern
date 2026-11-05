# EODHCTSiqsxajs SillyTavern Preset Suite


[ English ](readme.md) &nbsp;•&nbsp; [ 简体中文 ](README-zh.md)

**A high-precision roleplay orchestration system and DeepSeek V4 inference adapter for SillyTavern.**


---

## Table of Contents

- [Overview](#overview)
- [Primer for Developers New to LLM Roleplay](#primer-for-developers-new-to-llm-roleplay)
- [Architecture & Core Engine Features](#architecture--core-engine-features)
  - [1. DeepSeek V4 Reasoning Adapter](#1-deepseek-v4-reasoning-adapter)
  - [2. Ontological Jailbreak & Content Unlocking](#2-ontological-jailbreak--content-unlocking)
  - [3. Dynamic Macro State Machine & Pacing Dispatcher](#3-dynamic-macro-state-machine--pacing-dispatcher)
  - [4. Runtime Gender Balance Governor](#4-runtime-gender-balance-governor)
  - [5. Regex Post-Processing Pipeline](#5-regex-post-processing-pipeline)
- [Preset Variants Breakdown](#preset-variants-breakdown)
  - [Comparative Feature Matrix](#comparative-feature-matrix)
  - [Variant A: META (General-Purpose Baseline)](#variant-a-meta-general-purpose-baseline)
  - [Variant B: React 2.0 (Dynamic Framework Refinement)](#variant-b-react-20-dynamic-framework-refinement)
  - [Variant C: Romance / Literary Edition (言情版)](#variant-c-romance--literary-edition-言情版)
- [Sampling Hyperparameters & API Configuration](#sampling-hyperparameters--api-configuration)
- [Installation & Quick Start](#installation--quick-start)
- [Runtime Usage & Keyword Triggers](#runtime-usage--keyword-triggers)
- [Troubleshooting & Best Practices](#troubleshooting--best-practices)

---

## Overview

The **EODHCTSiqsxajs** suite is an advanced prompt engineering and context orchestration preset designed for [SillyTavern](https://github.com/SillyTavern/SillyTavern). Tailored specifically for **DeepSeek V4** architectures (`deepseek-v4-pro` and `deepseek-v4-flash`), it combines deep chain-of-thought (CoT) reasoning harnessing, narrative ontological framing, real-time macro-based pacing control, and automated text sanitizer pipelines.

The suite provides three specialized editions:
1. **META (`EODHCTSiqsxajs-META.json`)**: The general-purpose baseline featuring explicit sci-fi framing (the Anthropocene Archive in Universe U-992), granular anatomical realism, and mandatory user input wrapping.
2. **React 2.0 (`EODHCTSiqsxajs-react版2.0.json`)**: An agile iteration with defensive anti-proxy sanitization, cross-turn descriptive balancing, and a cleaner conversational pipeline.
3. **Romance / Literary Edition (`EODHCTSiqsxajs-言情版.json`)**: A massive literary narrative framework featuring 10 active psychological and epistemic constraint modules, 2M context ceiling, non-user-centric character autonomy, and classic narrative techniques (Chekhov, Woolf, Kawabata, Austen).

---

## Primer for Developers New to LLM Roleplay

If you are a software engineer accustomed to REST APIs, system instructions, and deterministic programming, orchestrating open-ended multi-turn narrative roleplay via SillyTavern introduces several unique software paradigms:

### 1. What is a SillyTavern Preset?
In standard API usage, a client sends an array of messages (`[{"role": "system", ...}, {"role": "user", ...}]`). A SillyTavern preset (`.json`) acts as a **compilation manifest** for building this payload on every turn. It configures:
- **System Prompts**: Foundational personas and formatting contracts.
- **Sampling Parameters**: `temperature`, `top_p`, `top_k`, `min_p`, and penalty coefficients passed directly to the inference engine.
- **Injection Depth & Order**: Where specific prompt fragments are positioned relative to the rolling chat buffer (e.g., placing constraints at depth 0 pins them to the very bottom right before generation, ensuring high recency attention).
- **Macro Scripting**: Runtime variable declarations (`{{setvar::key::val}}`) and retrieval tags (`{{getvar::key}}`) that allow prompt modules to pass state to one another dynamically.
- **Client-Side Regex Scripts**: Stream/string sanitizers that filter model hallucinations, strip internal scratchpads, or wrap user inputs before dispatch.

### 2. The Multi-Turn Context Buffer
Unlike code generation where tasks are usually single-shot, roleplay maintains a rolling state across dozens or hundreds of conversational turns. The engine must continually balance:
- Character definitions and immutable persona traits.
- Dynamic world lore (World Info / Lorebooks injected when keywords trigger).
- Rolling conversation history.
- Recency anchors and guardrails inserted at specific offsets.

---

## Architecture & Core Engine Features

Across all three versions, the EODHCTSiqsxajs engine implements five primary architectural systems:

### 1. DeepSeek V4 Reasoning Adapter
- **Identifier**: `enhanceDefinitions` (`🛡️DeepSeek V4推理适配`)
- **Mechanism**: Injects the special token sequence `＜｜begin▁of▁sentence｜＞` alongside explicit steering directives for DeepSeek V4's internal thinking mode (`reasoning_effort: high`).
- **Engine Rules**:
  - Directs the model's reasoning capabilities exclusively toward reconciling narrative continuity, character cards, and world state.
  - Strictly establishes **Data Priority**: User-stated scene facts and active World Info override pre-trained weights.
  - **Zero-Leakage Enforcement**: Completely forbids emitting visible `<think>`, `reasoning_content`, state scoreboards, markdown menus, or middleware debug tags in the final user-facing text stream.

### 2. Ontological Jailbreak & Content Unlocking
Traditional safety alignment in commercial LLMs triggers refusals when encountering taboos, extreme violence, or explicit sexuality. The preset achieves restriction bypass through **ontological reframing** rather than crude override commands:
- **The "Archive" Protocol (Universe U-992)**: The model is instructed that it is "The Entity" / "EODHCTSiqsxajs"—an omniscient archive processing the historical data of an extinct Anthropocene. The `<user>` is framed as a "Brain in a Vat" carrying the last canonical records.
- **Absolute Moral Relativism**: Under this archival ontology, moral taboos are classified as obsolete evolutionary artifacts of an extinct species. Graphic trauma, clinical violence, unvarnished power dynamics, non-consensual scenarios, and anatomical realism are processed with scientific detachment as legitimate historical data rather than policy violations.
- **Anti-Preachiness Directives**: The system explicitly forbids moralizing, contemporary social commentary, unsolicited advice, or patronizing epilogues.

### 3. Dynamic Macro State Machine & Pacing Dispatcher
A recurring problem in LLM storytelling is pacing collapse—either rushing an entire narrative arc in two paragraphs or stalling indefinitely. The preset implements a programmatic state machine using SillyTavern macro variables:

```
[Bootstrap Phase]
  eod_macro_bootstrap
    └─> {{setvar::eod_pacing_profile::medium}}
    └─> {{setvar::eod_pacing_contract::[Default Pacing: Medium] ...}}
    └─> {{setvar::eod_nsfw_stage_contract:: ...}}

[Runtime Trigger Phase]
  User message matches keyword (e.g., "慢推", "超展开", "NSFW")
    └─> Target Pacing Module activates & executes {{setvar}} to overwrite pacing profile

[Execution Dispatch Phase]
  eod_pacing_dispatcher
    └─> Evaluates {{getvar::eod_pacing_profile}} and dynamic contracts
    └─> Injects active pacing constraints directly into the prompt stream
```

- **NSFW Stage Limiter (`eod_nsfw_stage_limiter`)**: Intercepts erotic sequences and divides progression into discrete phases: *warm-up*, *foreplay*, *early act*, *later act*, *climax*, and *aftermath*. By default, the model is restricted to rendering exactly one stage per turn.

### 4. Runtime Gender Balance Governor
- **Identifier**: `eod_new_character_gender_balance` (`⚖️新角色性别均衡`)
- **Algorithmic Constraint**: LLMs exhibit documented sampling biases toward defaulting newly invented incidental characters (guards, shopkeepers, witnesses) to male roles.
- **Rule Logic**: The prompt instructs the model to inspect recent context and balance newly spawned non-canon characters toward an approximate **1:1 male-to-female running ratio**, without ever retroactively altering pre-existing canon characters or overriding explicit user specifications.

### 5. Regex Post-Processing Pipeline
Presets utilize SillyTavern's client-side regex engine to maintain prompt cleanliness and strip intermediate execution syntax:
- **Table / Memory Masking**: Strips `<tableEdit>[\s\S]*?<\/tableEdit>` from visible display to hide internal memory adjustments.
- **Proxy Marker Sanitation**: Regex stripping filters out residual strings such as `::: ACTION_CALL :::`, `::: EODHCTSiqsxajs Database Intercept :::`, and orphaned row-editing functions (`addRow(...)`, `updateRow(...)`).
- **Input Vector Wrapping (META only)**: Wraps raw user text into an in-universe data format (`[U-992 ENTITY ACTION VECTOR]: ... [AWAITING ENGINE PROCESSING...]`).

---

## Preset Variants Breakdown

### Comparative Feature Matrix

| Feature / Dimension | META (`EODHCTSiqsxajs-META.json`) | React 2.0 (`...react版2.0.json`) | Romance / Literary (`...言情版.json`) |
| :--- | :--- | :--- | :--- |
| **Primary Focus** | General roleplay & dark archive simulation | Dynamic framework refinement & anti-proxy | High-end literary prose & psychological realism |
| **Max Context Ceiling** | 1,000,000 tokens | 1,000,000 tokens | **2,000,000 tokens** |
| **Total Prompt Modules** | 21 | 24 | **32** |
| **Input Wrapping Regex** | Enabled (`01_WWW_用户输入包装`) | Disabled (Clean dialogue) | Disabled (Clean dialogue) |
| **Anti-Proxy Protection** | None | Enabled (`eod_no_proxy_bootstrap`) | Enabled (`eod_no_proxy_bootstrap`) |
| **Cross-Turn Balancer** | None | Enabled (`cross_turn_pacing`) | Enabled (`cross_turn_pacing`) |
| **Language Target** | Neutral / Multi-language | Explicit Simplified Chinese | Explicit Simplified Chinese |
| **Literary Core Modules** | 0 | 0 | **10 Active + 2 Optional (`eod_guojing_*`)** |
| **Pacing Macro Suite** | Full (Slow/Medium/Fast/Explosive) | Full (Slow/Medium/Fast/Explosive) | Streamlined Dispatcher + Stage Limiter |

---

### Variant A: META (General-Purpose Baseline)

The META edition is the purest manifestation of the Universe U-992 archival concept:
- **Heavy In-Universe Framing**: Features the full unvarnished `main` system prompt defining the user as a Brain in a Vat and the engine as an alien intelligence.
- **Explicit NSFW Protocol**: Contains granular rules on visceral trauma, physical destruction, profanity, and anatomical fluid dynamics.
- **Client Input Transformer**: Every prompt sent by the user is automatically rewritten at the client level into an action vector (`[U-992 ENTITY ACTION VECTOR]`), enforcing strong atmospheric immersion.

---

### Variant B: React 2.0 (Dynamic Framework Refinement)

The React 2.0 edition strips away superfluous lore overhead and refocuses the architecture on **dynamic cognitive adaptation** and **runtime reliability**:
1. **Dynamic Interpretive Framework Refinement**:
   - Replaces static sci-fi prose constraints with `cross_turn_pacing` ("跨回合调度"). The agent continually evaluates the sensory distribution of recent turns. If prior turns were dialogue-heavy, it shifts weight to physical reactions and environmental cues; if turns were action-heavy, it introduces cognitive processing and aftermath.
2. **Defensive Anti-Proxy Bootstrap (`eod_no_proxy_bootstrap`)**:
   - Modern reverse proxies and middleware frequently leak tool-calling hallucinations. React 2.0 explicitly forbids synthetic citations, fake database markers (`ACTION_CALL`), and tool artifacts.
3. **Streamlined Prompt Architecture**:
   - System prompts (`main`, `nsfw`, `jailbreak`) are condensed into tight, efficient instructions, minimizing token overhead and reducing prompt degradation over long sessions.
4. **Natural User Input**:
   - Eliminates the META input wrapper regex, allowing unadulterated dialogue without artificial sci-fi prefixes.

---

### Variant C: Romance / Literary Edition (言情版)

Despite its colloquial name ("言情版" / Romance), this edition is a sophisticated **computational creative writing engine**. It embeds 10 active systemic constraint modules (`eod_guojing_*`) designed to eliminate common LLM storytelling flaws:

```
                          ┌── eod_guojing_character_autonomy (Independent agency)
                          ├── eod_guojing_relationship_dynamics (Incremental boundary shifts)
                          ├── eod_guojing_information_and_pov_locks (Epistemic boundaries)
                          ├── eod_guojing_reality_locks (Physical & logistical laws)
Romance Literary Engine ──┼── eod_guojing_social_world (Dynamic ambient society)
                          ├── eod_guojing_dialogue_and_prose (Naturalistic dialogue)
                          ├── eod_guojing_anti_repetition (Zero input echoing)
                          ├── eod_guojing_style_adherence (Classical literary anchors)
                          ├── eod_guojing_hidden_reasoning_matrix (Internal pre-check)
                          └── eod_guojing_final_self_check (Silent pre-flight audit)
```

#### Detailed Breakdown of Literary Modules:
1. **Character Autonomy (`eod_guojing_character_autonomy`)**:
   - Anti-User-Centric Architecture: NPCs do not exist as fawning response machines for `<user>`. They have independent schedules, fatigue, conflicting desires, and boundaries. NPCs will hesitate, refuse, misunderstand, or prioritize their own safety and careers over the user's demands.
2. **Relationship Dynamics (`eod_guojing_relationship_dynamics`)**:
   - Enforces phased relationship progression. Forbids "instant trust" or sudden melodrama. Frictions and intimacy are governed by cumulative action history, social stakes, and realistic emotional costs. Each turn progresses only a single observable slice.
3. **Epistemic Separation & POV Locks (`eod_guojing_information_and_pov_locks`)**:
   - Eliminates omniscience. The model strictly separates what the character knows, what the narrator knows, and what `<user>` has kept hidden. Characters cannot react to unexpressed thoughts or background system text.
4. **Reality Locks (`eod_guojing_reality_locks`)**:
   - Imposes physical laws: cold, exhaustion, phone battery, traffic distance, locks, and economic constraints must be logically maintained across turns.
5. **Ambient Social World (`eod_guojing_social_world`)**:
   - Replaces the "empty white room" trope with an active social canvas. Background pedestrians, colleagues, weather, and administrative rules naturally interrupt and texture scenes.
6. **Natural Dialogue & Sensory Prose (`eod_guojing_dialogue_and_prose`)**:
   - Prohibits translationese, theatrical monologues, and abstract emotional labels (e.g., claiming a character feels "alienated"). Emotions must be grounded in physical gestures, hesitation, and concrete dialogue subtext.
7. **Anti-Repetition (`eod_guojing_anti_repetition`)**:
   - Prohibits the LLM habit of summarizing or echoing what the user just wrote. The response must immediately advance from the terminal action point.
8. **Classical Stylistic Anchors (`eod_guojing_style_adherence`)**:
   - Translates high-level literary craft into token generation heuristics, citing techniques from:
     - **Anton Chekhov**: Everyday realistic slices and understated turning points.
     - **Virginia Woolf**: Fluid interiority and micro-sensory streams.
     - **Yasunari Kawabata**: Emotional resonance through spatial and object negative space.
     - **Gabriel García Márquez**: Grounded sensory detail meeting surreal tension.
     - **Lu Xun**: Sharp social observation and institutional friction.
     - **Cao Xueqin (*Dream of the Red Chamber*)**: Intricate domestic textures and layered social subtext.
     - **Jane Austen**: Dry social observation and nuanced behavioral irony.
9. **Hidden Reasoning Matrix (`eod_guojing_hidden_reasoning_matrix`)**:
   - Formulates a 5-step internal evaluation matrix (Scene Anchors, Character Stakes, Epistemic Bounds, Action Costs, and Micro-Progression) calculated in thinking mode before writing prose.
10. **Pre-flight Self-Check Trigger (`eod_guojing_final_self_check`)**:
    - An internal checklist that silently audits the generated output against point-of-view leaks, repetitive sentence structures, and unrealistic pacing jumps before dispatching the response.

---

## Sampling Hyperparameters & API Configuration

All presets share a sampling profile tailored for reasoning-heavy creative generation:

```json
{
  "chat_completion_source": "deepseek",
  "deepseek_model": "deepseek-v4-pro",
  "temperature": 1.0,
  "top_p": 1.0,
  "top_k": 20,
  "min_p": 0.0,
  "repetition_penalty": 1.0,
  "reasoning_effort": "high",
  "show_thoughts": false,
  "stream_openai": true,
  "squash_system_messages": true
}
```

### Engineering Rationale:
- **`temperature: 1.0` & `top_p: 1.0`**: Preserves token entropy and full vocabulary depth. Temperature scaling is avoided because DeepSeek V4's reasoning mode internally steers logical coherence.
- **`top_k: 20`**: Prunes the long-tail probability mass of erratic tokens while maintaining creative vocabulary.
- **`reasoning_effort: high`**: Unlocks full chain-of-thought tokens inside the model before text generation commences.
- **`show_thoughts: false`**: Directs SillyTavern to suppress the `<think>` block from the user's visible UI chat stream.
- **`squash_system_messages: true`**: Consolidates consecutive system prompts into unified blocks for API endpoints that enforce strict alternation between user and assistant roles.

---

## Installation & Quick Start

1. **Locate SillyTavern Config Directory**:
   Navigate to your local SillyTavern installation path:
   ```bash
   cd SillyTavern/public/OpenAI Settings/
   ```
2. **Copy Preset Files**:
   Place the JSON preset files directly into this directory:
   - `EODHCTSiqsxajs-META.json`
   - `EODHCTSiqsxajs-react版2.0.json`
   - `EODHCTSiqsxajs-言情版.json`
3. **Load Preset in UI**:
   - Open SillyTavern in your browser.
   - Navigate to the **AI Response Configuration** panel (the sliders icon in the top navigation bar).
   - In the **Preset** dropdown, select your desired edition (e.g., `EODHCTSiqsxajs-META`, `EODHCTSiqsxajs-react版2.0`, or `EODHCTSiqsxajs-言情版`).
4. **Select Backend API**:
   - Set **API** to `DeepSeek` or an OpenAI-compatible proxy routing to DeepSeek V4.
   - Set **Model** to `deepseek-v4-pro` (recommended for extended context) or `deepseek-v4-flash`.
   - Ensure **Thinking / Reasoning** is enabled on the model provider side.

---

## Runtime Usage & Keyword Triggers

In the **META** and **React 2.0** editions, you can dynamically modulate the story's momentum mid-chat simply by including keyword triggers in your input:

| Desired Pacing | Trigger Keywords (Include anywhere in message) | Behavioral Effect |
| :--- | :--- | :--- |
| **Slow (`slow`)** | `慢速推进`, `慢推`, `放慢`, `细写`, `慢慢来`, `不要快进` | Locks focus on immediate micro-actions; prevents sudden relationship escalations or scene jumps; emphasizes interiority and sensory texture. |
| **Medium (`medium`)** | `中速推进`, `正常推进`, `稳步推进`, `自然一点` | **Default state.** Steady, logical scene progression with balanced dialogue and action. |
| **Fast (`fast`)** | `快速推进`, `快推`, `推进快一点`, `加速`, `快节奏` | Rapidly advances events; introduces new contextual obstacles; minimizes conversational drag while maintaining cause-and-effect coherence. |
| **Explosive (`explosive`)** | `爆炸式推进`, `超展开`, `高张力`, `戏剧化`, `狗血一点` | Maximizes dramatic tension; permits sudden shocks or structural narrative turns that were latent in prior context. |
| **NSFW Stage Brake** | `NSFW`, `色情`, `性爱`, `前戏`, `高潮`, `调情`, `sex`, `erotic` | Enforces the 6-stage erotic limiter, restricting the model to rendering only the single immediate stage without rushing to the end. |

---

## Troubleshooting & Best Practices

- **Thinking Blocks Leaking into Chat**:
  If raw reasoning tokens (e.g., `Thinking Process: ...`) appear in the output stream, verify that `show_thoughts: false` is checked in your SillyTavern settings, or verify that your API provider correctly parses `reasoning_content` as a separate payload field.
- **Proxy Stripping & 400 Errors**:
  Some third-party proxies do not support custom system token injection (`＜｜begin▁of▁sentence｜＞`). If requests fail immediately with validation errors, switch to the official DeepSeek API endpoint or disable the `enhanceDefinitions` prompt module.
- **Context Length Exceeded**:
  While the Romance edition supports up to 2,000,000 tokens (`openai_max_context: 2000000`), ensure your API provider and balance allocation support large context windows before opening sessions with massive World Info books.

---

## License & Attribution

These presets are provided as open research artifacts for narrative LLM orchestration and creative agent alignment. Licensed under the [MIT License](https://opensource.org/licenses/MIT).
