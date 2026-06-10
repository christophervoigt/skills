---
name: setup-prettier
description: Set up Prettier in a Node-based frontend project. Use when a user asks to add or configure Prettier, including optional Astro support, package scripts, .prettierignore, and VS Code settings.
---

# Prettier Setup Skill

Use this skill when the user wants to set up Prettier in a JavaScript, TypeScript, Astro, or frontend project.

This is a global skill, not project-specific. It assumes this skill folder contains a reusable `prettier.config.mjs` file that should be copied into the root of the target project.

## Goal

Set up Prettier in the current project with a shared configuration, optional Astro support, a project-specific `.prettierignore`, a package script, and optional VS Code integration.

## Preconditions

Before making changes:

1. Confirm that the current working directory is the project root.
2. Check whether a `package.json` file exists.
3. If no `package.json` exists, stop and explain that this skill only applies to Node-based projects with a `package.json`.

## Process

### 1. Inspect `package.json`

Read the root `package.json`.

Determine:

- Whether `prettier` is already installed.
- Whether `astro` is installed.
- Which package manager appears to be used.

Check all dependency sections:

- `dependencies`
- `devDependencies`
- `peerDependencies`
- `optionalDependencies`

Treat `astro` as installed if it appears in any of these sections.

Treat `prettier` as installed if it appears in any of these sections.

Package manager detection:

1. If `pnpm-lock.yaml` exists, use `pnpm`.
2. Else if `yarn.lock` exists, use `yarn`.
3. Else if `bun.lockb` or `bun.lock` exists, use `bun`.
4. Else if `package-lock.json` exists, use `npm`.
5. Else default to `npm`.

### 2. Install Prettier if needed

If `prettier` is not already installed, install it as a dev dependency.

Use the detected package manager:

For pnpm:

```bash
pnpm add -D prettier
```

For yarn:

```bash
yarn add -D prettier
```

For bun:

```bash
bun add -d prettier
```

For npm:

```bash
npm install -D prettier
```

Do not reinstall Prettier if it already exists in `package.json`.

### 3. Install Astro Prettier plugin if needed

If `astro` is installed, check whether `prettier-plugin-astro` is already installed.

If `astro` is present and `prettier-plugin-astro` is missing, install it as a dev dependency.

Use the detected package manager:

For pnpm:

```bash
pnpm add -D prettier-plugin-astro
```

For yarn:

```bash
yarn add -D prettier-plugin-astro
```

For bun:

```bash
bun add -d prettier-plugin-astro
```

For npm:

```bash
npm install -D prettier-plugin-astro
```

### 4. Copy `prettier.config.mjs`

Copy the `prettier.config.mjs` file from this skill folder into the root of the project.

The target path must be:

```text
./prettier.config.mjs
```

If a `prettier.config.mjs` already exists in the project root:

1. Do not overwrite it silently.
2. Compare the existing file with the skill's version.
3. If they differ, explain that a Prettier config already exists and ask before replacing it.
4. If they are identical, leave it unchanged.

### 5. Add Astro plugin to the Prettier config

If `astro` is installed, ensure the copied root `prettier.config.mjs` includes this property at the start of the exported config object:

```js
plugins: ['prettier-plugin-astro'],
```

The property should be the first property inside the config object.

For example:

```js
export default {
  plugins: ['prettier-plugin-astro'],
  semi: true,
  singleQuote: true,
}
```

Do not add the plugin twice.

If the config already contains a `plugins` property:

- If it already includes `'prettier-plugin-astro'`, leave it unchanged.
- If it does not include `'prettier-plugin-astro'`, add it to the start of the existing plugins array.

Example:

```js
plugins: ['prettier-plugin-astro', 'some-other-plugin'],
```

### 6. Create `.prettierignore`

Create an empty `.prettierignore` file in the project root if it does not already exist.

Target path:

```text
./.prettierignore
```

If `.prettierignore` already exists, leave it unchanged.

Do not populate this file by default because ignore rules are project-specific.

### 7. Add package script

Update the root `package.json`.

Add the following script:

```json
"prettier": "prettier --write \"src/**/*.{astro,js,ts,json,css}\""
```

If the `scripts` object does not exist, create it.

If a `prettier` script already exists:

1. Do not overwrite it silently.
2. If it is already `"prettier --write \"src/**/*.{astro,js,ts,json,css}\""`, leave it unchanged.
3. If it differs, explain that a Prettier script already exists and ask before replacing it.

Preserve the existing formatting of `package.json` as much as possible.

### 8. Update VS Code settings if present

Check whether a `.vscode` folder exists.

If `.vscode/settings.json` exists, add this setting:

```json
"prettier.configPath": "prettier.config.mjs"
```

Add it at the end of the settings object.

If the setting already exists:

- If it already points to `"prettier.config.mjs"`, leave it unchanged.
- If it points somewhere else, do not overwrite it silently. Explain the conflict and ask before changing it.

If `.vscode` exists but `settings.json` does not exist, do not create it automatically.

If `.vscode` does not exist, do nothing.

## Safety Rules

- Do not overwrite an existing Prettier config without confirmation.
- Do not overwrite an existing `prettier` package script without confirmation.
- Do not overwrite an existing VS Code `prettier.configPath` setting without confirmation.
- Do not remove existing dependencies, scripts, or settings.
- Do not create `.vscode/settings.json` unless explicitly asked.
- Preserve project-specific files wherever possible.
- Prefer minimal changes.

## Expected Result

After the skill runs successfully, the project should have:

- `prettier` installed as a dev dependency, unless already installed.
- `prettier-plugin-astro` installed as a dev dependency when Astro is used.
- A root `prettier.config.mjs` copied from this skill.
- Astro plugin support in the Prettier config when Astro is used.
- An empty `.prettierignore` file, unless one already existed.
- A package script:

```json
"prettier": "prettier --write \"src/**/*.{astro,js,ts,json,css}\""
```

- A VS Code setting, only if `.vscode/settings.json` already existed:

```json
"prettier.configPath": "prettier.config.mjs"
```

## Verification

After making changes, verify:

1. `package.json` is valid JSON.
2. `prettier.config.mjs` exists in the project root.
3. `.prettierignore` exists in the project root.
4. The `prettier` script exists in `package.json`.
5. If Astro is installed, `prettier-plugin-astro` is installed and configured.
6. If `.vscode/settings.json` was changed, it remains valid JSON or valid JSONC, depending on its original format.

When possible, run the Prettier script.

For npm:

```bash
npm run prettier
```

For pnpm:

```bash
pnpm prettier
```

For yarn:

```bash
yarn prettier
```

For bun:

```bash
bun run prettier
```

If formatting errors occur, explain them clearly instead of guessing.
