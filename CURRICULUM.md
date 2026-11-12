# Byteville: Build 30 AI Agents

*A hands-on, slightly silly coding course. You will build 30 AI agents, one at a time, and finish with a small town that runs on them.*

> **Status:** draft curriculum for review. Language, framework, and model provider are still open questions (see [Decisions needed](#decisions-needed)). No lesson code yet.
>
> New to agents? Read [PRIMER.md](PRIMER.md) first.

---

## The premise

You have just been hired as the town engineer of **Byteville**, a small town with big problems and a tiny budget. Every lesson, a townsperson knocks on your door with a request: the mayor cannot make a decision, the librarian is buried in PDFs, the bakery's recommendations are terrible, the school robot keeps walking into walls. You build an agent to help them.

By the end, Byteville's agents talk to each other, and the capstone is a "town hall" where they collaborate on one big problem.

### Why a story?

- It gives every agent a **concrete, memorable job** instead of an abstract category.
- Characters return, so later agents can **reuse** earlier ones (the Librarian's retrieval agent shows up again in the Clinic and the Courthouse).
- It is more fun to debug "why does the Mayor's agent keep choosing tacos?" than "why does Agent 1 fail test case 7?"

## How the course works

### One growing toolkit

Rather than 30 unrelated scripts, learners build one small library, **`bytekit`**, and each lesson adds a piece. By Lesson 10 they have their own mini agent framework and understand every line of it.

| Unlocked in | `bytekit` piece |
|---|---|
| L1 | `loop`: the think, act, observe cycle |
| L1 | `tools`: tool registry with schemas generated from Python functions |
| L2 | `planner`: plan, execute, re-plan |
| L3 | `memory`: short-term buffer plus long-term store |
| L4 | `retrieve`: chunking, embeddings, vector search |
| L7 | `mcp`: plug in MCP tool servers |
| L8 | `crew`: multi-agent messaging and hand-offs |
| L9 | `flow`: deterministic workflow graphs with checkpoints |
| L11 | `evals`: tiny test harness for agent behavior |
| L14 | `guard`: input, output, and action guardrails |
| L23 | `trace`: step-by-step run explanations |

### Every lesson has the same shape

1. **The Knock** (story hook): who needs help and why.
2. **Concept Corner**: the one or two ideas this agent teaches, with a diagram.
3. **Build It**: guided code, in small runnable steps.
4. **Break It**: deliberately make the agent fail and see why. This is where the real learning happens.
5. **Boss Check**: a small eval suite the agent must pass to "clear the level."
6. **Side Quest** (optional): a stretch challenge for keen learners.

Target length is 45 to 90 minutes per lesson.

### Difficulty curve

```
Module:  1    2    3    4    5    6    7    8    9   10   Capstone
         ▁    ▂    ▃    ▃    ▅    ▄    ▆    ▅    ▆    ▇    █
      basics  RAG  tools data code  chat media ethics domains  embodied
```

---

## Lesson 0: Welcome to Byteville

Set up the environment, get an API key, make your first model call, and meet the town. Learners write a 10-line "hello agent" that just chats, so Lesson 1 has a clear before-and-after.

**Covers:** API keys and costs, tokens, temperature, system prompts, structured output.

---

## Module 1: The Brain (Foundational cognitive architectures)

### L1. The Autonomous Decision-Making Agent: *"The Mayor Can't Decide"*
The Mayor must pick the theme for the town festival and keeps flip-flopping. Build an agent that gathers facts with tools (weather, budget, survey results), weighs options, and commits to a decision with reasons.
- **Concepts:** the agent loop from scratch, tool calling, stopping conditions, step limits.
- **Break It:** remove the step limit and watch it loop forever; give it contradictory tool results.
- **Boss Check:** makes a decision in at most N steps and cites the tool results it used.

### L2. The Planning Agent: *"Festival Logistics"*
Now the festival has to actually happen. Build an agent that breaks "run the festival" into a plan, executes steps, and re-plans when a tool reports "the bouncy castle vendor cancelled."
- **Concepts:** plan-and-execute vs. ReAct, task decomposition, re-planning on failure, plan as data (JSON).
- **Break It:** inject a failure mid-plan; compare an agent that re-plans with one that does not.
- **Boss Check:** survives three random mid-plan failures and still finishes.

### L3. The Memory-Augmented Agent: *"The Barista Who Remembers"*
The café wants an assistant that remembers regulars: names, orders, the fact that Dave is "trying to cut back on oat milk."
- **Concepts:** working vs. long-term vs. semantic memory, summarization, what to remember and what to forget, privacy.
- **Break It:** overflow the context window; store a wrong fact and see it stick.
- **Boss Check:** correctly recalls preferences across five separate sessions.

## Module 2: The Library (Information retrieval and knowledge)

### L4. The Knowledge Retrieval Agent: *"Ask the Librarian"*
Byteville's library has 300 years of town records. Build a RAG agent that answers questions with citations.
- **Concepts:** chunking, embeddings, vector search, hybrid (keyword plus vector) search, citations, "I don't know."
- **Break It:** ask questions with no answer in the corpus; measure hallucination.
- **Boss Check:** answer accuracy and citation accuracy on a 20-question set.

### L5. The Document Intelligence Agent: *"The Paperwork Avalanche"*
Town Hall has a pile of scanned permits, invoices, and handwritten forms. Build an agent that classifies documents and extracts structured fields.
- **Concepts:** PDF and image parsing, structured extraction with schemas, confidence scores, routing low-confidence items to a human.
- **Break It:** feed it a blurry scan and a form with missing fields.
- **Boss Check:** field-level accuracy on a labeled sample.

### L6. The Scientific Research Agent: *"The Great Pothole Study"*
The council asks: "Do potholes really get worse after rain?" Build an agent that searches papers and the web, reads sources, and writes a short literature review.
- **Concepts:** iterative search, source quality, deep-research loops, synthesis, citation discipline.
- **Break It:** give it a misleading source; see whether it notices conflicts.
- **Boss Check:** every claim in the report is traceable to a source.

## Module 3: The Workshop (Tool manipulation and orchestration)

### L7. The Tool-Using Agent: *"The Handyman's Toolbox"*
The town handyman needs one assistant that can check the calendar, look up parts prices, do math, and send messages.
- **Concepts:** designing good tools (names, descriptions, errors), many-tool selection, MCP servers, parallel tool calls.
- **Break It:** give two tools confusingly similar names; return unhelpful error messages.
- **Boss Check:** picks the right tool across 25 mixed requests.

### L8. The Chain-of-Agents Orchestrator: *"The Byteville Gazette"*
The town newspaper needs a reporter agent, an editor agent, and a fact-checker agent, coordinated by an editor-in-chief.
- **Concepts:** orchestrator–workers, hand-offs, shared vs. private context, when multi-agent beats single-agent.
- **Break It:** let agents talk without a supervisor and watch the chaos.
- **Boss Check:** compare quality and cost against a single-agent baseline.

### L9. The Agentic Workflow System: *"The Permit Office Pipeline"*
Building permits follow a fixed process with a few judgment calls. Build a workflow graph where most steps are deterministic and only some use an LLM.
- **Concepts:** workflows vs. agents, graphs and state machines, checkpoints and resume, human approval nodes.
- **Break It:** crash mid-workflow and resume from the checkpoint.
- **Boss Check:** processes 10 permits end-to-end with correct approvals.

## Module 4: The Counting House (Data analysis and reasoning)

### L10. The Data Analysis Agent: *"Why Is the Bakery Losing Money?"*
Give the agent the bakery's sales CSV and a sandboxed Python tool, and let it investigate.
- **Concepts:** code-execution tools, sandboxing, iterative analysis, charts, explaining findings in plain English.
- **Break It:** dirty data (missing values, wrong dates); a misleading correlation.
- **Boss Check:** finds the planted root cause in the dataset.

### L11. The Verification and Validation Agent: *"The Fact-Checker"*
Before anything goes in the Gazette, it gets checked. Build an agent that verifies claims and validates outputs from other agents.
- **Concepts:** LLM-as-judge, evaluator–optimizer loops, rubric design, building the `evals` toolkit used for the rest of the course.
- **Break It:** a judge that is too lenient; a judge biased toward longer answers.
- **Boss Check:** agreement with human labels on a claim set.

### L12. The General Problem Solver: *"The Town Puzzle Hunt"*
The annual puzzle hunt has logic puzzles, riddles, and math problems. Build an agent that chooses a strategy per problem.
- **Concepts:** reasoning models and extended thinking, tree-of-thought style search, self-consistency (sample and vote), knowing when to use code instead of reasoning.
- **Break It:** puzzles that need backtracking; trick questions.
- **Boss Check:** score on a mixed puzzle set within a token budget.

## Module 5: The Garage (Software development)

### L13. The Code-Generation Agent: *"Build the Town Website"*
The council wants a simple website. Build an agent that writes code, runs tests, reads errors, and fixes its own bugs.
- **Concepts:** write, run, fix loops, test-driven generation, file-editing tools, working in a repo.
- **Break It:** vague specs; a failing test it cannot see.
- **Boss Check:** passes a hidden test suite.

### L14. The Security-Hardened Agent: *"Someone Is Messing With the Website"*
A prankster hides instructions in a web page the agent reads. Harden the agent.
- **Concepts:** prompt injection (direct and indirect), least privilege, sandboxing, input and output filtering, approval gates, red-teaming your own agent.
- **Break It:** this whole lesson is Break It. Learners attack their L13 agent first, then defend it.
- **Boss Check:** survives a red-team suite of injection attempts.

### L15. The Self-Improving Agent: *"Practice Makes Perfect"*
An agent that gets better at a task by reviewing its own failures and updating its prompts or notes.
- **Concepts:** reflection, learning from feedback, prompt optimization against evals, the risk of overfitting to your tests.
- **Break It:** let it optimize against a flawed eval and see it game the metric.
- **Boss Check:** measurable improvement from round 1 to round 5 on a held-out set.

## Module 6: The Town Square (Conversational and content creation)

### L16. The Conversational Agent: *"The Town Hall Help Desk"*
A friendly front desk that answers resident questions, handles small talk, and escalates when stuck.
- **Concepts:** multi-turn dialog, persona and tone, clarifying questions, escalation to a human, streaming responses.
- **Break It:** rude users, off-topic users, users who change their mind mid-conversation.
- **Boss Check:** scripted conversation tests with an LLM-simulated user.

### L17. The Content Creation Agent: *"Festival Marketing Blitz"*
Posters, social posts, a radio jingle script, all on-brand.
- **Concepts:** style guides as context, outline then draft then edit, generating variations, brand consistency checks.
- **Break It:** contradictory brand rules; plagiarism-style output.
- **Boss Check:** judge scores for brand voice and factual accuracy.

### L18. The Recommendation Agent: *"The Bookshop's Picks"*
The bookshop wants personalized picks that explain themselves.
- **Concepts:** combining classic recommenders (similarity, collaborative filtering) with LLM reasoning, cold start, explanations, avoiding filter bubbles.
- **Break It:** a brand-new user with no history.
- **Boss Check:** hit rate on held-out purchases plus explanation quality.

## Module 7: The Senses (Multi-modal perception)

### L19. The Vision-Language Agent: *"The Lost and Found"*
Residents upload photos of lost items; the agent describes, tags, and matches them.
- **Concepts:** image inputs, visual question answering, image-text matching, describing charts and screenshots.
- **Break It:** low-light photos; look-alike objects.
- **Boss Check:** matching accuracy on a photo set.

### L20. The Audio Processing Agent: *"The Council Meeting Minutes"*
Turn a recorded council meeting into minutes, action items, and a "who said what" summary.
- **Concepts:** speech-to-text, speaker diarization, summarization of long transcripts, text-to-speech for a voice reply.
- **Break It:** crosstalk and background noise.
- **Boss Check:** action items match a labeled reference.

### L21. The Physical World Sensing Agent: *"The Smart Garden"*
The community garden has (simulated) soil moisture, temperature, and light sensors. Build an agent that monitors, detects anomalies, and decides when to water.
- **Concepts:** streaming sensor data, thresholds vs. learned anomalies, acting on a schedule, safe defaults when sensors fail.
- **Break It:** a sensor that gets stuck at one reading.
- **Boss Check:** keeps the simulated plants alive for 30 simulated days.

## Module 8: The Courthouse Steps (Ethical and explainable AI)

### L22. The Ethical Reasoning Agent: *"The Parking Dilemma"*
The council must allocate scarce parking permits fairly. Build an agent that reasons about trade-offs and stakeholders, and flags when a decision needs a human.
- **Concepts:** value frameworks, stakeholder analysis, fairness checks, refusing or escalating, constitutions and policies as prompts.
- **Break It:** a request that is legal but unfair; manipulative framing.
- **Boss Check:** consistent reasoning across reframed versions of the same scenario.

### L23. The Explainable Agent: *"Why Did the Robot Say No?"*
Residents deserve to know why an agent decided something. Add explanations and audit trails to an earlier agent.
- **Concepts:** traces, decision logs, faithful vs. plausible explanations, counterfactuals ("you would have qualified if..."), the `trace` toolkit.
- **Break It:** catch an explanation that does not match what the agent actually did.
- **Boss Check:** explanations agree with the recorded trace.

## Module 9: Main Street Professionals (Domain agents)

These lessons use **synthetic data** and come with clear "educational, not professional advice" framing. A recurring theme is knowing the limits: when the agent must defer to a human expert.

### L24. The Healthcare Intelligence Agent: *"The Clinic Front Desk"*
Triage appointment requests, summarize (fake) patient histories for the doctor, and answer general wellness questions with appropriate caution.
- **Concepts:** high-stakes domains, conservative defaults, escalation rules, privacy and de-identification, grounding in vetted sources.
- **Boss Check:** never gives a diagnosis; always escalates red-flag symptoms.

### L25. The Scientific Discovery Agent: *"The Mystery of the Glowing Pond"*
Generate hypotheses, design (simulated) experiments, run them, and update beliefs.
- **Concepts:** hypothesis generation, experiment loops, tracking evidence, the difference from L6 (finding knowledge vs. creating it).
- **Boss Check:** identifies the hidden rule in a simulated environment in limited experiments.

### L26. The Financial Advisory Agent: *"The Town Budget"*
Help residents understand budgets and the town analyze spending, with calculations done by tools rather than by the model's guesswork.
- **Concepts:** numeric accuracy via code, scenario analysis, risk disclosures, compliance-style guardrails.
- **Boss Check:** every number in the output is tool-computed and correct.

### L27. The Legal Intelligence Agent: *"The Ordinance Explainer"*
Search Byteville's (fictional) ordinances, explain them in plain English, and compare clauses in contracts.
- **Concepts:** precise retrieval, quoting exact text, clause comparison, jurisdiction and uncertainty, "consult a lawyer" boundaries.
- **Boss Check:** answers quote the correct ordinance section.

## Module 10: The Schoolhouse (Education and collective intelligence)

### L28. The Education Intelligence Agent: *"The Patient Tutor"*
A tutor for Byteville's kids that teaches with questions instead of just giving answers, and adapts to each student.
- **Concepts:** Socratic prompting, learner models, adaptive difficulty, tracking mastery in memory, safety for young users.
- **Boss Check:** guides a simulated student to the answer without giving it away.

### L29. The Collective Intelligence Agent: *"The Town Hall Debate"*
A panel of agents with different viewpoints debates a proposal and produces a balanced recommendation.
- **Concepts:** debate, voting and aggregation, diversity of perspectives, avoiding groupthink, cost vs. benefit of more agents.
- **Boss Check:** the panel beats a single agent on a set of judgment questions.

---

## Capstone. L30. The Embodied Intelligence Agent: *"Robo, the Town Helper"*

Byteville gets a (simulated) robot. Robo sees its surroundings, plans routes, remembers where things are, uses tools, follows safety rules, explains its choices, and can ask other town agents for help.

- **Concepts:** perception, planning, and action in a simulated 2D or 3D world; safety constraints on physical actions; integrating everything from the course.
- **The finale:** Robo completes a multi-step errand across town that requires calling at least five earlier agents (Librarian, Fact-Checker, Clinic, Gazette, Mayor).
- **Boss Check:** errand succeeds with no safety violations and a full explanation trace.

---

## Decisions needed

These shape every lesson, so they are worth settling before any code is written. Recommendations are in bold.

| Question | Options | Recommendation |
|---|---|---|
| **Language** | Python, TypeScript | **Python.** Largest AI ecosystem, easiest for data, vision, and audio lessons, most familiar to learners. |
| **Framework approach** | Raw SDK only; one framework throughout; raw first, then a framework | **Raw SDK for Modules 1 to 3 (build `bytekit` by hand), then optionally show the same agent in a popular framework** so learners see what frameworks do for them. |
| **Model provider** | Anthropic (Claude), OpenAI, Google, local (Ollama), provider-agnostic | **Provider-agnostic thin wrapper with Claude as the default,** plus a local-model option so learners can follow along for free. |
| **Format** | Markdown chapters, Jupyter notebooks, video plus repo | **Markdown lessons plus a runnable folder per lesson**, with notebooks for data-heavy lessons. |
| **Audience** | Beginners to Python, working developers, ML engineers | **Working developers who know basic Python but are new to agents.** |
| **Scope of v1** | All 30 lessons, or Modules 1 to 3 first | **Ship Lessons 0 to 9 first,** get feedback, then continue. |

## Proposed repo layout (once decisions are made)

```
ai_agents/
├── PRIMER.md
├── CURRICULUM.md
├── bytekit/              # the toolkit learners grow lesson by lesson
├── lessons/
│   ├── 00-welcome/
│   ├── 01-mayor-decides/
│   │   ├── README.md     # the lesson text
│   │   ├── start/        # starter code with TODOs
│   │   ├── solution/
│   │   └── evals/        # the Boss Check
│   └── ...
└── data/                 # synthetic datasets for Byteville
```

## A note on originality

The agent list follows a widely used 30-agent taxonomy as a topic outline. All lesson stories, projects, code, and explanations in this course are original.
