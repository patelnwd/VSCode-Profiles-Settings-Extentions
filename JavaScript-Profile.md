# VS Code — JavaScript Development Profile (Full Step‑by‑Step Guide)

> A complete, copy‑ready markdown guide to create a VS Code *profile* tuned for JavaScript projects (Windows). Includes extension install commands, `settings.json`, workspace recommendations, ESLint/Prettier setup, snippets, and automation tips.

---

## Table of contents

1. [Why use a Profile?](#why-use-a-profile)
2. [Prerequisites (Windows)](#prerequisites-windows)
3. [Quick checklist — what you'll end up with](#quick-checklist)
4. [Step 0 — Verify `code` CLI availability](#step-0---verify-code-cli-availability)
5. [Step 1 — Create a new profile (UI)](#step-1---create-a-new-profile-ui)
6. [Step 2 — Extensions to install (with commands)](#step-2---extensions-to-install-with-commands)
7. [Step 3 — User / Profile `settings.json` (recommended)](#step-3---user--profile-settingsjson-recommended)
8. [Step 4 — Workspace `.vscode/extensions.json` & workspace `settings.json`](#step-4---workspace-vscodeextensionsjson--workspace-settingsjson)
9. [Step 5 — Project lint/format toolchain (ESLint + Prettier)](#step-5---project-lintformat-toolchain-eslint--prettier)
10. [Step 6 — Snippets (useful JS snippets)](#step-6---snippets-useful-js-snippets)
11. [Step 7 — Export / Share / Reproduce the profile (automation)](#step-7---export--share--reproduce-the-profile-automation)
12. [Step 8 — Troubleshooting & tips](#step-8---troubleshooting--tips)
13. [Appendix: PowerShell automation script (copy & run)](#appendix-powershell-automation-script-copy--run)

---

# Why use a Profile?

VS Code *profiles* let you keep a focused set of settings, UI state, extensions, and keybindings for a particular workflow. For JavaScript development, a profile helps you quickly switch between e.g., "JavaScript Dev", "Writing", and "Debugging" setups without changing your global VS Code preferences.

---

# Prerequisites (Windows)

* VS Code (stable) installed. Prefer the **System Installer** or the user installer that puts `code` on PATH.
* Node.js + npm (for project tools like ESLint/Prettier).
* PowerShell or Windows Terminal for running install scripts.
* (Optional) Git installed if you want GitLens/intellisense to work fully.

---

# Quick checklist — what you'll end up with

* A named VS Code profile (example: **JavaScript Dev**)
* A curated list of extensions installed with CLI commands
* A profile-level `settings.json` tuned for JS (format on save, ESLint integration, Prettier default formatter, editor preferences)
* Workspace recommendations (`.vscode/extensions.json`) so teammates get the same extension hints
* Project-level ESLint + Prettier config and `package.json` scripts
* A short PowerShell script to automate installing the extensions and copying the `settings.json` if you want reproducibility

---

## Step 0 - Verify `code` CLI availability

1. Open PowerShell and run:

```powershell
code -v
```

2. If you see a version number, the CLI is available. If not, either reinstall VS Code and choose the option to add `code` to PATH or manually add the VS Code bin folder to PATH. On most Windows installs the CLI path is:

```
C:\Users\<your-user>\AppData\Local\Programs\Microsoft VS Code\bin
```

Add that to your PATH and reopen PowerShell.

---

## Step 1 - Create a new profile (UI)

1. Open VS Code.
2. Open the **Command Palette**: `Ctrl+Shift+P`.
3. Type **Profiles: Create Profile...** and press Enter.
4. Choose a name (e.g. `JavaScript Dev`) and whether to start from default or create from current settings.
5. Switch to your profile via **Profiles: Switch Profile...** and select your new `JavaScript Dev` profile.
6. While that profile is active, any changes to settings/extensions will be stored in that profile.

> Tip: You can open the Profile menu from the Accounts/Profile icon in the Status Bar (bottom-left) to quickly switch or export.

---

## Step 2 - Extensions to install (with commands)

Below is a curated list of essential extensions for JavaScript development together with commands you can paste into PowerShell. Each command uses `--force` so re-running the script will reinstall/update without prompting.

**Core recommended extensions (copy + paste into PowerShell):**

```powershell
code --install-extension esbenp.prettier-vscode --force
code --install-extension dbaeumer.vscode-eslint --force
code --install-extension ms-vscode.vscode-typescript-next --force
code --install-extension christian-kohler.path-intellisense --force
code --install-extension PKief.material-icon-theme --force
code --install-extension aaron-bond.better-comments --force
code --install-extension eamodio.gitlens --force
code --install-extension ritwickdey.LiveServer --force
code --install-extension EditorConfig.EditorConfig --force
code --install-extension VisualStudioExptTeam.vscodeintellicode --force
code --install-extension streetsidesoftware.code-spell-checker --force
```

**What these do (short):**

* `prettier` — opinionated code formatter
* `eslint` — JS/TS linting and fix-on-save integration
* `vscode-typescript-next` — TypeScript/JS next features preview (optional)
* `path-intellisense` — autocompletes filesystem paths
* `material-icon-theme` — nicer file icons
* `better-comments` — highlights TODOs and annotations
* `gitlens` — advanced git insights
* `liveserver` — quick static dev server for front-end files
* `EditorConfig` — keep consistent coding styles across editors
* `intellicode` — AI assist for completions
* `spell-checker` — catches typos in comments/strings

**Bulk install from a text file (PowerShell)**

1. If you save the extension list to `vscode-extensions.txt` (one id per line) then run:

```powershell
Get-Content .\vscode-extensions.txt | ForEach-Object { code --install-extension $_ --force }
```

**Bulk install (WSL / Git-Bash)**

```bash
cat vscode-extensions.txt | xargs -L 1 code --install-extension --force
```

---

## Step 3 - User / Profile `settings.json` (recommended)

Open **Command Palette** → `Preferences: Open Profile Settings (JSON)` (ensure your `JavaScript Dev` profile is selected). Replace or merge with the JSON below.

> **Important:** this JSON is safe to paste as-is. If you already have settings, merge carefully or keep a backup of your existing `settings.json`.

```json
{
  "workbench.colorTheme": "Default Dark+",
  "workbench.iconTheme": "material-icon-theme",

  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.detectIndentation": false,
  "editor.rulers": [80, 100],

  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },

  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,

  "files.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/build": true
  },

  "search.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/build": true
  },

  "javascript.format.enable": false,
  "prettier.requireConfig": true,
  "prettier.singleQuote": true,
  "prettier.trailingComma": "es5",

  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],

  "emmet.includeLanguages": {
    "javascript": "javascriptreact"
  },

  "git.autofetch": true,
  "git.enableSmartCommit": true,

  "terminal.integrated.defaultProfile.windows": "PowerShell",

  "telemetry.telemetryLevel": "off"
}
```

**Why a few of these matter**

* `prettier.requireConfig: true` makes Prettier obey your project `.prettierrc` (recommended for team consistency).
* `javascript.format.enable: false` disables the built‑in formatter so Prettier is the single formatter.
* `editor.codeActionsOnSave` runs ESLint fixes and organizes imports whenever you save.

---

## Step 4 - Workspace `.vscode/extensions.json` & workspace `settings.json`

When sharing code with teammates, add `.vscode/extensions.json` to recommend extensions.

**`.vscode/extensions.json`**

```json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "christian-kohler.path-intellisense",
    "PKief.material-icon-theme",
    "aaron-bond.better-comments",
    "eamodio.gitlens",
    "ritwickdey.LiveServer",
    "EditorConfig.EditorConfig"
  ]
}
```

**Workspace `settings.json`** (optional) — put under `.vscode/settings.json` to override user/profile settings for a specific project:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "eslint.validate": ["javascript", "javascriptreact"]
}
```

---

## Step 5 - Project lint/format toolchain (ESLint + Prettier)

Inside your project folder run:

```bash
npm init -y
npm i -D eslint prettier eslint-config-prettier eslint-plugin-prettier
```

Initialize ESLint quickly:

```bash
npx eslint --init
```

**Example `.eslintrc.json`**

```json
{
  "env": {
    "browser": true,
    "es2021": true,
    "node": true
  },
  "extends": ["eslint:recommended", "plugin:prettier/recommended"],
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "rules": {
    "no-unused-vars": "warn"
  }
}
```

**Example `.prettierrc`**

```json
{
  "singleQuote": true,
  "trailingComma": "es5",
  "tabWidth": 2,
  "semi": true
}
```

Add helpful `package.json` scripts:

```json
"scripts": {
  "lint": "eslint . --ext .js,.jsx,.ts,.tsx",
  "format": "prettier --write ."
}
```

---

## Step 6 - Snippets (useful JS snippets)

Open Command Palette → `Preferences: Configure User Snippets` → `javascript.json` and add a small snippet set. Example `javascript.json`:

```json
{
  "Console log": {
    "prefix": "clg",
    "body": ["console.log('$1')"],
    "description": "console.log snippet"
  },
  "Async Function": {
    "prefix": "asyncfn",
    "body": [
      "async function ${1:name}(${2:args}) {",
      "  try {",
      "    $3",
      "  } catch (err) {",
      "    console.error(err);",
      "  }",
      "}"
    ],
    "description": "async function template"
  }
}
```

---

## Step 7 - Export / Share / Reproduce the profile (automation)

**Export profile (GUI)**

1. Command Palette → `Preferences: Export Current Profile...`
2. Save the exported profile (result is a `.code-profile` file you can share).
3. Import on another machine: Command Palette → `Preferences: Import Profile...` and choose the file.

**Export installed extensions list (CLI)**

```powershell
code --list-extensions > vscode-extensions.txt
```

**Install from list (PowerShell)**

```powershell
Get-Content .\vscode-extensions.txt | ForEach-Object { code --install-extension $_ --force }
```

**Install from list (WSL / Bash)**

```bash
cat vscode-extensions.txt | xargs -L 1 code --install-extension --force
```

**Copy profile `settings.json`**

User settings path on Windows is usually:

```
%APPDATA%\Code\User\settings.json
```

You can copy your prepared `settings.json` file here (back up the original first).

---

## Step 8 - Troubleshooting & tips

* If `code` isn't recognized: ensure the VS Code `bin` folder is on your PATH and restart your terminal.
* If Prettier/ESLint conflicts: set `prettier.requireConfig: true` and add `eslint-config-prettier` to disable formatting rules that clash with Prettier.
* Extensions not applying? Ensure you switched to the correct profile (bottom-left Profile icon) and restart VS Code.
* If `editor.codeActionsOnSave` doesn't trigger ESLint fixes, ensure `"eslint": { "enable": true }` in settings and that ESLint is installed locally in the project (prefer local install).

---

# Appendix: PowerShell automation script (copy & run)

Save the following as `install-vscode-js-profile.ps1` and run it from PowerShell (run as your user, with VS Code closed/open). It will install recommended extensions and copy a `settings.json` file if present.

```powershell
# install-vscode-js-profile.ps1
$extensions = @(
  "esbenp.prettier-vscode",
  "dbaeumer.vscode-eslint",
  "ms-vscode.vscode-typescript-next",
  "christian-kohler.path-intellisense",
  "PKief.material-icon-theme",
  "aaron-bond.better-comments",
  "eamodio.gitlens",
  "ritwickdey.LiveServer",
  "EditorConfig.EditorConfig",
  "VisualStudioExptTeam.vscodeintellicode",
  "streetsidesoftware.code-spell-checker"
)

foreach ($ext in $extensions) {
  Write-Host "Installing: $ext"
  code --install-extension $ext --force
}

# Optional: copy settings.json to user settings (BACKUP first)
$src = Join-Path (Get-Location) 'settings.json'
$dest = Join-Path $env:APPDATA 'Code\User\settings.json'
if (Test-Path $src) {
  if (Test-Path $dest) {
    Copy-Item $dest ($dest + '.backup') -Force
  }
  Copy-Item $src $dest -Force
  Write-Host "Copied settings.json to $dest (backup created if existed)"
} else {
  Write-Host "No settings.json found in current folder. Skipping copy."
}

Write-Host "Done. Restart VS Code to apply changes."
```

> **Warning:** copying `settings.json` will overwrite existing settings — always keep the `.backup` created by the script.

---

## Final notes / next steps

* If you want, I can generate a ready-to-run `install-vscode-js-profile.ps1` including your exact preferences (theme name, specific `settings.json` values) or produce a workspace `.vscode` folder containing `extensions.json` and `settings.json` tailored to one of your projects.

---

*End of guide.*
