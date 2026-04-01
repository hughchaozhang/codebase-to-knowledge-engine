# Codebase-to-Knowledge-Engine

A Claude Code skill for turning a codebase into a polished, interactive HTML learning experience.

Point it at a local project, the current repo, or a GitHub URL. It analyzes the codebase, designs a short curriculum, and generates a browser-ready course that explains how the system works through plain-English code translations, data-flow visuals, quizzes, and glossary tooltips.

The spirit of the skill is simple: teach the codebase the way a smart operator would explain it on a whiteboard, not the way a textbook would.

## Who this is for

This skill is built for **vibe coders** and other non-traditional builders who can ship software with AI help but want a stronger grasp of what the code is actually doing.

It is optimized for people who want to:
- understand architecture without a formal CS background
- steer AI coding agents more precisely
- catch bad suggestions and hallucinations
- debug issues faster
- learn the vocabulary needed to talk about software clearly

## What it produces

The working output is a **course directory**.

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

For complex codebases, the build may also include a temporary `briefs/` directory used to coordinate module writing.

### Output contract

- Shared assets come from `references/`
- Module content is written as separate HTML sections
- `build.sh` assembles the final course
- The final deliverable is `index.html`
- The course runs directly in the browser
- External dependency: **Google Fonts CDN**

## What the course includes

- scroll-based modules with navigation and progress tracking
- code → plain-English translation blocks using real project code
- animated data-flow and component-communication visuals
- interactive quizzes focused on application and debugging judgment
- glossary tooltips for technical terms
- a warm, notebook-like visual style instead of generic AI aesthetics

## How to use

### Install as a Claude Code skill

Copy this folder into your Claude skills directory:

```bash
cp -R codebase-to-knowledge-engine ~/.claude/skills/
```

Then open a project in Claude Code and say something like:
- "Turn this codebase into an interactive course"
- "Explain this repo as an interactive walkthrough"
- "Teach me how this project works"
- "Make a guided codebase tutorial from this repo"

## Recommended workflow

1. Identify the source codebase
   - current repo
   - local path
   - GitHub URL
2. Analyze the codebase deeply enough to understand the main user flow, actors, and architecture
3. Design a 4-6 module curriculum
4. Choose sequential or parallel build path depending on complexity
5. Copy shared assets from `references/`
6. Write module files
7. Run `build.sh`
8. Open and review `index.html`

## Sequential vs parallel builds

Use the **sequential path** when most of these are true:
- one main app/runtime or one obvious entry point
- **5 modules or fewer**
- roughly fewer than **50 meaningful source files**
- one agent can comfortably hold the architecture in context

Use the **parallel path** when any of these are true:
- monorepo or multi-service system
- multiple distinct subsystems or product surfaces
- **6+ modules**
- likely quality drop if one agent writes the entire course in one pass

## Analysis budget

Do not spend unlimited context trying to understand every file.

As a rule of thumb:
- spend at most about **20% of available context** on codebase analysis
- prioritize README, entry points, route definitions, main data model, and the primary user journey
- do not chase every helper or utility unless it is central to the teaching story

## Example module

The repo includes `references/example-module.html`, a grounded reference module based on Karpathy's `autoresearch` repo.

Use it to calibrate:
- HTML structure
- visual density
- translation-block quality
- quiz tone
- tooltip usage
- interactive element wiring

It is a **reference**, not a template to copy blindly.

## Build troubleshooting

If `build.sh` succeeds but the result looks wrong, check these first:
- `_base.html` placeholders were fully replaced
- modules contain only module `<section>` content
- `data-steps` JSON is valid
- shared assets were copied from `references/`
- module filenames sort in the intended order

## Repository structure

```text
codebase-to-knowledge-engine/
├── SKILL.md
└── references/
    ├── _base.html
    ├── _footer.html
    ├── build.sh
    ├── content-philosophy.md
    ├── design-system.md
    ├── example-module.html
    ├── gotchas.md
    ├── interactive-elements.md
    ├── main.js
    ├── module-brief-template.md
    └── styles.css
```

## Design philosophy

The teaching model is simple: **start from what the learner already experiences, then trace what the code is doing underneath**.

The goal is not to turn someone into a software engineer. The goal is practical fluency:
- understand the moving parts
- know where logic lives
- follow the data
- debug more effectively
- make better calls when directing AI

## Notes

- Use real code snippets from the codebase
- Prefer visuals over paragraphs
- Keep modules practical and concrete
- Avoid jargon without definitions
- Always copy fresh shared assets from `references/`
- Make the final course feel polished, not autogenerated

---

Built by [Zara](https://x.com/zarazhangrui) with Claude Code.
