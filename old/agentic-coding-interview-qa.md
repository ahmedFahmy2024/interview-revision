# AI-Native / Agentic Coding Interview --- Q&A Revision Guide

> Based on the supplied transcript of **"Software Engineering Interviews
> Are Changing"**.
>
> **Purpose:** Fast revision before software-engineering interviews,
> especially interviews at AI-native companies.
>
> **Note:** Product/model names below follow the transcript wording.
> Since AI tooling changes quickly, verify current model names and
> capabilities before an interview.

------------------------------------------------------------------------

## 1. Agentic Coding Fundamentals

### Q1. What is an agentic coding workflow?

**Answer:**\
An agentic coding workflow is a development process where an AI coding
agent does more than autocomplete code. The agent can inspect a
codebase, reason about a task, create a plan, edit files, run tools,
execute tests, review its implementation, and potentially create a pull
request.

The developer remains the **human in the loop**: defining requirements,
selecting the right model and effort level, reviewing plans, validating
output, and deciding what can safely be automated.

**Interview keywords:** coding agent, tools, planning, implementation,
validation, human-in-the-loop, automation.

------------------------------------------------------------------------

### Q2. Why should engineers experiment with AI coding tools instead of only using Copilot?

**Answer:**\
The transcript argues that AI-native companies may expect engineers to
understand modern agentic workflows, not only autocomplete.
Experimenting with multiple tools helps you understand their different
models, planning capabilities, tool use, context management, sub-agents,
skills, and automation workflows.

The important point is **not knowing every tool**. It is showing that
you actively experiment and can explain why you choose a particular
workflow.

------------------------------------------------------------------------

### Q3. What coding-agent harnesses should I know about?

**Answer:**\
The transcript mentions examples such as:

-   Claude Code
-   Codex
-   Cursor
-   Pi
-   Muse
-   Grok Build
-   OpenCode

You do **not** need to master all of them. You should have practical
experience with at least some of them and be able to explain your
workflow.

------------------------------------------------------------------------

### Q4. What is a "harness" in agentic coding?

**Answer:**\
A harness is the environment around the model that gives it instructions
and capabilities such as codebase access, file editing, tool execution,
planning, context management, permissions, agents, skills, and
integrations.

The underlying model provides intelligence, while the harness determines
much of **how that intelligence interacts with your development
environment**.

------------------------------------------------------------------------

## 2. Models and Model Selection

### Q5. Why does model selection matter?

**Answer:**\
Different models can have different trade-offs in reasoning quality,
speed, cost, context handling, and ability to complete long-running
tasks.

A strong workflow does not automatically use the most expensive model
for everything. Instead, you select a model according to the complexity
and risk of the task.

------------------------------------------------------------------------

### Q6. How would you choose between a powerful model and a cheaper/faster model?

**Answer:**\
I would use a stronger model for complex, ambiguous, long-running, or
high-risk work where maintaining context and reasoning quality matters.

For smaller implementation tasks or well-defined work, I can use a
faster/cheaper model to reduce token cost and latency.

A useful pattern from the transcript is:

**Strong model → planning/orchestration**\
**Cheaper capable model → implementation sub-agents**

This lets you balance quality and cost.

------------------------------------------------------------------------

### Q7. What is an orchestrator model?

**Answer:**\
An orchestrator is the agent/model responsible for understanding the
overall feature, creating the plan, decomposing the work, and delegating
parts of it to other agents.

For example:

``` text
User requirement
      ↓
Orchestrator
      ↓
Plan + task decomposition
      ↓
┌─────────┬─────────┬─────────┐
│ Agent A │ Agent B │ Agent C │
└─────────┴─────────┴─────────┘
      ↓
Review / integration
      ↓
Final implementation
```

The orchestrator may use a stronger model, while implementation agents
may use cheaper models.

------------------------------------------------------------------------

### Q8. What are sub-agents?

**Answer:**\
Sub-agents are additional agents that an orchestrator can delegate
specific tasks to.

For example, a feature could be divided into:

-   UI implementation
-   API integration
-   tests
-   security review
-   documentation

These can potentially run independently or in parallel.

------------------------------------------------------------------------

## 3. Effort Levels

### Q9. What is an effort level?

**Answer:**\
Effort level controls how much reasoning/computation the model spends on
a task.

Lower effort is useful when you need a quick answer and the problem is
simple or already well understood. Higher effort is more appropriate
when accuracy, complexity, or risk increases.

------------------------------------------------------------------------

### Q10. When would you use low effort?

**Answer:**\
For quick, low-risk questions where I already know approximately where
the answer exists.

Example:

> "Explain how the top project bar works."

If I know the relevant area of the codebase and only need a quick
explanation, a lower effort level can save time and tokens.

However, I would treat the result with more caution because lower effort
may increase the chance of an incomplete or incorrect answer.

------------------------------------------------------------------------

### Q11. When would you use high or extra-high effort?

**Answer:**\
I would increase effort for tasks such as:

-   complex feature planning
-   large refactors
-   high-risk changes
-   changes spanning many files
-   integrations involving several systems
-   security-sensitive work
-   situations where repeated failure would cost more than spending
    extra tokens initially

The transcript gives an example of changes touching roughly **50--100
files** as the kind of task where higher effort may be justified.

------------------------------------------------------------------------

### Q12. Why not always use maximum effort?

**Answer:**\
Because there can be diminishing returns. Maximum reasoning may consume
significantly more tokens and time without producing proportionally
better results.

The goal is to choose enough reasoning for the risk and complexity of
the task.

------------------------------------------------------------------------

## 4. Planning and Plan Mode

### Q13. What is Plan Mode?

**Answer:**\
Plan Mode is a workflow where the agent investigates the codebase and
requirements before implementing changes.

Instead of immediately editing code, it first tries to understand:

1.  what currently exists,
2.  what the user wants,
3.  which parts of the system are affected,
4.  what uncertainties need clarification,
5.  how the change should be implemented.

The developer then reviews and iterates on the plan before
implementation.

------------------------------------------------------------------------

### Q14. What happens conceptually under the hood in Plan Mode?

**Answer:**\
According to the transcript, the harness uses internal/system
instructions that guide the model to explore the codebase, gather
relevant information, and form a plan before implementation.

The important interview concept is that Plan Mode is **not magic**. It
is a structured agent workflow driven by instructions and tools.

------------------------------------------------------------------------

### Q15. When should you use Plan Mode?

**Answer:**\
Good cases include:

-   large features
-   unclear requirements
-   architecture changes
-   high-risk work
-   large refactors
-   changes involving several systems

It is especially useful when you want the AI to ask questions before
coding.

------------------------------------------------------------------------

### Q16. What is the human's role during planning?

**Answer:**\
The human should review the plan, answer clarification questions,
correct assumptions, modify requirements, and approve the direction.

A good workflow is:

``` text
Requirement
   ↓
Agent explores codebase
   ↓
Agent asks questions
   ↓
Human clarifies
   ↓
Agent creates plan
   ↓
Human reviews / modifies
   ↓
Implementation
```

------------------------------------------------------------------------

### Q17. Do you need Plan Mode for every task?

**Answer:**\
No. Small and clearly defined tasks may not justify a separate planning
phase.

For a simple UI adjustment, a precise prompt may be enough. Planning
becomes more valuable as ambiguity, scope, and risk increase.

------------------------------------------------------------------------

### Q18. Can you use your own planning file instead of built-in Plan Mode?

**Answer:**\
Yes. The transcript mentions that you could maintain your own `plan.md`
or another planning workflow instead of relying entirely on the
harness's built-in Plan Mode.

The important concept is the **planning discipline**, not only the
specific feature name.

------------------------------------------------------------------------

## 5. Auto Mode and Tool Safety

### Q19. What is Auto Mode?

**Answer:**\
In the transcript, Auto Mode is described as a mechanism that sits
between the agent and tool calls and determines whether requested
operations are safe enough to execute automatically.

The agent may want to:

-   read/write files,
-   access the disk,
-   run commands,
-   interact with GitHub,
-   or invoke other tools.

Auto Mode can classify potentially dangerous operations before allowing
them.

------------------------------------------------------------------------

### Q20. Why is tool-call safety important?

**Answer:**\
Coding agents can execute real actions, not merely produce text.

A badly interpreted instruction could potentially delete files, modify
infrastructure, or perform another destructive operation. Therefore,
permission boundaries and safety checks are important parts of an
agentic workflow.

------------------------------------------------------------------------

## 6. Prompt Engineering for Coding Agents

### Q21. What makes a good coding-agent prompt?

**Answer:**\
The transcript recommends clearly communicating:

1.  **Where** the change is.
2.  **What** should change.
3.  **How it behaves now.**
4.  **How it should behave after the change.**
5.  Relevant constraints.

Example structure:

``` text
WHERE:
The left sessions panel.

CURRENT BEHAVIOR:
The grid can display three or four columns.

DESIRED BEHAVIOR:
It should display a maximum of two columns.

CONSTRAINT:
Never allow the grid to exceed two columns.
```

This reduces ambiguity.

------------------------------------------------------------------------

### Q22. Why is visual/contextual prompting useful?

**Answer:**\
Describing the visible UI and current behavior gives the agent more
context about exactly what needs to change.

Instead of:

> "Fix the grid."

Say:

> "In the left sessions panel, session cards currently expand to three
> or four columns. Cap the grid at two columns."

The second prompt gives location, current state, and expected state.

------------------------------------------------------------------------

### Q23. Why did the video mention voice prompting?

**Answer:**\
The speaker uses voice-to-text tooling such as SuperWhisper because
speaking a detailed prompt can be faster than manually typing it.

The broader interview point is not the specific tool. It is
demonstrating that you experiment with workflows that improve
development efficiency.

------------------------------------------------------------------------

## 7. Skills

### Q24. What is a skill in an AI coding workflow?

**Answer:**\
A skill is a reusable workflow or set of instructions that encapsulates
a repeated process.

Instead of repeatedly writing the same long prompt, you package the
process so that you and your team can invoke it consistently.

------------------------------------------------------------------------

### Q25. When should you create a skill?

**Answer:**\
A good rule from the transcript is:

> If you repeatedly perform the same workflow or repeatedly write the
> same prompt, consider turning it into a skill.

Good examples include:

-   creating pull requests,
-   reviewing pull requests,
-   deployment,
-   releases,
-   security review,
-   deep implementation review.

------------------------------------------------------------------------

### Q26. Why should skills be committed to the repository?

**Answer:**\
Because team-level skills can standardize workflows.

If ten engineers use the same PR skill, for example, pull-request
descriptions can follow a consistent format regardless of who creates
them.

This converts individual AI usage into a **shared engineering
workflow**.

------------------------------------------------------------------------

### Q27. Give an example of a PR skill.

**Answer:**

A PR skill could instruct the agent to produce:

``` text
## Summary
What changed?

## Motivation
Why was the change needed?

## Implementation
How was it implemented?

## Risk
What could break?

## Testing
What was tested?

## Screenshots
Include UI screenshots if applicable.
```

The key idea is consistency and repeatability.

------------------------------------------------------------------------

### Q28. Give an example of a PR-review skill.

**Answer:**\
A PR-review skill could require the agent to inspect:

-   correctness,
-   regressions,
-   security risks,
-   performance impact,
-   architecture consistency,
-   edge cases,
-   missing tests.

This makes code review more systematic.

------------------------------------------------------------------------

### Q29. Give an example of a release skill.

**Answer:**\
A release skill might document:

1.  run tests,
2.  build the application,
3.  update the version,
4.  create the Git tag,
5.  create the GitHub release,
6.  trigger the required GitHub Action,
7.  verify deployment.

Again, the value is turning a repeated process into a reusable workflow.

------------------------------------------------------------------------

## 8. CLAUDE.md / AGENTS.md

### Q30. What is `CLAUDE.md` or `AGENTS.md`?

**Answer:**\
It is a repository-level instruction file used to provide persistent
guidance to coding-agent sessions.

It can contain project-specific rules such as:

-   architecture conventions,
-   commands,
-   testing expectations,
-   coding standards,
-   repository structure,
-   completion requirements.

Conceptually:

``` text
Every agent session
       ↓
Loads project instructions
       ↓
Agent works using shared rules
```

------------------------------------------------------------------------

### Q31. What would you put in an `AGENTS.md` / `CLAUDE.md` file?

**Answer:**\
Examples from the transcript include instructions such as:

``` md
- Verify that all code passes tests before considering implementation complete.
- Follow the repository architecture conventions.
- Run required validation before completion.
```

The file should contain guidance that is useful broadly across sessions.

------------------------------------------------------------------------

### Q32. Why shouldn't you put everything into `AGENTS.md` / `CLAUDE.md`?

**Answer:**\
Because persistent instructions consume context/tokens and may be
applied even when they are unnecessary.

For example, forcing multiple adversarial reviews for every simple
question could waste time and tokens.

A better rule is:

**Always-needed project guidance → `AGENTS.md` / `CLAUDE.md`**\
**Optional/repeatable heavy workflow → Skill**

------------------------------------------------------------------------

### Q33. What is the difference between a skill and `AGENTS.md` / `CLAUDE.md`?

  -----------------------------------------------------------------------
  `AGENTS.md` / `CLAUDE.md`           Skill
  ----------------------------------- -----------------------------------
  Persistent project guidance         Invoked workflow

  Relevant across many sessions       Used when needed

  Good for coding/project rules       Good for repeated procedures

  Adds persistent context             Avoids loading unnecessary
                                      instructions every time
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 9. MCP Servers

### Q34. What is an MCP server?

**Answer:**\
In the workflow described by the transcript, an MCP server allows the
coding agent to interact with external services and tools through a
standardized integration.

Examples mentioned include integrations with:

-   Sentry
-   Stripe
-   Railway
-   Gmail
-   other third-party services

------------------------------------------------------------------------

### Q35. Give a practical MCP example using Sentry.

**Answer:**\
You could ask the coding agent to use the Sentry MCP integration to
retrieve recent errors and triage them.

A workflow might look like:

``` text
Agent
  ↓
Sentry MCP
  ↓
Fetch recent errors
  ↓
Analyze / group errors
  ↓
Check whether already documented
  ↓
Create GitHub issues for new problems
```

This removes manual UI work from part of the debugging workflow.

------------------------------------------------------------------------

### Q36. Give a practical MCP example using Stripe.

**Answer:**\
The transcript describes using a Stripe MCP integration so an agent
could interact with Stripe-related configuration, such as creating or
configuring products and their descriptions.

The broader idea is allowing the agent to operate across external
development services instead of forcing the developer to manually click
through every UI.

------------------------------------------------------------------------

### Q37. Why are MCP integrations useful?

**Answer:**\
They extend the agent beyond the local codebase.

Instead of only writing code, the agent can potentially gather
information or perform permitted operations in external systems involved
in the engineering workflow.

------------------------------------------------------------------------

## 10. Context Windows and Tokens

### Q38. What is a context window?

**Answer:**\
The context window is the amount of information the model can actively
work with during a session.

It may contain things such as:

-   system prompts,
-   conversation history,
-   code,
-   tool definitions,
-   agent definitions,
-   memory files,
-   skills,
-   MCP-related information.

All of these compete for limited context.

------------------------------------------------------------------------

### Q39. Why should developers understand where their context is being used?

**Answer:**\
Because unnecessary instructions and integrations can consume a
significant portion of the available context before meaningful
development work even starts.

A crowded context can:

-   consume more tokens,
-   increase cost,
-   reduce available working context,
-   potentially distract/confuse the model,
-   make long tasks harder to manage.

------------------------------------------------------------------------

### Q40. What is compaction?

**Answer:**\
The transcript mentions compaction as part of managing a limited context
window.

Conceptually, when a conversation becomes too large, older information
can be compressed/summarized so the agent can continue working without
carrying every original token.

The trade-off is that some detail can potentially be lost during
compression.

------------------------------------------------------------------------

## 11. Skill Hygiene and MCP Hygiene

### Q41. What is skill hygiene?

**Answer:**\
Skill hygiene means periodically reviewing the skills available to the
agent and removing or updating ones that no longer provide enough value.

Questions to ask:

-   Do we still use this skill?
-   Is it outdated?
-   Does it duplicate another skill?
-   Is it unnecessarily consuming context?
-   Does it improve the workflow enough to keep?

------------------------------------------------------------------------

### Q42. What is MCP hygiene?

**Answer:**\
MCP hygiene means periodically reviewing enabled MCP integrations and
disabling/removing ones that are unnecessary.

The transcript warns that unused integrations can consume context/tokens
even when they are irrelevant to the current task.

------------------------------------------------------------------------

### Q43. Why does context hygiene matter more for smaller models?

**Answer:**\
Smaller models may have less usable context, so unnecessary tools,
skills, and instructions can occupy a larger percentage of the available
window.

The principle is:

> Keep enough context to accomplish the task, but avoid filling the
> context with irrelevant information.

------------------------------------------------------------------------

## 12. Git Worktrees and Parallel Agent Work

### Q44. What is a Git worktree?

**Answer:**\
A Git worktree allows you to have multiple working directories attached
to the same repository, usually with different branches checked out.

This is useful for agentic development because several independent tasks
can be worked on simultaneously without agents modifying the same
working directory.

------------------------------------------------------------------------

### Q45. How can worktrees be used with coding agents?

**Answer:**\
Suppose you have five independent GitHub issues.

You could create separate worktrees:

``` text
Repository
│
├── worktree/issue-101
├── worktree/issue-102
├── worktree/issue-103
├── worktree/issue-104
└── worktree/issue-105
```

Then assign an agent to each issue.

Each agent works in isolation and can potentially produce its own branch
and pull request.

------------------------------------------------------------------------

### Q46. Why is this better than running several agents in one working directory?

**Answer:**\
Isolation reduces conflicts.

If multiple agents edit the same working tree simultaneously, they may
overwrite or interfere with each other's changes.

Separate worktrees provide each task with its own filesystem/branch
context.

------------------------------------------------------------------------

## 13. Cloud Runners and Sandboxes

### Q47. What is a cloud/sandbox runner?

**Answer:**\
It is an isolated environment where an agent can work on a task
independently from your local development environment.

Instead of keeping five agent sessions running locally, you could send
individual issues to separate cloud/sandbox environments.

------------------------------------------------------------------------

### Q48. What could a cloud-agent workflow look like?

**Answer:**

``` text
GitHub Issue
     ↓
Cloud / Sandbox Agent
     ↓
Inspect repository
     ↓
Implement fix
     ↓
Run validation
     ↓
Create branch
     ↓
Open pull request
```

This can make parallel execution easier.

------------------------------------------------------------------------

### Q49. Local worktrees vs cloud runners --- when would you use each?

**Answer:**

**Local worktrees** are useful when:

-   you want direct local control,
-   you need local tooling,
-   you want to inspect changes immediately,
-   the task depends on your local environment.

**Cloud/sandbox runners** are useful when:

-   tasks are independent,
-   you want parallel execution,
-   you don't want to consume local resources,
-   the task can run autonomously.

------------------------------------------------------------------------

## 14. Parallelism and Engineering Efficiency

### Q50. What is the deeper advantage of agentic coding?

**Answer:**\
The major efficiency gain is not simply "AI writes code faster."

It is the ability to **parallelize engineering work**.

Instead of:

``` text
Issue A → finish → Issue B → finish → Issue C
```

you may be able to do:

``` text
        ┌→ Agent A → Issue A
Human ──┼→ Agent B → Issue B
        └→ Agent C → Issue C
```

The developer shifts toward orchestration, review, validation, and
decision-making.

------------------------------------------------------------------------

### Q51. What does a mature agentic workflow look like?

**Answer:**

A mature workflow may include:

``` text
1. Understand requirement
2. Select harness
3. Select model
4. Select effort level
5. Explore codebase
6. Clarify requirements
7. Create/review plan
8. Split independent work
9. Launch sub-agents/worktrees/sandboxes
10. Implement
11. Run tests/build/lint
12. Review output
13. Perform adversarial review where appropriate
14. Fix discovered issues
15. Create PR
16. Human performs final review
```

The exact workflow depends on the risk and complexity of the task.

------------------------------------------------------------------------

## 15. Adversarial Reviews and Guardrails

### Q52. What is an adversarial review?

**Answer:**\
An adversarial review asks another model/agent---or a separate review
pass---to actively search for flaws rather than simply approving the
implementation.

It can look for:

-   security issues,
-   hidden bugs,
-   incorrect assumptions,
-   regressions,
-   architectural problems,
-   poor error handling,
-   missing edge cases.

------------------------------------------------------------------------

### Q53. Why use a separate review pass?

**Answer:**\
The implementing agent may repeat or overlook its own assumptions.

A separate review pass gives the system another opportunity to challenge
the implementation.

Conceptually:

``` text
Implementation Agent
        ↓
      Code
        ↓
Adversarial Reviewer
        ↓
Problems / risks
        ↓
Fixes
        ↓
Tests + human review
```

------------------------------------------------------------------------

### Q54. Should adversarial review run on every task?

**Answer:**\
Not necessarily.

The transcript specifically warns against putting expensive workflows
into persistent instructions when they are unnecessary.

Use deeper review when the task justifies it---for example:

-   authentication,
-   payments,
-   permissions,
-   infrastructure,
-   large refactors,
-   critical business logic.

For a tiny styling change, the same level of review may be excessive.

------------------------------------------------------------------------

### Q55. What are guardrails in agentic coding?

**Answer:**\
Guardrails are constraints and validation steps designed to prevent
low-quality or dangerous agent output from being accepted automatically.

Examples include:

-   tests must pass,
-   lint must pass,
-   build must succeed,
-   security review for sensitive changes,
-   restricted tool permissions,
-   human approval before destructive actions,
-   required PR review.

------------------------------------------------------------------------

## 16. System Prompts

### Q56. What is a system prompt?

**Answer:**\
A system prompt is a higher-level set of instructions that shapes how
the model behaves.

Coding harnesses can use system instructions to tell the model how to:

-   inspect a repository,
-   use tools,
-   plan,
-   respect permissions,
-   follow workflows,
-   format results.

This is one reason two products using capable models can still behave
differently.

------------------------------------------------------------------------

### Q57. Why should an engineer understand system prompts even if they cannot see all of them?

**Answer:**\
Because understanding that the harness is guiding the model helps you
reason about why features such as Plan Mode or tool permissions behave
differently.

You do not need to memorize hidden prompts. You should understand that
the harness contributes significant behavior beyond the base model.

------------------------------------------------------------------------

## 17. Interview Scenarios

### Q58. "You have five independent GitHub issues. How would you use AI agents efficiently?"

**Strong answer:**\
First I would check whether the issues are truly independent. If they
are, I would avoid running all agents against the same working
directory.

Locally, I could create a Git worktree and branch per issue and run an
agent in each isolated worktree. Alternatively, if cloud/sandbox runners
are available, I could assign each issue to a separate environment.

Each agent would implement its issue, run the required validation, and
prepare a pull request. I would then review the PRs and integration
risks before merging.

------------------------------------------------------------------------

### Q59. "When would you use Plan Mode?"

**Strong answer:**\
I use planning when the task has ambiguity, high risk, broad scope, or
several affected systems. I want the agent to inspect the codebase and
ask clarification questions before it changes code.

For a tiny, well-defined change, I usually prefer a precise direct
prompt because a full planning phase would add unnecessary overhead.

------------------------------------------------------------------------

### Q60. "How do you choose an AI model?"

**Strong answer:**\
I choose based on task complexity, risk, latency, and cost. For
architecture, complex debugging, or large refactors, I prefer a stronger
reasoning model and higher effort. For small, well-defined
implementation tasks, I can use a faster and cheaper model.

For larger features, I may use a stronger model as an orchestrator and
delegate isolated implementation tasks to cheaper sub-agents.

------------------------------------------------------------------------

### Q61. "How do you manage AI token cost?"

**Strong answer:**\
I manage cost at several levels:

-   use lower effort for simple questions,
-   use stronger models only when complexity justifies them,
-   delegate straightforward implementation to cheaper models,
-   keep persistent agent instructions concise,
-   remove unused skills,
-   disable unnecessary MCP integrations,
-   avoid loading irrelevant context.

The goal is not minimum token usage; it is the best cost-to-quality
trade-off for the task.

------------------------------------------------------------------------

### Q62. "How do you prevent AI-generated code from reducing code quality?"

**Strong answer:**\
I treat generated code as untrusted until it passes the same engineering
checks as human-written code.

My workflow can include:

1.  clear requirements,
2.  planning for complex changes,
3.  constrained tool permissions,
4.  lint/type checking,
5.  tests,
6.  build validation,
7.  targeted security review,
8.  adversarial review for high-risk work,
9.  final human review.

AI increases implementation speed, but validation still determines
whether the change is acceptable.

------------------------------------------------------------------------

### Q63. "What belongs in `AGENTS.md` versus a skill?"

**Strong answer:**\
I put guidance that should apply broadly across sessions into
`AGENTS.md`, such as repository conventions and baseline validation
requirements.

I put optional or specialized repeatable workflows into skills---for
example deployment, release creation, deep security review, or a
standardized PR process.

That keeps persistent context smaller while preserving reusable
automation.

------------------------------------------------------------------------

### Q64. "What is MCP and why would you use it?"

**Strong answer:**\
MCP allows an agent to interact with external tools or services through
integrations. For example, I could let an agent retrieve recent Sentry
errors, analyze them, and help create issues, or interact with another
supported engineering service.

The advantage is reducing manual context switching and allowing the
agent to participate in workflows outside the local repository.

------------------------------------------------------------------------

### Q65. "What do you do when an agent gives you a suspicious answer?"

**Strong answer:**\
I do not assume the output is correct. I increase verification according
to the importance of the task.

That could mean inspecting the relevant source files, increasing model
effort, asking the agent to cite the exact code path, running tests,
using a second review agent, or manually validating the behavior.

Lower-effort responses are particularly useful as quick guidance rather
than unquestioned truth.

------------------------------------------------------------------------

## 18. Rapid-Fire Questions

### Q66. What is a coding harness?

An environment that combines a model with tools, instructions, context,
permissions, and coding workflows.

### Q67. What is Plan Mode?

A structured workflow for exploring and understanding a task before
implementation.

### Q68. What is a skill?

A reusable packaged workflow for a repeated task.

### Q69. What is `AGENTS.md` / `CLAUDE.md`?

Persistent repository-level instructions supplied to coding-agent
sessions.

### Q70. What is MCP?

A mechanism for connecting agents to external tools/services through
standardized integrations.

### Q71. What is a context window?

The finite amount of information the model can actively process during a
session.

### Q72. What is compaction?

Compressing/summarizing accumulated context so the session can continue
within context limits.

### Q73. What is skill hygiene?

Removing or updating skills that no longer provide enough value.

### Q74. What is MCP hygiene?

Disabling/removing unnecessary integrations to reduce context and token
overhead.

### Q75. What is a Git worktree?

An additional working directory for the same Git repository, commonly
attached to a different branch.

### Q76. Why use worktrees with agents?

To isolate concurrent agent tasks and reduce file/branch conflicts.

### Q77. What is a sub-agent?

An agent delegated a narrower task by another agent/orchestrator.

### Q78. What is an orchestrator?

The agent/model responsible for planning, decomposition, coordination,
and integration.

### Q79. What is an adversarial review?

A review pass explicitly tasked with finding flaws, risks, or incorrect
assumptions.

### Q80. What is a guardrail?

A constraint or validation step that limits unsafe or low-quality agent
behavior.

### Q81. When should you increase effort level?

When complexity, ambiguity, scope, or risk increases.

### Q82. Why not use maximum effort all the time?

Cost, latency, token usage, and diminishing returns.

### Q83. Why use voice prompting?

It can make detailed prompting faster and improve workflow efficiency.

### Q84. What is the human's role in agentic development?

Define intent, clarify requirements, choose workflows, review plans,
validate output, manage risk, and approve final changes.

------------------------------------------------------------------------

## 19. 60-Second Interview Answer

### Q85. "Describe your AI-assisted development workflow."

**Answer to practice:**

> I treat AI coding agents as engineering collaborators rather than
> autocomplete. For simple tasks, I use a direct, specific prompt with
> the location, current behavior, desired behavior, and constraints. For
> larger or higher-risk features, I first let the agent inspect the
> codebase and create a plan, then I review and refine that plan before
> implementation.
>
> I choose the model and reasoning effort based on complexity and risk
> instead of always using the most expensive option. For larger tasks, I
> can use a stronger model for orchestration and delegate independent
> implementation work to sub-agents using isolated Git worktrees or
> sandbox environments.
>
> I keep persistent project instructions concise, move repeatable
> workflows into skills, and use integrations such as MCP when the agent
> needs access to external engineering systems. Before accepting
> changes, I run the normal engineering checks---tests, type checking,
> linting, builds, and deeper review for high-risk changes. The goal is
> to increase throughput without giving up engineering judgment or code
> quality.

------------------------------------------------------------------------

## 20. Practical Interview Demo Checklist

Before an AI-native interview, be prepared to **demonstrate**, not only
define:

-   [ ] Use at least one coding-agent harness fluently.
-   [ ] Explain why you selected a particular model.
-   [ ] Change reasoning/effort level based on the task.
-   [ ] Write a precise implementation prompt.
-   [ ] Use planning for a non-trivial feature.
-   [ ] Explain the human-in-the-loop review process.
-   [ ] Show or explain a reusable skill.
-   [ ] Explain what belongs in `AGENTS.md` / `CLAUDE.md`.
-   [ ] Explain an MCP integration you have tried.
-   [ ] Inspect/understand context-window usage.
-   [ ] Explain skill and MCP hygiene.
-   [ ] Explain Git worktrees.
-   [ ] Explain parallel agent execution.
-   [ ] Discuss cloud/sandbox runners.
-   [ ] Explain sub-agents and orchestration.
-   [ ] Explain adversarial review.
-   [ ] Describe your guardrails and validation.
-   [ ] Explain how you balance quality, speed, and token cost.

------------------------------------------------------------------------

## 21. Final Revision Cheat Sheet

``` text
HARNESS
Claude Code / Codex / Cursor / Pi / etc.
        ↓
MODEL
Choose based on complexity + cost + risk
        ↓
EFFORT
Low → quick/simple
High → complex/high-risk
        ↓
PROMPT / PLAN
Precise direct prompt OR Plan Mode
        ↓
CONTEXT
System prompts + repo instructions + skills + MCP + code
        ↓
EXECUTION
Main agent / sub-agents / worktrees / sandboxes
        ↓
VALIDATION
Tests + lint + types + build + review
        ↓
ADVERSARIAL REVIEW
For important/high-risk changes
        ↓
PR
Human reviews and approves
```

### Remember these contrasts

  Question                    Key distinction
  --------------------------- -------------------------------------------
  Plan or direct prompt?      Complexity/risk vs simplicity
  Strong or cheap model?      Reasoning need vs cost/latency
  High or low effort?         Accuracy/risk vs speed
  `AGENTS.md` or skill?       Persistent guidance vs on-demand workflow
  One agent or sub-agents?    Sequential work vs decomposable work
  Same folder or worktrees?   Conflict risk vs isolated parallelism
  Local or cloud runner?      Direct control vs scalable isolation
  Trust output or verify?     **Always verify according to risk**

------------------------------------------------------------------------

## 22. The Core Message to Remember

The transcript's central interview message is not:

> "Use AI to generate as much code as possible."

It is:

> **Be able to explain and demonstrate a deliberate agentic engineering
> workflow.**

A strong candidate should be able to discuss:

**Harnesses → Models → Effort → Planning → Prompting → Skills → Agent
instruction files → MCP → Context → Hygiene → Worktrees → Sandboxes →
Parallelism → Reviews → Guardrails.**

The strongest interview answers connect these concepts to **real
workflows you have personally tried** rather than only defining the
terminology.
