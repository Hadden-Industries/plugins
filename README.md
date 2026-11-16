# Hadden Industries Plugins

Reusable skills for Codex, Claude Code, and Google Antigravity.

Each plugin bundles a skill's instructions, references, and any helper scripts.

## Choose a plugin

Open a plugin's instructions for its requirements and usage.
Use its name in the installation commands below.

| Plugin                                                                                                                                  | What you can do                                                    |
| --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| [committing-to-git](committing-to-git/skills/committing-to-git/SKILL.md)                                                                | Draft messages and prepare, create, and verify signed Git commits. |
| [defining-concepts](defining-concepts/skills/defining-concepts/SKILL.md)                                                                | Define, assess, and refine concepts and terminology.               |
| [naming-objects-in-software-engineering](naming-objects-in-software-engineering/skills/naming-objects-in-software-engineering/SKILL.md) | Choose precise names for software artifacts.                       |
| [reading-epubs](reading-epubs/skills/reading-epubs/SKILL.md)                                                                            | Read EPUB content with the accompanying conversion workflow.       |

## Install a plugin

These examples install `committing-to-git`.
Replace that name with another plugin from the catalog to install it instead.

### Codex

Run these commands in your terminal.

```sh
codex plugin marketplace add Hadden-Industries/plugins
codex plugin add committing-to-git@hadden-industries-plugins
```

[Codex plugin documentation](https://developers.openai.com/plugins/build/plugins)

### Claude Code

Run these commands inside Claude Code.

```text
/plugin marketplace add Hadden-Industries/plugins
/plugin install committing-to-git@hadden-industries-plugins
```

[Claude Code marketplace documentation](https://code.claude.com/docs/en/plugins/marketplace-reference)

### Google Antigravity

Clone the catalog, then install the plugin from its local directory.

```sh
git clone https://github.com/Hadden-Industries/plugins.git
cd plugins
agy plugin install ./committing-to-git
```

[Antigravity plugin documentation](https://antigravity.google/docs/plugins/)

## Use an installed skill

Name the skill and describe the result you need.
After installing `committing-to-git`, try this request in your agent.

```text
Use committing-to-git to draft a detailed commit message for my staged changes.
```

Provide the project context, files, or source material your task needs.

## Manage versions

### Update a plugin

- For Codex, run `codex plugin marketplace upgrade hadden-industries-plugins` to refresh the catalog, then apply the plugin update through your host's Plugins view.
- For Claude Code, run `/plugin marketplace update hadden-industries-plugins`, then `/plugin update committing-to-git@hadden-industries-plugins`.
- For Antigravity, update your local checkout to the release you want, then run the plugin's install command again.

Follow your host's reload or restart prompt to activate an updated plugin.
Replace `committing-to-git` with the plugin you are updating.

### Pin a release

For repeatable installations, select a release tag before installing.
To upgrade a pinned installation, select a newer tag explicitly.

- For Codex, add `--ref` followed by the release tag to the marketplace command.
- For Claude Code, check out the tag locally and add that checkout as the marketplace.
- For Antigravity, check out the tag in your local clone before running the install command.

## Get help or contribute

If a skill is missing, confirm that its plugin is installed and enabled, then start a new session.
If a helper fails, check the requirements in that skill's instructions.

Search [existing issues](https://github.com/Hadden-Industries/plugins/issues) or [open an issue](https://github.com/Hadden-Industries/plugins/issues/new) for bugs, corrections, and skill ideas.
Include plugin and host versions, expected and actual results, and a minimal example suitable for a public issue.

Maintainers apply skill changes in the authoring repository and regenerate the published packages.

## License

Plugins use the [Mozilla Public License 2.0](LICENSE).
Each package includes its license and applicable third-party notices.

See the [Git helper's source availability information](committing-to-git/README.md) to request the corresponding source for its bundled executable.
