# Contributing

[Simplified Chinese](docs/zh-CN/CONTRIBUTING.md)

Contributions of new Loop templates, improvements to existing workflows, and documentation fixes are welcome.

## How to Contribute

1. Add a template: copy the directory containing [Starter](templates/starter/LOOP.md) and fill it in using the [authoring guide](docs/template-authoring.md).
2. Update a template: keep the related execution rules and usage documentation in sync.
3. Submit a PR: explain the problem, the changes, and how you verified them.

## Template Conventions

- Use Starter's structure of two sections and five fields, with the workflow defined in Markdown.
- Keep agent execution content in templates and user-facing usage and configuration documentation in docs. Each template directory must be usable independently.
- Implement the necessary workflow first, keep only essential configuration options, and define state for each specific Loop.
- Submit reusable templates with empty initial state and logs. Keep personal configuration, credentials, caches, and run records locally.

Before submitting, check documentation links, JSON syntax, and consistency between the guides and templates.

This project and its contributions are licensed under the [MIT License](LICENSE).
