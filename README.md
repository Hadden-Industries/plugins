# Hadden Industries Plugins

Each directory under `plugins/` is an installable plugin. The package contains its skill, resources, license, and applicable third-party notices. License and source availability information is included with each package.

| Plugin | Use it to |
| --- | --- |
| [Committing to Git](plugins/committing-to-git/) | Prepare and verify signed Git commits. |
| [Defining Concepts](plugins/defining-concepts/) | Define and assess concepts and terminology. |
| [Naming Objects in Software Engineering](plugins/naming-objects-in-software-engineering/) | Choose precise names for software artifacts. |
| [Reading EPUBs](plugins/reading-epubs/) | Read EPUB content with the accompanying conversion workflow. |

## Codex

```sh
codex plugin marketplace add Hadden-Industries/plugins
codex plugin add committing-to-git@hadden-industries-plugins
```

Use `--ref` when adding a marketplace to pin an immutable release tag. Select the plugin you want by its directory name. [Codex plugins](https://developers.openai.com/plugins/build/plugins).

## Claude Code

```text
/plugin marketplace add Hadden-Industries/plugins
/plugin install committing-to-git@hadden-industries-plugins
```

For an exact snapshot, check out the release tag and add that local directory as the marketplace. [Claude marketplace documentation](https://code.claude.com/docs/en/plugins/marketplace-reference).

## Google Antigravity

Check out the desired release tag and install the individual plugin directory with the Antigravity CLI:

```sh
agy plugin install ./plugins/committing-to-git
```

The directory has an Antigravity manifest and the bundled skill. [Antigravity plugin documentation](https://antigravity.google/docs/plugins/).
