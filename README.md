# App Catalog

This repository holds the **app catalog** for the package manager app. It is a single JSON file (`apps.json`) fetched at runtime, so the app list can be updated **without releasing a new version** of the app.

---

## How It Works

Every time the app launches, it fetches this file directly from GitHub:

```
https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/apps.json
```

- If online → always gets the latest version
- If offline → uses the last cached version stored locally
- The app compares versions and only updates the local cache when something changed

---

## File Structure

```json
{
  "version": "1.0.0",
  "updatedAt": "2026-02-20",
  "categories": {
    "Category Name": [
      { "id": "Publisher.AppName", "name": "Display Name", "icon": "🎯" },
      { "id": "Publisher.AppName", "name": "Display Name", "icon": "🎯", "isNew": true }
    ]
  }
}
```

### Top-level fields

| Field | Description |
|---|---|
| `version` | Catalog version — **must be bumped** on every change |
| `updatedAt` | Date of the last update (`YYYY-MM-DD`) |
| `categories` | Object where each key is a category name and the value is an array of apps |

### App entry fields

| Field | Required | Description |
|---|---|---|
| `id` | ✅ | Winget package ID — must be exact |
| `name` | ✅ | Display name shown in the UI |
| `icon` | ✅ | Emoji shown next to the app |
| `isNew` | ❌ | Set to `true` to feature the app in the **New Apps** section |

---

## Available Categories

| Category | Description |
|---|---|
| `Browsers` | Web browsers |
| `Development` | IDEs, languages, runtimes |
| `Developer Tools` | Dev utilities, DB tools, terminals |
| `Communication` | Messaging and video call apps |
| `Media` | Media players, editors, viewers |
| `Gaming` | Game launchers and platforms |
| `Security` | Antivirus, password managers, encryption |
| `Compression` | Archive tools |
| `Utilities` | System tools and productivity utilities |
| `Office & Productivity` | Office suites and note-taking apps |
| `Cloud Storage` | Cloud sync and storage clients |
| `VPN & Privacy` | VPN clients and privacy tools |
| `AI Tools` | AI assistants and local LLM tools |
| `Design` | Design and creative tools |

---

## Adding a New App

### 1. Find the winget ID

```bash
winget search <app name>
```

Example:
```
> winget search testapp

Name     Id                  Version
---------------------------------------
TestApp  Publisher.TestApp   1.0.0
```

Use the value from the **Id** column.

### 2. Add it to `apps.json`

Pick the right category and add your entry:

```json
{ "id": "Publisher.TestApp", "name": "Test App", "icon": "🧪" }
```

To feature it in the New Apps section, add `"isNew": true`:

```json
{ "id": "Publisher.TestApp", "name": "Test App", "icon": "🧪", "isNew": true }
```

### 3. Bump the version

Always increment the `version` field when making changes:

```json
"version": "1.0.0"  →  "version": "1.1.0"
```

### 4. Update the date

```json
"updatedAt": "2026-02-20"
```

### 5. Push to GitHub

The app will pick up the changes automatically on next launch. No rebuild needed.

---

## Removing the "New" Badge

Once an app is no longer new, remove the `isNew` flag (or set it to `false`) and bump the version:

```json
{ "id": "Publisher.TestApp", "name": "Test App", "icon": "🧪" }
```
