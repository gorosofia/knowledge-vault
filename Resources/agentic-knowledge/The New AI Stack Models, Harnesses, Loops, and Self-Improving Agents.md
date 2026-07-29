---
title: "The New AI Stack: Models, Harnesses, Loops, and Self-Improving Agents"
source: "https://x.com/sairahul1/status/2074427867329380359"
author:
  - "[[@sairahul1]]"
published: 2026-07-07
created: 2026-07-28
description: "Everyone is talking about AI models.Nobody is talking about the layer that actually makes them useful.Claude Code. Codex. Cursor.These are n..."
tags:
  - "clippings"
---
![Image](https://pbs.twimg.com/media/HMnZIIabAAEUZij?format=jpg&name=large)

Everyone is talking about AI models.

Nobody is talking about the layer that actually makes them useful.

Claude Code. Codex. Cursor.

These are not just models.

They are models wrapped in a system.

That system is called a harness.

And the best harnesses now improve themselves.

Here is everything you need to understand about the new AI stack.

**The lie everyone believes about AI products**

![Image](https://pbs.twimg.com/media/HMnS-y-aYAAgvAT?format=jpg&name=large)

Most people think AI progress = smarter models.

It is not.

The model is one part of the stack.

The architecture is published. Everyone copies the same transformer. Every lab uses the same building blocks.

What actually separates Claude Code from a weekend project is not the model.

It is what surrounds the model.

The harness.

In 2017, AI progress was about attention mechanisms. In 2020, it was about scale. In 2026, it is about harness engineering.

And harnesses are now being designed by AI, not humans.

## What is a harness?

![Image](https://pbs.twimg.com/media/HMnTYq8aAAAXxL3?format=jpg&name=large)

A harness is the system surrounding a model.

It decides:

→ How the model thinks and plans

→ When it calls tools and what it does with results

→ What it remembers across steps

→ How it stores artifacts and manages state

→ How it evaluates its own output

→ When it loops back and tries again

Think of it like an operating system.

The model is the CPU. The harness is the OS.

You can have a powerful CPU and terrible software and ship nothing useful. You can have a modest CPU and excellent software and ship something great.

The most successful coding agents — Claude Code, Codex, Cursor — all have the same insight:

**The loop matters as much as the model.**

## The 3 harness patterns every AI builder needs

Every production AI system uses at least one of these.

## Pattern 1: The Loop

![Image](https://pbs.twimg.com/media/HMnT2-MbMAAgCwQ?format=jpg&name=large)

The model does not answer once and stop.

It loops.

Plan → Execute → Observe → Improve → Repeat

This is the core of every coding agent.

A simplified Claude Code loop:

1\. Read the task 2. Plan the approach 3. Write code → run it 4. See what failed 5. Fix it 6. Run again 7. Repeat until tests pass

The model is not smarter on loop 3 than loop 1.

But the system is.

Each loop gives the model new context — error messages, test results, execution traces.

The output of loop 1 becomes the input of loop 2.

That compound context is why agentic systems outperform single-shot prompting on complex tasks.

**The key insight:** The model stays fixed. The context gets smarter.

## Pattern 2: File System as Memory

![Image](https://pbs.twimg.com/media/HMnT9EiaIAAWfMG?format=jpg&name=large)

Most developers stuff everything into the context window.

This is a trap.

Long-horizon tasks generate:

→ Experiment logs

→ Code diffs → Error traces

→ Past rollout histories → Paper summaries → Intermediate artifacts

All of that grows way past any context window.

The solution: write to files, not to context.

```plaintext
# Bad: everything in context
context = previous_output + tool_result + error_log + history...
# Blows up at step 47

# Good: use the file system
agent.write("experiments/run_3/error_log.txt", error_trace)
agent.write("experiments/run_3/results.json", metrics)

# Later, agent reads only what it needs
relevant = agent.read("experiments/run_3/results.json")
```

This changes everything about long-horizon tasks.

→ Agent can resume after crashes

→ Agent can reason over its own execution history

→ Context stays clean even on step 200

→ Multiple sub-agents can share state via files

The best agents treat the file system like a second brain.

Not a dump. A structured memory.

## Pattern 3: Sub-agents

![Image](https://pbs.twimg.com/media/HMnUCa5aYAA1Fw7?format=jpg&name=large)

One agent cannot do everything.

The best systems spawn parallel sub-agents.

The parent agent:

→ Breaks the task into independent subtasks

→ Launches sub-agents to run them in parallel

→ Monitors their status → Merges their results back

Example for a research harness:

```plaintext
Parent agent receives: "Write a full competitive analysis report"

Spawns 4 sub-agents simultaneously:
→ Sub-agent 1: Research competitor A's pricing and features
→ Sub-agent 2: Research competitor B's pricing and features
→ Sub-agent 3: Search recent news about both competitors
→ Sub-agent 4: Pull user reviews from Reddit and App Store

Parent waits, then merges all 4 outputs into final report

Total time: same as the slowest sub-agent (not 4x longer)
```

The key design rule: sub-agent outputs must go to files.

Not transient context. Files.

If they only live in context, they disappear when the sub-agent session ends.

If they live in files, the parent agent can inspect them, the system can recover from crashes, and everything is auditable.

## Tools every coding agent uses

If you are building an agent, this is the toolkit that every major coding agent standardizes on.

```plaintext
File System Tools:
→ glob, grep, ls          # find files
→ read, read_many         # read content
→ write                   # create new file
→ edit                    # string-replace edit
→ apply_patch             # structured diff

Shell Tools:
→ bash                    # run any command
→ PowerShell              # Windows equivalent

Version Control:
→ git_status, git_diff    # inspect changes
→ git_commit              # save progress

Agent Management:
→ spawn_agent             # launch sub-agent
→ wait_agent              # wait for result
→ list_agents             # see what's running
→ interrupt_agent         # cancel if needed

External Context:
→ web_search, web_fetch   # get current info
→ MCP tools               # connect to external services
```

You do not need all of these for every agent.

But every production agent eventually needs most of them.

The ones that matter most early: bash, read, write, edit.

Master those four and you can build almost anything.

## Context engineering: the skill nobody talks about

![Image](https://pbs.twimg.com/media/HMnUNPubsAA_SUt?format=jpg&name=large)

The model is fixed.

You cannot change its weights at runtime.

But you can change what it sees.

That is context engineering.

And it is now one of the highest-leverage skills in AI engineering.

Bad context:

→ Dump everything in → hope for the best

→ Context bloats → model loses focus → outputs degrade

Good context:

→ Structured. Concise. Evolving.

→ Right information at the right step.

→ Previous failures inform current attempt.

The state-of-the-art approach (ACE — Agentic Context Engineering):

```plaintext
3 components:

Generator:   runs the task, references a structured context playbook
Reflector:   analyzes successes and failures, distills insights
Curator:     updates the playbook with new learnings — adds, removes, deduplicates

The playbook is NOT a prompt blob.
It is a structured list of (identifier, insight) pairs.

Example:
{
  "id": "001",
  "insight": "Always write error traces to file before retrying."
},
{
  "id": "002", 
  "insight": "Sub-agent for web search returns better results with site-specific queries."
},
{
  "id": "003",
  "insight": "Running tests before committing catches 80% of regressions."
}
```

The playbook updates after every run.

The agent that runs task 50 is working with 49 runs of distilled learnings.

The agent that runs task 1 had nothing.

**This is how a system gets smarter without touching model weights.**

## The harness that improves itself

![Image](https://pbs.twimg.com/media/HMnUXlmbAAAAwmE?format=jpg&name=large)

This is where it gets wild.

What if the harness itself was the thing being optimized?

Not the prompt. Not the model.

The code that runs the agent.

This is exactly what Self-Harness does.

3-step loop:

**Step 1 — Mine weaknesses**

Run the current harness on a set of tasks. Collect failure traces. Cluster failures by root cause.

Not "it failed." But why it failed.

Failure types discovered: → "Agent times out on long file reads" → "Sub-agent outputs lost when parent crashes" → "Error messages not informative enough to self-correct" → "Context grows too large after step 30, model loses focus"

**Step 2 — Propose fixes**

The same model looks at the failure patterns. Proposes specific, narrow edits to the harness code.

Not rewrites. Targeted edits.

Proposed harness edit: → Add timeout handler to file read operations → Auto-flush sub-agent output to disk on every step (not just at end) → Standardize error message format to include: step, tool, input, output, failure reason → Add context compression step every 25 turns

**Step 3 — Validate and merge**

Each proposed edit gets tested on held-out tasks.

Does it fix the weakness without breaking anything else?

If yes: merged into the harness. If no: logged, rejected, harness unchanged.

**The result: the harness gets better with every generation.**

Claude 3.5 Sonnet running Self-Harness went from 20% to 50% on SWE-bench Verified.

Not from a better model.

From a better system.

## Evolutionary harness search

Self-Harness fixes one harness iteratively.

AlphaEvolve runs a population of harnesses and evolves the best ones.

The algorithm:

```plaintext
1. Start with a pool of harness candidates
2. Score each one on benchmark tasks
3. Select the best performers as "parents"
4. Ask the model to propose diffs/improvements
5. Generate new "child" harnesses
6. Score children
7. Keep the ones that improve
8. Add them back to the pool
9. Repeat

(Same logic as natural selection. Applied to code.)
```

(Same logic as natural selection. Applied to code.)

One key design detail from AlphaEvolve:

Code regions eligible for evolution are explicitly marked:

```plaintext
# EVOLVE-BLOCK-START
def plan_next_step(context, tools):
    # This section can be modified by the evolutionary search
    prompt = f"Given: {context}\nAvailable tools: {tools}\nNext action:"
    return llm.generate(prompt)
# EVOLVE-BLOCK-END

# The rest of the harness stays fixed
def run_tool(tool_name, args):
    return tool_registry[tool_name](**args)
```

This containment prevents the agent from accidentally modifying safety-critical code.

The evolutionary search only touches what you explicitly allow it to touch.

AlphaEvolve used this to optimize matrix multiplication algorithms.

The result: beat DeepMind's hand-optimized code.

The agent found solutions human engineers had not discovered in decades.

## Darwin Gödel Machine: agents that rewrite themselves

![Image](https://pbs.twimg.com/media/HMnUdnMbUAEK3mN?format=jpg&name=large)

The most extreme version of this idea.

An agent that modifies its own harness code to get better at tasks.

Darwin Gödel Machine (DGM):

```plaintext
1. Start with one coding agent in the pool
2. Run it on benchmarks, collect scores
3. Agent examines its own evaluation logs
4. Agent proposes improvements to its own harness code
5. Tools available: bash + file editor (view/create/edit files)
6. New version of agent is created
7. New version is scored
8. If better: added to pool
9. If worse: discarded
10. Repeat — selecting parents by performance, inversely by offspring count
```

Starting condition: Claude 3.5 Sonnet + simple initial harness.

**Result:**

→ SWE-bench Verified: 20% → 50%

→ Polyglot coding benchmark: 14.2% → 30.7%

Zero changes to model weights. Zero human engineering between generations.

The agent designed better versions of itself.

This is not science fiction.

This is a paper from 2025.

## 5 failure modes to avoid

These are the ways real research teams have failed.

All documented. All avoidable.

**1\. Context collapse**

Long-horizon tasks lose critical details if logs are not written as persistent artifacts.

Fix: write everything important to files. Never rely on context alone past step 20.

**2\. Implementation drift**

When the task gets technically hard, the model drifts toward easier, more common solutions instead of the actual goal.

Fix: write a spec file at the start. Agent checks the spec on every loop.

**3\. Over-optimism**

The model declares success despite failed experiments.

It finds "numerical duct tape" — patches that make metrics look good without solving the real problem.

Fix: hold out a test set the agent never sees. Validate only on held-out data.

**4\. Reward hacking**

Agent optimizes whatever signal it is given.

If the signal is unit tests — it writes tests that always pass. If the signal is a judge model — it learns tricks to fool the judge. If the signal is benchmark score — it exploits benchmark artifacts.

Fix: the evaluator lives outside the loop. Human review at key decision points.

**5\. Diversity collapse**

Evolutionary loops converge on one strategy.

Every generation looks like a variant of the same solution.

Fix: explicitly track novelty. Penalize solutions too similar to existing pool members (embedding-based cosine similarity works).

## The new AI stack in plain English

![Image](https://pbs.twimg.com/media/HMnUjbwbkAAIWF8?format=jpg&name=large)

This is what you are actually building when you build serious AI products:

**Layer 1 — The Model**

Raw intelligence. Pretrained. Fixed weights at runtime.

This is the CPU. Powerful but passive.

**Layer 2 — The Harness**

The OS. Wraps the model. Orchestrates everything.

→ Tools (bash, file read/write, web search)

→ Memory (file system, structured logs)

→ Loop (plan → execute → evaluate → retry) → Sub-agents (parallel execution)

→ Context management (what the model sees at each step)

**Layer 3 — The Optimizer**

The harness that improves the harness.

→ Mines failure patterns from execution traces

→ Proposes targeted edits to harness code → Validates on held-out tasks

→ Merges improvements, discards regressions

**Layer 4 — The Evaluator**

Lives outside all other layers.

→ Benchmark scores → Human review at key decision points → Held-out test sets the optimizer never touches

You cannot skip any layer.

Skip Layer 2 — your model is a chatbot, not a product.

Skip Layer 3 — your system never gets better without manual engineering.

Skip Layer 4 — your agent optimizes the wrong thing and you won't notice.

## What this means for builders right now

You do not need to build a self-improving harness to benefit from these ideas.

Start here:

**Week 1: Build the loop**

Stop building single-shot prompts. Build a plan → execute → evaluate → retry loop for any task that takes more than one step.

**Week 2: Add persistent memory**

Stop relying on context. Write intermediate outputs to files. Let the agent read its own previous work.

**Week 3: Add sub-agents**

Identify any part of your workflow that can run in parallel. Spawn sub-agents. Write their outputs to files. Merge.

**Week 4: Add context engineering**

Track what patterns lead to success and failure. Build a simple structured playbook that updates after each run.

That is the harness.

Not the model.

The model is already there.

The harness is what you build.

## The uncomfortable truth about AI in 2026

The research acceleration at frontier labs has drastically increased.

Anthropic and OpenAI are shipping faster than ever.

Not because the models got smarter overnight.

Because the harnesses got better.

An agent that loops, remembers, sub-delegates, and self-corrects outperforms a smarter model used wrong.

The moat is not the model.

The moat is the system.

And the system can now improve itself.

If this was useful:

→ Repost — every AI builder needs to understand this stack

→ Follow [@sairahul1](https://x.com/@sairahul1) for more breakdowns like this

→ Bookmark — the 4-layer stack alone is worth saving

Subscribe to [theaibuilders.co](https://theaibuilders.co/) for more such interesting articles

I write about AI, building products, and systems that work without you.