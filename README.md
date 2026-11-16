# Hadden Industries Plugins

Tools for Codex, Claude Code, and Google Antigravity.

## Choose a plugin

Open a plugin's instructions for its requirements and usage.
Use its name in the installation commands below.

| Plugin                                                                                                                                          | What you can do                                                    |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| [committing-to-git](plugins/committing-to-git/skills/committing-to-git/SKILL.md)                                                                | Draft messages and prepare, create, and verify signed Git commits. |
| [defining-concepts](plugins/defining-concepts/skills/defining-concepts/SKILL.md)                                                                | Define, assess, and refine concepts and terminology.               |
| [naming-objects-in-software-engineering](plugins/naming-objects-in-software-engineering/skills/naming-objects-in-software-engineering/SKILL.md) | Choose precise names for software artifacts.                       |
| [reading-epubs](plugins/reading-epubs/skills/reading-epubs/SKILL.md)                                                                            | Read EPUB content with the accompanying conversion workflow.       |

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
agy plugin install ./plugins/committing-to-git
```

[Antigravity plugin documentation](https://antigravity.google/docs/plugins/)

## Choose a release

For repeatable installations, select a release tag before installing.

- For Codex, add `--ref` followed by the release tag to the marketplace command.
- For Claude Code, check out the tag locally and add that checkout as the marketplace.
- For Antigravity, check out the tag in your local clone before running the install command.

## License

Plugins use the [Mozilla Public License 2.0](LICENSE).
Each package includes its license and applicable third-party notices.

See the [Git helper's source availability information](plugins/committing-to-git/README.md) to request the corresponding source for its bundled executable.
