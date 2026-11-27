# Agent Skills

Reusable skills for Claude Code and OpenAI Codex.

## Install

Install every skill from this repository into the current project for both agents with one command:

```sh
npx skills add BrandonLiang/skills --skill '*' --agent claude-code codex --yes
```

The Skills CLI creates agent-readable links in the project. Claude Code uses `.claude/skills/`; Codex uses `.agents/skills/`. Keep the generated `skills-lock.json` with your project if you want the installation to be reproducible for teammates. See the [Skills CLI reference](https://github.com/vercel-labs/skills/blob/main/docs/commands.md) for all options.

To install only one skill, replace `'*'` with its directory name, for example `--skill code-review`.

## Update

From the project where the skills were installed, update them with:

```sh
npx skills update --yes
```

To update only this repository's skills in a project containing other managed skills, list the desired skill names:

```sh
npx skills update code-review --yes
```

The CLI checks the recorded source and downloads changed skill contents. Re-run the install command above to add skills introduced in a later release.

## Use

Start Claude Code or Codex in the project after installation. Ask the agent to use an installed skill by name, or use the agent's skill picker/command where available. The root [`CLAUDE.md`](CLAUDE.md) and [`AGENTS.md`](AGENTS.md) describe how each agent should discover and follow repository skills.

**Claude Code note:** Claude Code may also provide a built-in `/code-review`. Installing this repository's skill under the same name can shadow that command. If you need both, install this skill under a distinct name with `npx skills add BrandonLiang/skills --skill code-review --rename repo-code-review --agent claude-code --yes`, or install it only for Codex.

## Developing this repository

Each skill lives in its own directory under `skills/` and must include a `SKILL.md` with `name` and `description` frontmatter. The Skills CLI discovers these files when installing the repository.
