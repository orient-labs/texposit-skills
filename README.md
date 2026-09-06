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

Copy a skill's directory into your TeXposit project's own `skills/` folder (via the
project's AI Setup ▸ Manage Skills, or by adding the files directly if you sync your
project with git). Project skills take precedence over any built-in skill of the same
name.

## Contributing a skill

See [CONTRIBUTING.md](CONTRIBUTING.md) for the file format and how to submit one.

## License

MIT — see [LICENSE](LICENSE). This applies to the skill content in this repository
only; the TeXposit application itself is licensed separately (AGPL-3.0) in the
[main repo](https://github.com/orient-labs/TeXposit).
