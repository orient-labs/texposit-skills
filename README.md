# texposit-skills

Community-contributed skills for the [TeXposit](https://github.com/orient-labs/TeXposit) in-editor AI assistant.

A **skill** is a modular, filesystem-based playbook that teaches the assistant how to
handle a specific LaTeX-writing task — troubleshooting compile errors, managing a
bibliography, formatting a Beamer presentation, following a particular field's
conventions, and so on. TeXposit loads skills from a `<slug>/SKILL.md` file and
surfaces them to the assistant situationally, when a skill's trigger condition matches
the user's request.

This repository is the **public, community-facing** skill library — separate from the
skills that ship built-in with TeXposit itself. It's the place to propose new skills,
improve existing ones, and browse what others have written.

## Using a skill

In your TeXposit project, open **Settings ▸ AI ▸ Skills**, create a new skill with the
same slug as the one you want to use, and paste in its `SKILL.md` content. Project
skills take precedence over any built-in skill of the same name.

That UI edits a single `SKILL.md` file per skill — it doesn't yet support skills that
ship extra files (a `GUIDE.md` or other resources alongside `SKILL.md`). If a skill in
this repo has those, you'll need to add them to your project's `skills/<slug>/`
directory some other way (e.g. through your project's own git history, if you sync it
externally).

## Contributing a skill

See [CONTRIBUTING.md](CONTRIBUTING.md) for the file format and how to submit one.

## License

MIT — see [LICENSE](LICENSE). This applies to the skill content in this repository
only; the TeXposit application itself is licensed separately (AGPL-3.0) in the
[main repo](https://github.com/orient-labs/TeXposit).
