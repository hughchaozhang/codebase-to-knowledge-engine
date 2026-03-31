# Codebase to Knowledge Engine

A Claude Code skill that turns any codebase into:
• executive walkthrough
• onboarding course
• architecture deep dive
• feature learning path
• 30-minute repo explainer
• a beautiful, interactive single-page HTML course.

Point it at a repo. Get back a stunning, self-contained course that teaches how the code works — with scroll-based navigation, animated visualizations, embedded quizzes, and code-with-plain-English side-by-side translations.

## Who is this for?

**"Vibe coders"** — people who build software by instructing AI coding tools in natural language, without a traditional CS education.



## What the course looks like

The output is a **single HTML file** — no dependencies, no setup, works offline. It includes:

- **Scroll-based modules** with progress tracking and keyboard navigation
- **Code ↔ Plain English translations** — real code on the left, what it means on the right
<img width="720" alt="Code translation block" src="https://github.com/user-attachments/assets/fb9e7fac-05c1-4f98-b80c-46543ef81afc" />

- **Animated visualizations** — data flow animations, group chat between components, architecture diagrams
<img width="720" alt="Animated data flow" src="https://github.com/user-attachments/assets/20fb403e-7dfd-4a47-989b-bbae86ca8041" />

- **Interactive quizzes** that test *application* not memorization ("You want to add favorites — which files change?")
<img width="720" alt="Interactive quiz" src="https://github.com/user-attachments/assets/57706496-9fa8-457a-8450-3da22789951c" />

- **Glossary tooltips** — hover any technical term for a plain-English definition
<img width="720" alt="Glossary tooltip" src="https://github.com/user-attachments/assets/ac2f160a-d73f-4779-97b2-a06fdb5f3227" />

  
- **Warm, distinctive design** — not the typical purple-gradient AI look

## How to use

### As an Agent skill

### Trigger phrases

- "Turn this into a course"
- "Explain this codebase"


## Design philosophy

### Show, don't tell

Every screen is at least 50% visual. Max 2-3 sentences per text block. If something can be a diagram, animation, or interactive element — it shouldn't be a paragraph.

### Quizzes test doing, not knowing

No "What does API stand for?" Instead: "A user reports stale data after switching pages. Where would you look first?" Quizzes test whether you can *use* what you learned to solve a new problem.

### No recycled metaphors

Each concept gets a metaphor that fits *that specific idea*. A database is a library with a card catalog. Auth is a bouncer checking IDs. API rate limiting is a nightclub with a capacity limit. Never the same metaphor twice.

### Original code only

Code snippets are exact copies from the real codebase — never modified or simplified. The learner should be able to open the actual file and see the same code they learned from.

## Skill structure

```
codebase-to-course/
├── SKILL.md                          # Main skill instructions
└── references/
    ├── design-system.md              # CSS tokens, typography, colors, layout
    └── interactive-elements.md       # Quiz, animation, and visualization patterns
```

