# Contributing a skill

## Directory structure

```
skills/
  my-gerund-skill/
    SKILL.md       # Primary entry point, required
    GUIDE.md        # (Optional) Deep-dive resource
    resource.txt    # (Optional) Any other support file
```

## SKILL.md format

Every `SKILL.md` must start with YAML frontmatter, then Markdown instructions written
for the assistant to follow — not for a human reader:

```markdown
---
name: name-of-skill
description: Concise, third-person summary including the triggers that should fire this skill.
author: your.email@example.com
---
# Name of Skill
... instructions for the assistant ...
```

`author` is optional but recommended — an email TeXposit can show next to your skill
in the app's "browse community skills" list, and use to reach you about it.

## Naming

Use gerund phrases, matching the tone of the built-in library: `troubleshooting-latex`,
`managing-bibliographies`, `formatting-beamer-slides`. The directory name and the
frontmatter `name` field must match.

## Writing guidelines

1. **Write for the assistant, not a human reader.** No prose explaining what the skill
   is for beyond the frontmatter `description` — the body is instructions to follow.
2. **Name the tools to use.** If the assistant should call specific tools
   (`read_file`, `get_tex_structure`, etc.), say so explicitly.
3. **Keep `SKILL.md` concise.** Move detailed tables, long examples, or reference data
   into a separate file in the same directory and point to it.
4. **No placeholders.** Give real, working LaTeX snippets and commands, not
   `<your code here>`.
5. **Cover the failure case.** Say what the assistant should do if the primary
   approach doesn't work.
6. **Stay generic.** A skill runs across many different projects — don't bake in
   assumptions specific to one document, journal, or institution.

A starting template is in [`skills/_template/`](skills/_template/SKILL.md).

## Submitting

1. Fork this repo.
2. Add your skill under `skills/<your-gerund-name>/`.
3. Open a pull request describing what the skill does and, ideally, an example of the
   request it should trigger on.
4. A maintainer will review for format, scope, and quality before merging.

## Scope

Skills should be about **LaTeX writing and document workflows** — compiling, citing,
formatting, structuring, collaborating — not general-purpose assistant behavior. If
you're unsure whether something belongs here, open an issue first.
