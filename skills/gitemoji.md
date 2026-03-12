---
name: gitemoji
description: |
  Gitmoji Commit Templates skill for consistent, expressive version control.
  Provides ready-to-use commit message formats for features, fixes, refactors, config, dependencies, and more.
  Includes universal, scoped, monorepo, and multi-file templates.
  Use this skill to standardize commit messages across teams and projects, improve readability, and enable automated changelog generation.
---

# Gitmoji

A set of ready-to-use commit message templates using **Gitmoji**, designed for consistent and expressive version control.

## Rules

- Always prefix commits with the appropriate Gitmoji emoji.
- Use the official Gitmoji meanings from [gitmoji.dev](https://gitmoji.dev).
- Commit message structure: `<emoji> [scope]: <short description>`
- Keep commit messages short, clear, and action-oriented.
- Use one Gitmoji per commit. If more than one is needed, split the commit.

## Commit Structure

```bash
<emoji> [scope]: <short description>
```

Example:

```bash
✨(auth): add login flow
```

## Behavior

These templates help teams maintain clarity, consistency, and semantic meaning in version control history.

## Official Resources

- [Gitmoji Website](https://gitmoji.dev)
- [Gitmoji npm package](https://www.npmjs.com/package/gitmojis)
- [Gitmoji CLI tool](https://github.com/carloscuesta/gitmoji-cli)

## Contributing & Badge

You can contribute new emojis or improvements via the [Gitmoji issues](https://github.com/carloscuesta/gitmoji/issues/new). To add a badge to your project, use:

```html
<a href="https://gitmoji.dev">
  <img src="https://img.shields.io/badge/gitmoji-%20😜%20😍-FFDD67.svg?style=flat-square" alt="Gitmoji" />
</a>
```

---

## 1. Universal Commit Template

```bash
<emoji> [scope]: <short description>

<body - optional>
<footer - optional>
```

Example:

```bash
✨(auth): add login flow
```

---

## 2. Templates by Commit Type

All official emojis from [gitmoji.dev](https://gitmoji.dev):

### 🎨 Structure / Format

```bash
🎨 improve code structure or formatting
```

### ⚡ Performance

```bash
⚡ improve performance
```

### 🔥 Remove Code or Files

```bash
🔥 remove code or files
```

### 🐛 Bug Fix

```bash
🐛 fix a bug
```

### 🚑️ Critical Hotfix

```bash
🚑️ critical hotfix: <description>
```

### ✨ New Feature

```bash
✨ add new feature: <description>
```

### 📝 Documentation

```bash
📝 add/update documentation
```

### 🚀 Deploy

```bash
🚀 deploy changes
```

### 💄 UI / Styles

```bash
💄 add/update UI and style files
```

### 🎉 Begin Project

```bash
🎉 begin a project
```

### ✅ Tests

```bash
✅ add/update/pass tests
```

### 🔒️ Security / Privacy

```bash
🔒️ fix security or privacy issues
```

### 🔐 Secrets

```bash
🔐 add/update secrets
```

### 🔖 Release / Version Tags

```bash
🔖 release version: <version>
```

### 🚨 Linter Warnings

```bash
🚨 fix compiler/linter warnings
```

### 🚧 Work in Progress

```bash
🚧 wip: <description>
```

### 💚 Fix CI Build

```bash
💚 fix CI build
```

### ⬇️ Downgrade Dependencies

```bash
⬇️ downgrade dependency: <package>
```

### ⬆️ Upgrade Dependencies

```bash
⬆️ upgrade dependency: <package>
```

### 📌 Pin Dependencies

```bash
📌 pin dependency: <package>@<version>
```

### 👷 CI Build System

```bash
👷 add/update CI build system
```

### 📈 Analytics / Tracking

```bash
📈 add/update analytics or tracking code
```

### ♻️ Refactor

```bash
♻️ refactor code
```

### ➕ Add Dependency

```bash
➕ add dependency: <package>
```

### ➖ Remove Dependency

```bash
➖ remove dependency: <package>
```

### 🔧 Config Files

```bash
🔧 add/update configuration files
```

### 🔨 Development Scripts

```bash
🔨 add/update development scripts
```

### 🌐 Internationalization / Localization

```bash
🌐 add/update i18n/l10n support
```

### ✏️ Fix Typos

```bash
✏️ fix typos
```

### 💩 Bad Code

```bash
💩 write bad code that needs to be improved
```

### ⏪️ Revert Changes

```bash
⏪️ revert changes
```

### 🔀 Merge Branches

```bash
🔀 merge branch: <branch>
```

### 📦️ Compiled Files / Packages

```bash
📦️ add/update compiled files or packages
```

### 👽️ External API Changes

```bash
👽️ update code due to external API changes
```

### 🚚 Move / Rename Resources

```bash
🚚 move or rename resources
```

### 📄 License

```bash
📄 add/update license
```

### 💥 Breaking Change

```bash
💥 breaking change: <explanation>
```

### 🍱 Assets

```bash
🍱 add/update assets
```

### ♿️ Accessibility

```bash
♿️ improve accessibility
```

### 💡 Comments

```bash
💡 add/update comments in source code
```

### 🍻 Write Code Drunkenly

```bash
🍻 write code drunkenly
```

### 💬 Text / Literals

```bash
💬 add/update text and literals
```

### 🗃️ Database

```bash
🗃️ perform database related changes
```

### 🔊 Add Logs

```bash
🔊 add/update logs
```

### 🔇 Remove Logs

```bash
🔇 remove logs
```

### 👥 Contributors

```bash
👥 add/update contributors
```

### 🚸 UX / Usability

```bash
🚸 improve user experience/usability
```

### 🏗️ Architecture

```bash
🏗️ make architectural changes
```

### 📱 Responsive Design

```bash
📱 work on responsive design
```

### 🤡 Mocks

```bash
🤡 mock things
```

### 🥚 Easter Egg

```bash
🥚 add/update an easter egg
```

### 🙈 .gitignore

```bash
🙈 add/update .gitignore file
```

### 📸 Snapshots

```bash
📸 add/update snapshots
```

### ⚗️ Experiments

```bash
⚗️ perform experiments
```

### 🔍️ SEO

```bash
🔍️ improve SEO
```

### 🏷️ Types

```bash
🏷️ add/update types
```

### 🌱 Seed Files

```bash
🌱 add/update seed files
```

### 🚩 Feature Flags

```bash
🚩 add/update/remove feature flags
```

### 🥅 Catch Errors

```bash
🥅 catch errors
```

### 💫 Animations / Transitions

```bash
💫 add/update animations and transitions
```

### 🗑️ Deprecate Code

```bash
🗑️ deprecate code that needs to be cleaned up
```

### 🛂 Auth / Roles / Permissions

```bash
🛂 work on authorization, roles and permissions
```

### 🩹 Simple Fix

```bash
🩹 simple fix for a non-critical issue
```

### 🧐 Data Exploration

```bash
🧐 data exploration/inspection
```

### ⚰️ Remove Dead Code

```bash
⚰️ remove dead code
```

### 🧪 Failing Test

```bash
🧪 add a failing test
```

### 👔 Business Logic

```bash
👔 add/update business logic
```

### 🩺 Healthcheck

```bash
🩺 add/update healthcheck
```

### 🧱 Infrastructure

```bash
🧱 infrastructure related changes
```

### 🧑‍💻 Developer Experience

```bash
🧑‍💻 improve developer experience
```

### 💸 Money / Sponsorships

```bash
💸 add sponsorships or money related infrastructure
```

### 🧵 Multithreading / Concurrency

```bash
🧵 add/update code related to multithreading or concurrency
```

### 🦺 Validation

```bash
🦺 add/update code related to validation
```

### ✈️ Offline Support

```bash
✈️ improve offline support
```

### 🦖 Backwards Compatibility

```bash
🦖 code that adds backwards compatibility
```

### 🧩 New Component

```bash
🧩 add a new component: <name>
```

### 📖 Storybook

```bash
📖 add/update Storybook files: <name>
```

---

## 3. Template with Scopes

```bash
<emoji>(<scope>): <short description>

[optional body]
[optional footer]
```

Example:

```bash
♻️(button): simplify event handlers
```

---

## 4. Extended Template (Gitmoji + Conventional Commits)

```bash
<emoji>(scope): <summary>

BREAKING CHANGE: <explanation>   # optional

# Body (optional):
- What changed?
- Why?
- How?

Refs: #issue, @user
```

Example:

```bash
💥(api): remove deprecated /v1 endpoint

BREAKING CHANGE: clients must migrate to /v2.
```

---

## 5. Monorepo Template

```bash
<emoji>(package/<name>): <summary>

[optional body]
Refs: <issues>
```

Example:

```bash
✨(packages/utils): add date formatter helper
```

---

## 6. Multi-file Commit Template

```bash
<emoji>: <summary>

Affected:
- <file or module>
- <file or module>

Details:
- <point>
- <point>
```

---

## Usage

These templates can be integrated into:

- `.gitmessage`
- VSCode snippets
- gitmoji-cli prompts
- Your `copilot-instructions` skill system
