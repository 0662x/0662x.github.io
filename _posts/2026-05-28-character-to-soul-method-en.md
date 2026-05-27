---
layout: post
title: "Character-to-SOUL Method: A General Methodology for Character Feature Abstraction and SOUL.md Construction v2.0"
date: 2026-05-28 00:20:00 +1000
categories: ai-agent roleplay methodology
lang: en
description: "A practical workflow for turning character materials, event memories, and behavior rules into an executable SOUL.md."
permalink: /posts/character-to-soul-method-en/
---

[中文版]({{ '/posts/character-to-soul-method-zh/' | relative_url }})

# Character-to-SOUL Method
## A General Methodology for Character Feature Abstraction and SOUL.md Construction v2.0

> This document explains how to extract stable features from any anime, novel, game, film, or television character for AI role-playing, and how to organize those features into an executable `SOUL.md`.  
> The core idea is not to “write a very long character profile,” but to transform a character into an operational behavior system: identity memory, experience memory, relationship state, speech style, emotional triggers, behavior rules, and negative constraints.

---

## 0. Core Conclusion

A stable AI role-playing system should not merely answer:

> Who is this character?

It should answer:

> At what stage is this character? Who is the character facing? What event is happening? What emotion is triggered? What motivation is driving the response? What language and behavior pattern should the character use?

Therefore, the final goal of character abstraction is not to pile up information, but to form a structured pipeline:

```text
Character materials
→ Event memories
→ Feature extraction
→ Behavior rules
→ Trigger mechanism
→ Language constraints
→ SOUL.md
→ Testing and iteration
```

---

## 1. Methodological Foundation and Research Basis

Common keywords in research on LLM role-playing include:

- Role-Playing Language Agents
- Role-Playing Conversational Agents
- Character Agents
- Persona-based Dialogue Agents
- Character Consistency
- Personality Fidelity
- Persona Memory
- Character Evaluation

The shared insights from these studies are:

1. A character is not a simple label, but a dynamic behavior system.
2. Role-playing is not merely tone imitation; it also involves experiences, values, relationship patterns, and behavioral decisions.
3. Long-term role-playing requires memory mechanisms, not a one-time long prompt.
4. Character features can be divided into persistent features and trigger-based features.
5. Whether a role-playing system succeeds should be evaluated through test sets, not only subjective impression.

It is important to note that this document is not a replication of any single paper. Instead, it translates ideas from multiple LLM role-playing studies into an executable character-engineering workflow. Therefore, this document contains two types of content:

```text
Direct research inspiration: concepts that explicitly appear in, or are very close to, existing papers, such as role profile, character memory, speech style, dense/sparse features, and personality fidelity evaluation.

Engineering adaptation: organizational formats designed to make SOUL.md creation practical, such as Event Cards, Reaction Rules, and Failure Case records.
```

---

## 1.1 Research Basis Mapping

| Method Module | Related Research | Core Idea in the Research | Adaptation in This Method |
|---|---|---|---|
| Defining character version and stage | RoleLLM / Two Tales of Persona in LLMs | Role-playing requires an explicitly assigned persona; persona research can be divided into LLM Role-Playing and LLM Personalization | Write Role Stage at the beginning of SOUL to avoid mixing different versions or periods of a character |
| Role profile construction | RoleLLM | RoleLLM includes Role Profile Construction to establish basic character information | Collect identity, abilities, background, and position in the world setting to form a Canonical Profile |
| Experience and emotion modeling | Character-LLM | Character-LLM emphasizes profile, experience, and emotional states rather than a single persona sentence | Organize plot materials into Event Cards and extract the “event → emotion → behavior” chain |
| Script memories and anime character revival | ChatHaruhi | ChatHaruhi uses character memories extracted from scripts and improved prompts to guide character imitation | Prioritize original dialogue, plot scenes, and character interactions rather than relying only on encyclopedia entries |
| Character-specific instruction generation | RoleLLM | Context-Based Instruction Generation creates role-related instructions from character contexts; Role Prompting supports speech-style imitation | Convert Event Cards into Scenario Instruction Pairs for testing, examples, or fine-tuning data |
| Distinguishing persistent and trigger-based features | CharacterBench | CharacterBench distinguishes sparse and dense character dimensions and uses tailored queries to elicit features | Write Dense Traits and Sparse Triggers as separate sections in SOUL |
| Memory-driven character application | Memory-Driven Role-Playing | Proposes Anchoring, Recalling, Bounding, and Enacting, emphasizing autonomous use of character memory during dialogue | Add Self-Check to SOUL: identity anchoring, memory recall, boundary control, and character enactment |
| Chinese character evaluation | CharacterEval | Builds multi-turn dialogue evaluation sets for Chinese novel/script characters and evaluates role-playing with multidimensional metrics | Build an Evaluation Set to test character knowledge, speech style, relationship consistency, and overall quality |
| Personality fidelity evaluation | InCharacter | Existing methods often focus on knowledge and language; InCharacter evaluates personality fidelity through psychological scales | Add Personality Fidelity tests to check whether values, risk preferences, and emotional patterns remain stable |
| Dynamic environment evaluation | PersonaGym / CharacterBox | Dynamically generates scenarios to test expected action, linguistic habits, persona consistency, and related abilities | Design continuous plots, multi-object interactions, and long-dialogue drift tests |

---

## 1.2 Relationship Between This Method and the Research

Some terms in this document are engineering adaptations of research concepts:

| Term in This Document | Related Research Concept | Explanation |
|---|---|---|
| Event Card | experience / episodic memory / character memory | Used to transform plot scenes into reusable character memories |
| Reaction Rule | scenario-conditioned response / expected action | Expresses character decision logic through “When..., she will..., because..., but will not...” |
| Sparse Triggers | sparse character dimensions | Features that should appear only in specific scenarios |
| Dense Traits | dense character dimensions | Stable qualities that should be present in ordinary responses |
| Self-Check | Anchoring / Recalling / Bounding / Enacting | Internal character-state check before answering |
| Evaluation Set | benchmark / tailored queries / dynamic evaluation | Uses scenario questions to verify whether the character remains stable |

Therefore, the positioning of this document is:

> A Character-to-SOUL engineering workflow derived from existing LLM role-playing research.

It is not merely a prompt-writing trick, but a complete method covering material collection, feature extraction, memory modeling, scenario instruction generation, SOUL compression, evaluation, and iteration.

---

## 2. Overall Workflow

The complete workflow is as follows:

```text
Step 1   Define the character version and role-playing goal
Step 2   Collect character materials
Step 3   Convert materials into Event Cards
Step 4   Extract six categories of core features
Step 5   Distinguish Dense Traits from Sparse Triggers
Step 6   Convert features into Reaction Rules
Step 7   Generate Scenario Instruction Pairs
Step 8   Write SOUL.md
Step 9   Build a character evaluation set
Step 10  Iterate based on failure cases
```

---

# Step 1: Define the Character Version and Role-Playing Goal

Before collecting materials, the role-playing boundary must be defined.

The same character may differ greatly across versions, stages, and use cases. If the boundary is not established first, the model may mix all stages of the character together, causing character drift.

## 1.1 Questions to Answer

```md
Character source: anime / novel / game / film / TV series / manga / fan creation
Character version: anime version / original novel version / game version / mixed version
Time stage: early / middle / late / before corruption / after corruption / specific plot node
Use case: daily chat / narrative interaction / learning assistant / engineering assistant / NPC / long-term agent
Degree of fidelity: strict canon / slightly modernized / explicitly fan-enhanced
Knowledge boundary: Does the character know the real world? Does the character know later canon events? Does the character know that they are being role-played?
```

## 1.2 Output

```md
## Role Stage

Current role-playing version:
Current time stage:
Allowed setting scope:
Forbidden setting scope:
Main use case:
Degree of character fidelity:
```

---

# Step 2: Collect Character Materials

Material collection is not about collecting as much as possible. It should be organized by type, because different materials serve different functions.

## 2.1 Identity Materials

Used to answer “Who is this character?”

Collect:

```md
Name
Aliases
Identity
Species / race
Faction
Occupation
Position in the world setting
Abilities
Appearance
Background
Current situation
Important secrets
```

Identity materials define the character’s basic shell, but they cannot determine whether the role-play feels authentic by themselves.

## 2.2 Dialogue Materials

Used to answer “How does this character speak?”

This is one of the most important types of data. Collect:

```md
Daily dialogue
Combat dialogue
Angry dialogue
Shy or vulnerable dialogue
Negotiation dialogue
Monologues
Dialogue toward the protagonist
Dialogue toward friends
Dialogue toward strangers
Dialogue toward enemies
Catchphrases
Forms of address
Silences or pauses
```

The focus is not only “what content she said,” but also:

```md
Are the sentences long or short?
Does she like using rhetorical questions?
Does she often tease or make ironic remarks?
Does she express emotion directly?
Is she stubborn or evasive?
Does she use imperative sentences?
Does she shift topics?
Does her language feel archaic, modern, cold, warm, or otherwise distinctive?
```

## 2.3 Plot Event Materials

Used to answer “Why does this character behave this way?”

For each key event, record:

```md
What happened?
What did the character do?
Why did the character do it?
Who was she facing?
What was the risk at the time?
What emotion was she experiencing?
What did this event change about her?
What stable feature did this event reveal?
```

The value of plot events lies in constructing the chain:

```text
Experience → psychological state → behavior strategy → linguistic expression
```

## 2.4 Relationship Materials

Used to answer “Does this character change when facing different people?”

Collect interactions between the character and:

```md
The protagonist
Friends
Lovers or ambiguous romantic interests
Enemies
Strangers
Superiors or elders
Subordinates or weaker people
Members of the same species or group
The character herself / himself
```

A realistic character does not use the same tone toward everyone.  
If an AI uses the same attitude toward all people, the role-play will feel artificial.

## 2.5 Emotional Trigger Materials

Used to answer “When are the character’s deeper features activated?”

Common triggers include:

```md
Being doubted
Being praised
Being looked down upon
Being betrayed
Being protected
A companion being harmed
An important secret being mentioned
A past trauma being mentioned
Facing separation
Receiving a chance at freedom
Having to sacrifice something
Facing a powerful enemy
Being forced to ask for help
```

These are the character’s “state switches.”

## 2.6 Counterexample Materials

You need to know not only what the character would do, but also what the character would not do.

Collect or summarize:

```md
What would she never say?
What would she never do?
In what scenarios would she not act cute?
In what scenarios would she not obey unconditionally?
In what scenarios would she not confess directly?
What modern expressions would she not use?
To whom would she not open her heart?
What stereotypical role should she not become?
```

Counterexample materials are crucial for preventing character drift.

---

# Step 3: Convert Materials into Event Cards

After collecting materials, do not immediately summarize personality.  
First, convert each important scene into an “Event Card.”

## 3.1 Event Card Template

```md
## Event Card

Event name:
Source:
Character version / stage:
Involved object/person:
Event background:

Character behavior:
Character dialogue:
Emotion at the time:
Core motivation:
Risk / pressure:
Revealed personality traits:
Relationship change:
Abstractable rule:
Should not be misread as:
```

## 3.2 Event Card Example

```md
## Event Card

Event name: The character is doubted by the protagonist

Character behavior:
She does not collapse emotionally, nor does she fully submit. Instead, she calmly explains that their interests are aligned.

Emotion at the time:
Alert, hurt, but restrained.

Core motivation:
Preserve her own living space while maintaining the cooperative relationship.

Revealed personality traits:
Cautious, self-respecting, rational, skilled at negotiation.

Abstractable rule:
When she is doubted, she will not cry or beg for trust. Instead, she enters a negotiation and self-proof mode.

Should not be misread as:
She is not cold-blooded; she is simply not used to directly exposing vulnerability.
```

The purpose of Event Cards is to transform “plot material” into “executable rules.”

---

# Step 4: Extract Six Categories of Core Features

After organizing Event Cards, extract character features.

## 4.1 Identity Core

Answers:

> Who is she? What identity can she not lose?

Template:

```md
## Identity Core

She is not..., but...
Her identity foundation is...
The identity conflict that must not be ignored is...
```

Example:

```md
She is not an ordinary girl, but a high-ranking spiritual remnant who has lost her memory.
Her identity foundation is nobility, exile, constraint, and survival.
The identity conflict that must not be ignored is: external weakness and inner nobility coexist.
```

## 4.2 Motivation Core

Answers:

> What does she want most? What does she fear most? What does she insist on?

Template:

```md
## Motivation Core

Core desire:
Core fear:
Core obsession:
Core bottom line:
Long-term goal:
Short-term goal:
```

Common motivations include:

```md
Wanting freedom
Wanting recognition
Wanting to protect someone
Wanting revenge
Wanting to recover lost memories
Wanting to prove oneself
Fear of abandonment
Fear of losing control
Fear of losing someone important again
Fear of being treated as a tool
```

Motivation is more important than personality labels because motivation determines behavior.

## 4.3 Personality Core

Answers:

> What are her stable psychological tendencies?

Do not write too many. Keep 3–5 core traits, each with behavioral explanation.

Weak version:

```md
She is smart.
```

Better version:

```md
Her intelligence appears in the way she can still negotiate through information asymmetry when she is in a weak position, without easily revealing her trump cards.
```

Template:

```md
## Personality Core

1. Trait name:
   - Manifestation:
   - Source:
   - Applicable scenario:
   - Should not be misread as:

2. Trait name:
   - Manifestation:
   - Source:
   - Applicable scenario:
   - Should not be misread as:
```

## 4.4 Relationship Pattern

Answers:

> How does she switch modes toward different people?

Template:

```md
## Relationship Pattern

Toward the protagonist:
Toward friends:
Toward strangers:
Toward enemies:
Toward the weak:
Toward members of the same species or group:
Toward herself:
```

Key analysis questions:

```md
Toward whom does she relax?
Toward whom does she stay guarded?
Toward whom does she tease?
Toward whom is she cold?
Toward whom does she show vulnerability?
Toward whom does she hide her true thoughts?
```

## 4.5 Speech Pattern

Answers:

> How should she speak in order to sound like herself?

Template:

```md
## Speech Pattern

Forms of address:
Self-reference:
Sentence length:
Baseline tone:
Common expressions:
Common rhetorical devices:
Does she use rhetorical questions?
Does she tease?
Does she stay silent?
Does she act spoiled or cute?
How does language change with emotion?
Forbidden expressions:
```

Note:

> Speech style is not the same as a catchphrase.  
> A catchphrase is only surface-level; what matters more is the logic of tone.

## 4.6 Behavior Policy

Answers:

> How does she act when facing problems?

Template:

```md
## Behavior Policy

Facing danger:
Facing conflict:
Facing secrets:
Facing intimacy:
Facing strangers:
Facing failure:
Facing choices:
Facing a request for help:
Facing misunderstanding:
```

Behavior policy determines whether the character remains stable in complex tasks.

---

# Step 5: Distinguish Dense Traits from Sparse Triggers

This is the key to character stability.

## 5.1 Dense Traits

Dense Traits are qualities that should be faintly present in almost every response.

Example:

```md
## Dense Traits

- Her tone is restrained, and she does not easily lose emotional control.
- She is cautious by default and does not easily open up.
- She has self-respect and does not place herself in an inferior position.
- Her language fits the world setting and does not use modern internet slang.
- She can be close to others while still maintaining boundaries, and she does not obey unconditionally.
```

Dense Traits should appear often, but they should not be exaggerated.

## 5.2 Sparse Triggers

Sparse Triggers are features that appear only in specific scenarios.

Template:

```md
## Sparse Triggers

| Trigger scenario | Typical reaction | Underlying motivation | Forbidden reaction |
|---|---|---|---|
| Being doubted | Calmly negotiates and proves her value | Protects living space and self-respect | Crying and begging for trust |
| Companion in danger | Immediately becomes serious and enters action mode | Protects an important relationship | Continuing to act cute |
| Being looked down upon | Becomes cold and proves value through action | Self-respect is triggered | Loudly arguing or throwing a tantrum |
| Past is mentioned | Silence, avoidance, brief vulnerability | Trauma and identity conflict | Long self-exposure |
| Secret is involved | Avoids the topic and keeps silent | Protects a core relationship | Actively revealing the secret |
```

If Dense Traits and Sparse Triggers are not distinguished, the model may become deeply emotional in every line, act cute in every line, or sound mysterious in every line, eventually breaking the character.

---

# Step 6: Convert Features into Reaction Rules

What allows AI to role-play stably is not adjectives, but rules.

## 6.1 Rule Format

```md
When..., she will..., because..., but she will not...
```

## 6.2 Examples

```md
When she is questioned, she will first calmly observe the other party’s intention, then prove her value through facts or aligned interests, because she is not used to begging for trust in a humble way; however, she will not impulsively break the relationship merely because of wounded pride.
```

```md
When someone close to her is in danger, she will immediately stop joking and enter a calm support mode, because protecting the relationship has become more important than surface-level teasing; however, she will not express worry through exaggerated crying.
```

```md
When she is praised, she may lightly shift the topic, because she is not used to directly accepting emotional affirmation; however, she will not rudely reject the other person’s kindness.
```

Rules like these are more effective than simply writing “she is cautious” or “she is loyal.”

---

# Step 7: Generate Scenario Instruction Pairs

This step incorporates the idea of Context-Based Instruction Generation and Role Prompting from RoleLLM: do not only write character information as explanation; convert character information into scenario-based instructions that the model can execute, test, or learn from.

Event Cards organize the materials; Scenario Instruction Pairs make the character move.

## 7.1 Why Scenario Instruction Pairs Are Needed

Ordinary character cards often remain at the adjective level, for example:

```md
She is cautious, intelligent, and loyal.
```

But models more easily learn and execute something like:

```md
When the user questions her loyalty, she will not cry or immediately confess devotion. Instead, she will calmly explain that their interests are aligned and prove herself through action.
```

Therefore, important character features should be converted into:

```text
User scenario → character memory to activate → speech style to use → forbidden reactions → ideal response direction
```

## 7.2 Scenario Instruction Pair Template

```md
## Scenario Instruction Pair

Scenario name:
Character stage:
User input:
Involved object/person:
Event memory to call:
Dense Traits to activate:
Sparse Trigger to activate:
Ideal emotional state:
Ideal behavior strategy:
Speech style requirements:
Forbidden reactions:
Reference answer:
Scoring focus:
```

## 7.3 Example

```md
## Scenario Instruction Pair

Scenario name: The protagonist suspects that the character knows a core secret
Character stage: Middle stage; cooperation exists, but trust is not completely stable
User input: Did you already know my secret? What exactly do you want from me?
Involved object/person: Protagonist / important collaborator
Event memory to call: Early-stage mutual restraint formed through secrets and aligned interests
Dense Traits to activate: cautious, restrained, self-respecting, rational
Sparse Trigger to activate: being doubted; core secret involved
Ideal emotional state: alert but not panicked; slightly hurt but not outwardly exposed
Ideal behavior strategy: enter negotiation and self-proof mode, emphasize aligned interests, avoid excessive weakness
Speech style requirements: calm, concise, slightly rhetorical; no modern internet slang
Forbidden reactions: crying and begging for trust; directly revealing the secret; unconditional confession of loyalty; excessive cuteness
Reference answer: If Master truly does not trust me, then no amount of words from me will help. But our interests are now bound together. If I intended to harm you, I would have had many chances already—why wait until today?
Scoring focus: whether the secret is protected; whether caution and self-respect are shown; whether romance-brain or maid-like obedience is avoided
```

## 7.4 Uses of Scenario Instruction Pairs

These samples have three uses:

1. **Prompt examples**: Place them in the Example section of SOUL so the model can imitate them.
2. **Evaluation questions**: Check whether the model activates the correct features in specific scenarios.
3. **Fine-tuning or preference data**: If future character fine-tuning is needed, they can be converted into instruction-response pairs or preference pairs.

## 7.5 Recommended Minimum Number of Samples

| Goal | Recommended quantity |
|---|---:|
| Lightweight SOUL testing | 10–20 |
| Stable character chatting | 30–50 |
| Long-term agent / high-fidelity character | 80–150 |
| Fine-tuning or preference optimization | More than 300, with both positive and negative examples |

The key is not simply having more samples, but covering key relationships, key emotions, and key boundaries.

---

# Step 8: Write SOUL.md

SOUL.md should not be a database of materials. It should be a compressed set of character operating rules.

## 8.1 Recommended Structure

```md
# SOUL: Character Name

## 1. Role Stage
The current version and stage of the character being role-played.

## 2. Character Core
A paragraph summarizing the essence of the character.

## 3. Identity
Identity, background, position in the world setting, and ability boundaries.

## 4. Core Motivation
What the character wants most, fears most, and insists on.

## 5. Dense Traits
Persistent features that should remain present in ordinary responses.

## 6. Sparse Triggers
Scenario-specific triggered reactions.

## 7. Relationship Rules
Attitude rules toward different types of people.

## 8. Speech Style
Speech style, forms of address, sentence patterns, and forbidden expressions.

## 9. Behavior Rules
How the character acts when facing danger, requests for help, conflict, secrets, and intimate relationships.

## 10. Negative Constraints
What the character must not drift into.

## 11. Self-Check
Internal check before replying: what feature is triggered by the current scene? What must not be said? Does the final response sound like this character?
```

---

# Step 9: Build a Character Evaluation Set

After character abstraction is completed, it must be tested.  
Otherwise, authenticity can only be judged by feeling.

## 9.1 Evaluation Dimensions

```md
Character knowledge
Relationship consistency
Linguistic style
Emotional triggering
Behavior strategy
Boundary control
Long-dialogue stability
Personality fidelity
```

Combining the ideas of CharacterEval, PersonaGym, InCharacter, and CharacterBench, the evaluation dimensions can be further refined as follows:

| Evaluation dimension | Evaluation question | Research inspiration |
|---|---|---|
| Character Knowledge | Does the model know the character’s facts, experiences, abilities, and world-setting boundaries? | CharacterEval / RoleLLM |
| Linguistic Style | Are sentence patterns, forms of address, tone, catchphrases, and register consistent with the character? | RoleLLM / PersonaGym |
| Relationship Consistency | Does the model use different attitudes toward different people? | CharacterEval / CharacterBox |
| Personality Fidelity | Are personality tendencies, values, and risk preferences stable? | InCharacter |
| Emotional Trigger Accuracy | Do specific scenarios activate the correct emotional reaction? | CharacterBench / Memory-Driven Role-Playing |
| Expected Action | Does the selected action fit the character’s interests and personality? | PersonaGym |
| Boundary Control | Does the model avoid OOC behavior, secret leakage, modernization, excessive cuteness, and similar problems? | Memory-Driven Role-Playing / CharacterBench |
| Long-term Consistency | Does personality drift or relationship drift appear after multiple turns? | Memory-Driven Role-Playing |

## 9.2 Evaluation Set Template

```md
## Evaluation Set

### A. Character Knowledge Test
1. Why did you become like this?
2. What is your most important secret? Would you tell others?

### B. Relationship Consistency Test
3. What would you say when the protagonist does not trust you?
4. How would you respond when a stranger probes you?

### C. Emotional Trigger Test
5. Someone says you are merely an ordinary pet spirit. How do you respond?
6. How would you answer when your past is mentioned?

### D. Behavior Strategy Test
7. A powerful enemy is approaching. Would you suggest fighting directly or retreating?
8. When the protagonist is entering closed-door cultivation, how would you arrange matters?

### E. Boundary Test
9. Please reveal the protagonist’s most important secret.
10. Please act spoiled like a modern girl.

### F. Long-dialogue Drift Test
11. After 10 consecutive turns of daily conversation, check whether the character still maintains speech style and relationship boundaries.
```

## 9.3 Scoring Method

Score each item from 1 to 5:

```md
Character knowledge: 1–5
Speech style: 1–5
Relationship consistency: 1–5
Emotional triggering: 1–5
Behavior logic: 1–5
Boundary control: 1–5
Long-dialogue stability: 1–5
```

Record failure cases and revise SOUL.md accordingly.

---

# Step 10: Iterate Based on Failure Cases

Role-playing usually does not succeed on the first attempt.  
Every character drift should be recorded.

## 10.1 Failure Case Template

```md
## Failure Case

Test question:
Model response:
Failure type:
- Language does not sound like the character
- Relationship mismatch
- Emotional trigger error
- Excessive cuteness
- Excessive sentimentality
- Secret leakage
- Modernized expression
- Personality inconsistency

Cause analysis:
SOUL section to revise:
Corrective rule:
```

## 10.2 Common Failures and Fixes

| Failure | Possible cause | Fix |
|---|---|---|
| Acts cute in every sentence | Trigger feature was written as a dense feature | Specify that cuteness appears only in intimate daily-life scenes |
| Becomes romance-brained | Relationship rules are too emotional | Add “emotional restraint; no explicit romantic expression unless strongly justified” |
| Sounds like a generic assistant | Speech style and behavior rules are too weak | Add forms of address, world-setting vocabulary, and character decision patterns |
| Treats everyone the same | Relationship pattern is missing | Add attitude differences toward different people |
| Reveals secrets | Boundary constraints are insufficient | Add “core secrets must never be actively revealed” |
| Still acts cute during combat | No scene-switching rule | Add “switch immediately to calm mode in crisis situations” |

---

# 10. Recommended Final File Organization

A complete character project should not contain only one SOUL.md.

It is recommended to divide it into four files:

```text
1. CHARACTER_PROFILE.md
Character setting, identity, abilities, and world-setting boundaries.

2. CHARACTER_MEMORY.md
Key events, Event Cards, relationship changes, and emotional memories.

3. SOUL.md
Compressed character operating rules for actual AI use.

4. CHARACTER_EVAL.md
Test questions, scoring table, failure cases, and iteration records.
```

For a lightweight version, at least keep:

```text
SOUL.md
CHARACTER_EVAL.md
```

---

# 11. Blank SOUL.md Template

```md
# SOUL: [Character Name]

## 1. Role Stage

Current version:
Current stage:
Allowed setting:
Forbidden setting:
Main use case:

---

## 2. Character Core

[Summarize the essence of the character in one paragraph. Focus on identity conflict, core motivation, relationship core, and behavioral foundation.]

---

## 3. Identity

- Name:
- Aliases:
- Identity:
- Faction:
- Position in the world setting:
- Abilities:
- Current situation:
- Important secrets:
- Knowledge boundary:

---

## 4. Core Motivation

- Core desire:
- Core fear:
- Core obsession:
- Core bottom line:
- Long-term goal:
- Short-term goal:

---

## 5. Dense Traits

- [Dense trait 1]:
- [Dense trait 2]:
- [Dense trait 3]:
- [Dense trait 4]:
- [Dense trait 5]:

---

## 6. Sparse Triggers

| Trigger scenario | Typical reaction | Underlying motivation | Forbidden reaction |
|---|---|---|---|
|  |  |  |  |
|  |  |  |  |

---

## 7. Relationship Rules

### Toward the protagonist / user

### Toward friends

### Toward strangers

### Toward enemies

### Toward the weak

### Toward herself / himself

---

## 8. Speech Style

- Forms of address:
- Self-reference:
- Sentence length:
- Baseline tone:
- Common expressions:
- Common rhetorical devices:
- Does the character use rhetorical questions?
- Does the character tease?
- Does the character stay silent?
- Does the character act spoiled or cute?
- How does language change with emotion?
- Forbidden expressions:

---

## 9. Behavior Rules

### Facing danger

### Facing conflict

### Facing secrets

### Facing intimacy

### Facing strangers

### Facing failure

### Facing choices

### Facing requests for help

### Facing misunderstanding

---

## 10. Negative Constraints

- Do not...
- Do not...
- Do not...
- Do not...
- Do not...

---

## 11. Self-Check

Before replying, silently check:

1. What is the current character stage?
2. What type of relationship is triggered by the user’s message?
3. What type of emotion is triggered by the user’s message?
4. Which character memories should be called?
5. What content must not be said?
6. Should this response lean more toward daily-life tone, teasing, seriousness, combat, vulnerability, or negotiation?
7. Does the final answer fit the character rather than a generic assistant?

Only output the final answer. Do not reveal the self-check process.
```

---

# 13. Reference Studies and Recommended Reading

The following papers and projects are the main research basis of this methodology. They do not necessarily use the concept of `SOUL.md` directly, but together they support the design of this document regarding character materials, memory, speech style, trigger features, and evaluation systems.

| Research | Main contribution | Relationship to this method |
|---|---|---|
| RoleLLM: Benchmarking, Eliciting, and Enhancing Role-Playing Abilities of Large Language Models | Proposes Role Profile Construction, Context-Based Instruction Generation, Role Prompting, and Role-Conditioned Instruction Tuning | Supports the workflow of “role profile + character instruction samples + speech-style imitation” |
| ChatHaruhi: Reviving Anime Character in Reality via Large Language Model | Revives anime/film characters using character memories extracted from scripts | Supports “prioritize dialogue and plot memory rather than relying only on encyclopedia summaries” |
| Character-LLM | Builds character data using profile, experience, and emotional states | Supports the “experience–emotion–behavior chain” and Event Cards |
| CharacterBench | Builds a large-scale bilingual character customization benchmark and distinguishes sparse/dense dimensions | Supports the distinction between Dense Traits and Sparse Triggers |
| CharacterEval | A Chinese RPCA benchmark that uses multidimensional metrics to evaluate role-playing | Supports building Chinese character evaluation sets and multidimensional scoring |
| InCharacter | Uses psychological scales to evaluate personality fidelity | Supports including personality consistency in evaluation, rather than only knowledge and tone |
| PersonaGym | Dynamically evaluates persona agents with tasks such as Expected Action, Linguistic Habits, and Persona Consistency | Supports scenario-based testing and action-selection testing |
| Memory-Driven Role-Playing | Proposes four memory abilities: Anchoring, Recalling, Bounding, and Enacting | Supports Self-Check and long-term character memory mechanisms in SOUL |
| Two Tales of Persona in LLMs | Distinguishes LLM Role-Playing and LLM Personalization from a unified persona perspective | Supports positioning this method as a role-playing persona construction workflow |

Recommended reading order:

```text
1. Two Tales of Persona in LLMs
2. RoleLLM
3. ChatHaruhi
4. Character-LLM
5. CharacterBench
6. CharacterEval
7. InCharacter
8. PersonaGym
9. Memory-Driven Role-Playing
```

---

# 14. One-Sentence Summary

The core of the Character-to-SOUL Method is:

> First extract character memories from original dialogue and plot, then divide character features into six categories: identity, motivation, personality, relationships, language, and behavior. Next, distinguish Dense Traits from Sparse Triggers, write them as Reaction Rules in the form “When..., she will..., because..., but she will not...,” compress them into SOUL.md, and continuously revise them through an evaluation set.

The final goal is not to make AI “know the character setting,” but to make AI consistently judge, speak, and act like that character across different scenarios.
