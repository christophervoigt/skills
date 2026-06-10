---
name: setup-stylelint
description: Set up Stylelint in a Node-based frontend project. Use when a user asks to add or configure Stylelint, including stylelint-order, a shared stylelint.config.mjs, and a package.json lint script.
---

# Stylelint Setup Skill

Use this skill when the user wants to set up Stylelint in a JavaScript, TypeScript, or frontend project.

This is a global skill, not project-specific. It assumes this skill folder contains a reusable `stylelint.config.mjs` file that should be copied into the root of the target project.

## Goal

Set up Stylelint in the current project with a shared configuration, the required Stylelint packages, and a package script.

## Preconditions

Before making changes:

1. Confirm that the current working directory is the project root.
2. Check whether a `package.json` file exists.
3. If no `package.json` exists, stop and explain that this skill only applies to Node-based projects with a `package.json`.

## Process

### 1. Inspect `package.json`

Read the root `package.json`.

Determine:

- Whether `stylelint` is already installed.
- Whether `stylelint-order` is already installed.
- Which package manager appears to be used.

Check all dependency sections:

- `dependencies`
- `devDependencies`
- `peerDependencies`
- `optionalDependencies`

Treat `stylelint` as installed if it appears in any of these sections.

Treat `stylelint-order` as installed if it appears in any of these sections.

Package manager detection:

1. If `pnpm-lock.yaml` exists, use `pnpm`.
2. Else if `yarn.lock` exists, use `yarn`.
3. Else if `bun.lockb` or `bun.lock` exists, use `bun`.
4. Else if `package-lock.json` exists, use `npm`.
5. Else default to `npm`.

### 2. Install Stylelint packages if needed

If `stylelint` and/or `stylelint-order` are not already installed, install the missing packages as dev dependencies.

Only install packages that are missing.

Use the detected package manager.

For pnpm:

```bash
pnpm add -D stylelint stylelint-order
```

For yarn:

```bash
yarn add -D stylelint stylelint-order
```

For bun:

```bash
bun add -d stylelint stylelint-order
```

For npm:

```bash
npm install -D stylelint stylelint-order
```

If only one package is missing, install only that package.

Examples:

```bash
npm install -D stylelint
```

```bash
npm install -D stylelint-order
```

Do not reinstall packages that already exist in `package.json`.

### 3. Copy `stylelint.config.mjs`

Copy the `stylelint.config.mjs` file from this skill folder into the root of the project.

The target path must be:

```text
./stylelint.config.mjs
```

If a `stylelint.config.mjs` already exists in the project root:

1. Do not overwrite it silently.
2. Compare the existing file with the skill's version.
3. If they differ, explain that a Stylelint config already exists and ask before replacing it.
4. If they are identical, leave it unchanged.

### 4. Add package script

Update the root `package.json`.

Add the following script:

```json
"lint": "stylelint \"src/**/*.css\" --fix"
```

If the `scripts` object does not exist, create it.

If a `lint` script already exists:

1. Do not overwrite it silently.
2. If it is already `"stylelint \"src/**/*.css\" --fix"`, leave it unchanged.
3. If it differs, explain that a lint script already exists and ask before replacing it.

Preserve the existing formatting of `package.json` as much as possible.

## Safety Rules

- Do not overwrite an existing Stylelint config without confirmation.
- Do not overwrite an existing `lint` package script without confirmation.
- Do not remove existing dependencies, scripts, or settings.
- Preserve project-specific files wherever possible.
- Prefer minimal changes.
- Install only missing packages.

## Expected Result

After the skill runs successfully, the project should have:

- `stylelint` installed as a dev dependency, unless already installed.
- `stylelint-order` installed as a dev dependency, unless already installed.
- A root `stylelint.config.mjs` copied from this skill.
- A package script:

```json
"lint": "stylelint \"src/**/*.css\" --fix"
```

## Verification

After making changes, verify:

1. `package.json` is valid JSON.
2. `stylelint.config.mjs` exists in the project root.
3. The `lint` script exists in `package.json`.
4. `stylelint` is installed.
5. `stylelint-order` is installed.

When possible, run the Stylelint script.

For npm:

```bash
npm run lint
```

For pnpm:

```bash
pnpm lint
```

For yarn:

```bash
yarn lint
```

For bun:

```bash
bun run lint
```

If linting errors occur, explain them clearly instead of guessing.