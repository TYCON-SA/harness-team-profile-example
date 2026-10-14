# Team profile — example

[Castellano](README.md)

An example **team profile** for the harness, built for an invented organization: **acme**. It shows your organization
how to build its own, so you can copy and adapt it. No name in here is real.

The files themselves are commented in Spanish, the main language of the harness; this page explains them in English.

## What a team profile is

A small **private** repository of your organization with what the whole team shares: the configuration, where each
repository lives, and the team's own reviewers and skills. Every machine that installs the harness with `--team`
downloads it, validates it and stacks it on top of the public harness. When the profile changes, each machine gets the
change on its next update.

## What this example contains

```
team-layer.json                                         the profile declaration (required)
perfil-equipo.json                                      the team configuration
team-repos.json                                         where each harness repository of the organization lives
packs/acme/pack.json                                    a reviewer pack: when each reviewer runs
agents/acme-logging-reviewer.md                         a team reviewer
skills/infrastructure/git-workflow/SKILL.md             a skill: the team's git conventions
skills/infrastructure/git-workflow/references/*.md      supporting material for the skill
README.md, README.en.md                                 this page (not installed)
```

| File | What it is for |
|---|---|
| `team-layer.json` | Declares this repository as a team profile: its name and the minimum harness version that understands it. Only `schema`, `name` and `requires`, no comments. |
| `perfil-equipo.json` | Which integrations are on, how machines update, which reviewer packs run and how much the harness installs. Team values only: an email, a chat id or a token makes it invalid. |
| `team-repos.json` | Where each repository lives: where machines install from (`public`), this repository (`profile`) and the one for scheduled jobs (`operations`). |
| `packs/acme/pack.json` | Groups the team's reviewers and says when each one joins the review of a change. |
| `agents/acme-logging-reviewer.md` | A reviewer: a read-only agent the pipeline runs on a change, with its rule catalog. |
| `skills/infrastructure/git-workflow/` | Instructions the assistant loads when the task matches its description. |

Every file carries a short comment: in the `.json` files, the `__doc` key (keys starting with `__` are comments); in
the `.md` files, an HTML comment; in `pack.json`, its `description` and the reviewer's `note`.

## How to copy and adapt it

1. **Create a private repository** in your organization, on GitHub or Azure DevOps, visible only to your team. For
   example `harness-team-profile`.
2. **Copy these files** into it.
3. **Replace `acme` with your organization's name** in `team-layer.json` (`name`), `team-repos.json` and
   `perfil-equipo.json` (`ado.org`, if you use Azure DevOps; otherwise delete that block).
4. **Fill in `team-repos.json`** with your real repositories. On GitHub: `{ "platform": "github", "org", "repo" }`; on
   Azure DevOps: `{ "platform": "azure", "org", "project", "repo" }`. You can also edit it with the
   `harness.js team-config` page, which validates what you type.
5. **Choose the configuration** in `perfil-equipo.json`. Start with `install.profile: minimal` and `update.apply: ask`.
6. **Keep what you need.** The reviewer, the pack and the skill are examples: rename them with your own prefix (if you
   rename a file or folder, change its `name:` too) or delete them. A profile can hold just `team-layer.json`,
   `perfil-equipo.json` and `team-repos.json`.
7. **Install with the profile**, from the public harness:

   ```bash
   node scripts/setup/harness.js install --version latest --team <your-org>/harness-team-profile
   ```

   On Azure DevOps, `--team` takes the repository URL: `https://dev.azure.com/<org>/<project>/_git/<repo>`. The
   credential to read a private repository is stored once with `harness.js config set github.token` (or
   `azure.token`), which asks for it without echoing it.

## What the harness accepts in a profile

The harness checks the whole profile before installing it. **If a single file fails, nothing from the profile is
installed**, and it says which file and why.

- **Allowed paths:** `perfil-equipo.json`, `team-repos.json`, `packs/<pack>/pack.json`, `agents/<name>.md`, the `.md`
  files of a skill under `skills/` (with its `SKILL.md`, up to three folders below `skills/`) and the `.md` files at the
  root of `onboarding/` (the text a new developer pastes to the assistant). The root may also hold `README*`,
  `LICENSE*` and `.gitignore`, which are not installed.
- **Nothing executable:** no scripts, hooks, symlinks or executable files. The frontmatter of an agent or a skill
  cannot contain `hooks:` or `mcpServers:`.
- **Names:** an agent's `name:` is its file name; a skill's, its folder; a pack's, its folder. None can share a name
  with an agent, skill or pack the harness already ships.
- **Limits:** up to 300 files and 1 MB in total.

## Trying it

With the profile pushed, an install with `--team` says whether it was accepted. On an installed machine,
`node scripts/setup/harness.js doctor` shows the profile status and `harness.js version` which commit it came from.
