---
name: codebase-to-knowledge-engine
description: "Turn a codebase into a polished interactive HTML learning experience for non-technical or lightly technical readers. Use when someone wants an interactive walkthrough, course, tutorial, guided explainer, or teach-me-how-this-code-works artifact from a local project, current repo, or GitHub repository. Best for helping vibe coders understand architecture, data flow, APIs, and debugging concepts through code-to-English explanations, quizzes, and visualizations. Output is a buildable course directory with shared assets, module HTML files, and an assembled index.html."
---

# Codebase-to-Knowledge-Engine

Turn a codebase into a polished interactive course that explains how it works to a non-technical or lightly technical reader.

Think of the job as: understand the product, extract the architecture, then teach it visually.

The output is a **course directory** containing shared assets (`styles.css`, `main.js`, `_base.html`, `_footer.html`, `build.sh`), module HTML files, and a final assembled `index.html`. The final user-facing artifact is `index.html`, built from the directory.

## First response

If the skill is triggered before a codebase is clearly identified, say:

> I can turn a codebase into an interactive learning experience that explains how it works in plain English.
>
> Point me at one of these:
> - a local folder
> - a GitHub repo URL
> - the current project
>
> I’ll analyze the code, design a short curriculum, and generate a browser-ready course with visuals, code-to-English breakdowns, and quizzes.

If the user provides a GitHub URL, clone it first. If they say “this codebase” or equivalent, use the current working directory.

## Audience and teaching goal

Assume the learner has little or no formal CS background.

They want practical fluency, not theory for its own sake. Optimize for helping them:
- steer AI coding agents better
- spot bad architectural decisions
- debug common failures
- understand where logic lives
- learn the right software vocabulary

Explain terms in plain language. Define jargon on first use. Tie every module back to a practical payoff.

## Workflow

### 1) Analyze the codebase

Read only as much of the codebase as needed to understand:
- what the product does
- the main user journey
- the core actors (components, services, modules)
- the key data flows
- external systems (APIs, databases, queues, auth, storage)
- noteworthy engineering patterns or failure modes

Start with the README, entry points, route definitions, main data model, and the files that power the primary user flow.

During analysis, **bookmark or copy 2-3 short, self-contained code snippets per planned module**. Do this before writing. It prevents context-expensive re-reads later.

**Do not try to understand every helper.** For larger repos, spend at most about **20% of your available context** on analysis before moving into curriculum design.

Do not ask the user to explain the product unless the codebase is genuinely ambiguous.

### 2) Design a short curriculum

Usually build **4-6 modules**. Go beyond that only when the codebase truly needs it.

Use an arc like:
1. what the product does + what happens during a core user action
2. the main actors
3. how the pieces talk
4. external systems and boundaries
5. clever patterns or tradeoffs
6. debugging / failure modes / architecture recap

This is guidance, not a rigid template. Adapt to the codebase.

Each module should include:
- 3-6 screens
- at least one code-to-English translation block
- at least one interactive element
- at least one practical takeaway tied to debugging, decision-making, or steering AI

Across the full course, always include:
- at least one group chat animation
- at least one message/data flow animation
- at least one quiz per module
- glossary tooltips on first technical-term use per module

Do not present the curriculum for approval unless the user explicitly asks.

### 3) Choose the build path

Use the **sequential path** when most of these are true:
- the repo has one main app/runtime or one obvious entry point
- the course will be **5 modules or fewer**
- the repo is relatively compact (roughly fewer than **50 meaningful source files**)
- one agent can comfortably hold the architecture in context

Use the **parallel path** when any of these are true:
- the repo is a monorepo or multi-service system
- the codebase has several distinct subsystems or surfaces
- the course clearly needs **6+ modules**
- writing one module at a time in a single context would likely degrade later-module quality

When in doubt: if the architecture can be explained cleanly in 4-5 modules, use sequential. If it needs briefing packets to stay coherent, use parallel.

### 4) Name the course

Use:
- **course directory name:** a slugified project/repo name, usually `<project>-course`
- **display title:** a clean human-readable project name or product name

Keep the directory name filesystem-safe and stable, because every downstream path depends on it.

### 5) Build the course directory

Write a directory with this structure:

```text
course-name/
  styles.css
  main.js
  _base.html
  _footer.html
  build.sh
  modules/
    01-intro.html
    02-actors.html
    ...
  index.html
```

For complex builds, you may also create:

```text
course-name/briefs/
  01-intro.md
  02-actors.md
  ...
```

#### Shared assets

Copy these files verbatim from `references/`:
- `styles.css`
- `main.js`
- `_footer.html`
- `build.sh`

Always copy fresh assets from `references/`. **Do not reuse shared files from an older course build.**

`_base.html` opens the page shell. `_footer.html` closes the `</main>`, `</body>`, and `</html>` tags opened there.

Read `references/_base.html`, then customize only:
- `COURSE_TITLE`
- the accent color placeholders
- `NAV_DOTS`

Quick guide:
- `COURSE_TITLE` → visible course title
- accent placeholders → replace with one chosen palette from `_base.html`
- `NAV_DOTS` → one nav-dot button per module, in module order
- alternate module backgrounds → odd modules use `style="background: var(--color-bg-warm)"`, even modules use `style="background: var(--color-bg)"`

Do not regenerate shared CSS or JS from scratch.

#### Sequential path

Read:
- `references/content-philosophy.md`
- `references/gotchas.md`
- only the relevant sections of `references/interactive-elements.md` for the elements you will use in the current module
- `references/design-system.md` when needed for visual conventions

Then write modules one at a time.

Each module file should contain only the module `<section>` content. Do not add page boilerplate.

If you need calibration for output quality, read `references/example-module.html` before writing the first real module.

#### Parallel path

Before dispatching subagents, read:
- `references/module-brief-template.md`
- `references/content-philosophy.md`

Write one brief per module in `briefs/`, including:
- teaching arc
- pre-extracted code snippets with file paths and line numbers
- interactive elements required
- exact reference sections needed
- previous/next module connections

Then dispatch module-writing agents in small batches. Give each agent only:
- its brief
- `references/content-philosophy.md`
- `references/gotchas.md`
- the specific relevant sections from `interactive-elements.md` and `design-system.md`

Do not give writing agents the full codebase unless truly necessary.

### 6) Assemble and review

Run:

```bash
cd course-name && bash build.sh
```

This produces `index.html`.

Open `index.html` and do a quick QA pass:
- nav dots match modules
- module order makes sense
- no obvious tone drift
- interactive elements are present
- the course feels visual, not text-heavy

If the build or final page looks wrong, check these first:
- `_base.html` placeholders were fully replaced
- module files contain only module `<section>` blocks
- `data-steps` JSON is valid and does not break on quotes
- module filenames sort in the intended order
- shared assets were copied from `references/`, not regenerated
- the module mix still feels visual; if a screen turns into a paragraph wall, rewrite it before shipping

## Design rules

The experience should feel like a warm, distinctive developer notebook.

Read `references/design-system.md` for the full visual system.

## Content rules

Read `references/content-philosophy.md` before writing module content.

Use it as the source of truth for:
- visual density
- metaphor quality
- code-to-English translation style
- tooltip behavior
- quiz design

## Common failure points

Before finishing, check `references/gotchas.md`.

Especially watch for:
- walls of text
- under-tooltipping
- missing interactive elements
- broken flow-animation JSON
- clipped tooltips
- changed or shortened code snippets
- `scroll-snap-type: y mandatory` instead of `proximity`

## Reference files

Read only what is needed for the phase you are in:
- `references/content-philosophy.md`
- `references/gotchas.md`
- `references/module-brief-template.md`
- `references/design-system.md`
- `references/interactive-elements.md`
- `references/example-module.html`
- `references/_base.html`
