# Contributing to Krabby Organizational Assets

First off, thank you for considering contributing to the Krabby ecosystem! It's people like you that make the open-source community such an incredible place to learn, inspire, and create.

This repository is the central hub for Krabby's organizational identity, engineering standards, and public profile content. Contributions here help define how the world sees Krabby and will make much impact on everything we do. All types of contributions are encouraged and valued. Please see the table of contents for different ways to help and details on how to ensure you do so without encountering any issues.

## Table of Contents

- [Code of Conduct](#code-of-conduct)

- [I Have a Question](#i-have-a-question)

- [I Want To Contribute](#i-want-to-contribute)

- [Reporting Issues](#reporting-issues)

- [Suggesting Improvements](#suggesting-improvements)

- [Making Your First Contribution](#making-your-first-contribution)

- [Conclusion](#conclusion)

- [Attribution](#attribution)

## Code of Conduct

This project and everyone participating in it is governed by the [Krabby Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to [okpainmoandrew@gmail.com](mailto:okpainmoandrew@gmail.com).

## I Have a Question

Before you ask a question, please search existing [Issues](https://github.com/KrabbyHQ/.github/issues) to see if it has already been addressed. If you still need clarification:

- Open an [Issue](https://github.com/KrabbyHQ/.github/issues/new).
- Provide as much context as possible.

## I Want To Contribute

> ### Legal Notice
>
> When contributing to this project, you must agree that you have authored 100% of the content, that you have the necessary rights to the content, and that the content you contribute may be provided under the project license.

### Reporting Issues

We use GitHub issues to track bugs in our documentation, broken links in our profile, or issues with our shared assets.

- Explain the behavior you would expect and the actual behavior.
- Provide screenshots if you're reporting a visual bug in the profile or banners.

### Suggesting Improvements

Whether it's a better way to phrase our documentation, a new brand asset, or an update to our engineering standards, we value your input.

- Use a clearly descriptive title.
- Explain why this improvement would benefit the Krabby ecosystem.

## Making Your First Contribution

### Setup

Please follow the instruction in README.md for project prerequisites and environment setup.

We use Husky and Commitlint to ensure high contribution standards. After cloning the repo, make sure to run:

```shell
bun install
```

This will set up the git hooks required for development.

### Workflow

1. Fork the repo and create your branch from `main`.

> All contributions must come from a branch named in this format: `<preferred_code_name>_dev` - all lowercase.
>
> E.g. `okpainmo_dev`
>
> To maintain a clean history, we prefer that contributors use a single development branch for their contributions rather than creating multiple feature branches.

2. Ensure your changes follow our formatting standards.

3. If you've updated the Profile README, ensure all links and assets are correctly referenced.

4. Run formatting before committing:

   ```bash
   bun run format
   ```

### Commit Messages

This project uses **Conventional Commits**, enforced by `Commitlint` and `Husky`.

Structure:

```
<type>[optional scope]: <description>

[optional body]
```

**Common types:**

- `feat`: A new organizational asset or profile feature.

- `fix`: Correcting a typo, broken link, or visual glitch.

- `docs`: Documentation updates.

- `style`: Formatting changes.

- `chore`: Maintenance tasks (package updates, husky config).

Example: `docs(profile): update architectural overview links`

### Making a Pull Request.

Refer to the attached [pull request template](.github/pull_request_template.md) for all the details you'll need to ensure you submit PRs that will easily pass our approval checks. The tips below will also be very helpful.

- Maintain context with each PR. Each PR should solve/handle something specific.

- Keep PRs as small as possible. Bulky PRs(PRs with plenty commits/changes) will demand longer reviews, hence the longer it will be before your contribution will possibly be accepted.

- Write clearly. Ensure to communicate your incoming contributions as consisely as you possibly can.

## Conclusion.

Thanks once again for your interest in contributing to this project. Hope to see you PR soon. Cheers!!!

## Attribution

This guide is based on the [contributing.md](https://contributing.md) template.
