# Agent Skills Based on Books I Have Read

A collection of [Agent Skills](https://agentskills.io), each distilling one engineering book into a focused set of rules. One book, one skill — so you can pull in exactly the guidance the task needs and nothing else.

Each skill is a plain `SKILL.md` folder following the open Agent Skills format, so it works in **Claude Code** and **Cursor** (and any other skills-compatible agent) without modification.

## Skills

| Skill | Book & Author | Good to use when |
| --- | --- | --- |
| [`book-ai-engineering`](skills/book-ai-engineering/SKILL.md) | *AI Engineering* — Chip Huyen. Building reliable applications on top of foundation models. | Working with LLMs: prompts, structured outputs, evaluation, and reliability. |
| [`book-the-engineers-guide-to-rag`](skills/book-the-engineers-guide-to-rag/SKILL.md) | *The Engineer's Guide to RAG* — Shivani Virdi. Practical retrieval-augmented generation. | Building or improving a RAG pipeline: chunking, retrieval, and grounded answers. |
| [`book-designing-data-intensive-applications`](skills/book-designing-data-intensive-applications/SKILL.md) | *Designing Data-Intensive Applications* — Martin Kleppmann. The foundations of scalable, reliable data systems. | Designing systems around data: storage, scaling, consistency, and distribution. |
| [`book-refactoring`](skills/book-refactoring/SKILL.md) | *Refactoring* — Martin Fowler. Code smells and the refactorings that fix them. | Cleaning up existing code and keeping new code readable and maintainable. |
| [`book-dont-make-me-think`](skills/book-dont-make-me-think/SKILL.md) | *Don't Make Me Think* — Steve Krug. Common-sense web usability and interface design. | Building frontend UI/UX: navigation, scannable layouts, forms, and accessibility. |
| [`book-the-pragmatic-programmer`](skills/book-the-pragmatic-programmer/SKILL.md) | *The Pragmatic Programmer* — David Thomas & Andrew Hunt. Timeless engineering philosophy and craftsmanship. | Writing decoupled, testable, adaptable code and making pragmatic day-to-day design decisions. |

## Install

Clone the repo, then copy the skills you want into your agent's skills directory.

```bash
git clone https://github.com/doruyaar/agent-skills-based-on-books-I-have-read.git
cd agent-skills-based-on-books-I-have-read
```

**Claude Code**

```bash
# Personal (all projects)
cp -r skills/* ~/.claude/skills/

# Or per project
mkdir -p .claude/skills && cp -r skills/* .claude/skills/
```

**Cursor**

```bash
# Personal (all projects)
cp -r skills/* ~/.cursor/skills/

# Or per project
mkdir -p .cursor/skills && cp -r skills/* .cursor/skills/
```

Copy a single book instead of all of them by naming its folder, e.g. `cp -r skills/book-refactoring ~/.claude/skills/`.

## Usage

Because there is one skill per book, you can **call a book by name** whenever you want its perspective. All skills share the `book-` prefix, so typing `/book-` surfaces the full set:

> Use the `book-refactoring` skill on `src/billing/invoice.ts`.

> Review this component with `book-dont-make-me-think`.

> Apply `book-designing-data-intensive-applications` to this partitioning plan.

Each skill also carries a description saying what it covers and when it applies, so your agent can pull the right book in on its own when the task obviously calls for it. Only the name and description stay in context; the full rule set loads only when the skill is actually used.

## Skill structure

```
skills/
└── book-<book-name>/
    └── SKILL.md    # YAML frontmatter (name, description) + the rules
```

Frontmatter is deliberately limited to `name` and `description` — the two fields required by the Agent Skills spec — so nothing is tied to a single vendor.

## Naming convention

Skill folders use a `book-` prefix, then the book title lowercased with hyphens (`book-the-pragmatic-programmer`, `book-dont-make-me-think`). The prefix makes the collection easy to discover via `/book-` after install, and the folder name is what you type to invoke the skill.

## Adding a book

1. Create `skills/book-<book-name>/SKILL.md`.
2. Add frontmatter with `name` (matching the folder) and a `description` covering both **what** the book teaches and **when** to reach for it.
3. Distill the book into grouped, bullet-point rules — each one actionable on its own.
4. Add a row to the table above.
