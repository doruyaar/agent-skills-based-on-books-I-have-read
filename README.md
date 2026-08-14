# Agent Rules Based on Books I Have Read

A small collection of tool-agnostic agent rule files, each distilling one engineering book into a focused set of rules. They are plain Markdown with no tool-specific format, so they work with any AI coding assistant — Cursor, GitHub Copilot, Claude, and others. Point your assistant at the relevant file to give it opinionated, book-backed guidance for the task at hand.

## Rules

| Rule | Book & Author | Good to use when |
| --- | --- | --- |
| [`AI_Engineering_by_Chip_Huyen.md`](AI_Engineering_by_Chip_Huyen.md) | *AI Engineering* — Chip Huyen. Building reliable applications on top of foundation models. | Working with LLMs: prompts, structured outputs, evaluation, and reliability. |
| [`The_Engineers_Guide_to_RAG_by_Shivani_Virdi.md`](The_Engineers_Guide_to_RAG_by_Shivani_Virdi.md) | *The Engineer's Guide to RAG* — Shivani Virdi. Practical retrieval-augmented generation. | Building or improving a RAG pipeline: chunking, retrieval, and grounded answers. |
| [`Designing_Data-Intensive_Apps_by_Martin_Kleppmann.md`](Designing_Data-Intensive_Apps_by_Martin_Kleppmann.md) | *Designing Data-Intensive Applications* — Martin Kleppmann. The foundations of scalable, reliable data systems. | Designing systems around data: storage, scaling, consistency, and distribution. |
| [`Refactoring_by_Martin_Fowler.md`](Refactoring_by_Martin_Fowler.md) | *Refactoring* — Martin Fowler. Code smells and the refactorings that fix them. | Cleaning up existing code and keeping new code readable and maintainable. |
| [`Dont_Make_Me_Think_by_Steve_Krug.md`](Dont_Make_Me_Think_by_Steve_Krug.md) | *Don't Make Me Think* — Steve Krug. Common-sense web usability and interface design. | Building frontend UI/UX: navigation, scannable layouts, forms, and accessibility. |
| [`The_Pragmatic_Programmer_by_David_Thomas_and_Andrew_Hunt.md`](The_Pragmatic_Programmer_by_David_Thomas_and_Andrew_Hunt.md) | *The Pragmatic Programmer* — David Thomas & Andrew Hunt. Timeless engineering philosophy and craftsmanship. | Writing decoupled, testable, adaptable code and making pragmatic day-to-day design decisions. |

## How to use

Each file is grouped, bullet-point Markdown you can drop into whichever mechanism your tool uses:

- **Cursor** — copy into `.cursor/rules/` (add frontmatter if you want `alwaysApply`/`globs`).
- **GitHub Copilot** — copy into `.github/copilot-instructions.md`.
- **Claude** — copy into `CLAUDE.md`.
- **Any assistant** — paste the file (or link it) into your prompt or context.

## Naming convention

Files follow `Title_by_Author.md` — underscores separate words, hyphens are reserved for genuine compounds (e.g. `Data-Intensive`).
