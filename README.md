# Loop Engineering Templates

[Simplified Chinese](docs/zh-CN/README.md)

**Loop Engineering Templates** is a collection of Markdown Loop templates that can be copied and used independently.

Each template defines goals, boundaries, procedures, and the state and logs to retain. A local agent reads the template and performs each round of work in your project or working directory.

## Templates

| Template | Purpose | Guide |
|---|---|---|
| [GitHub Maintainer](templates/github-maintainer/) | Follow up on GitHub issues and PRs, reply and merge according to repository rules, and send a summary notification | [Usage and configuration](docs/github-maintainer-guide.md) |
| [Doc Maintainer](templates/doc-maintainer/) | Compare code changes in merged PRs with specified documentation and submit a documentation update PR when discrepancies are found | [Usage and configuration](docs/doc-maintainer-guide.md) |
| [Project Development](templates/project-development/) | Connect selected stages from project initiation to production deployment, with stage delegation, returns to code development for fixes, and an iteration limit | [Usage and configuration](docs/project-development-guide.md) |
| [Starter](templates/starter/) | A generic blank template for writing a new Loop | [Template authoring guide](docs/template-authoring.md) |

## Use an Existing Template

1. Copy the entire directory of the template you need into your project or working directory.
2. Follow its guide to fill in `CONFIG.md` in your copy and complete any other required files.
3. Tell your local agent: `Read <template-copy-directory>/LOOP.md and execute one round.`

For scheduled execution, configure a schedule in your local agent.

## Contributing and License

Contributions of new templates, workflow improvements, and documentation updates are welcome. See the [contributing guide](CONTRIBUTING.md). This project is licensed under the [MIT License](LICENSE).
