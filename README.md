# scriptcat-mgr

CLI tool to manage [ScriptCat](https://github.com/scriptscat/scriptcat) userscripts in Brave Browser via LevelDB.

Instead of relying on ScriptCat's sync or importing scripts one-by-one through the UI, this tool writes directly to the LevelDB storage used by the ScriptCat extension. This makes it fast and scriptable.

## How it works

ScriptCat stores each userscript as 4 keys in a LevelDB at:

```
~/.config/BraveSoftware/Brave-Browser/Default/Local Extension Settings/ndcooeababalnlpkfedmmbbbgkljhpjf/
```

Each installed script gets a UUID and 4 keys:

| Key | Value |
|-----|-------|
| `script:{uuid}` | JSON metadata (name, version, matches, run-at, download URL, etc.) |
| `scriptCode:{uuid}` | The raw `.user.js` source code |
| `compiled_resource:{ns}:{uuid}` | Compiled script cache |
| `value:{uuid}` | Script's stored data (GM_setValue) |

The tool generates a new UUID, populates all 4 keys, and Brave picks them up on next launch.

## Requirements

- **Brave Browser** with **ScriptCat Beta** (`ndcooeababalnlpkfedmmbbbgkljhpjf`)
- **python-plyvel** (`sudo pacman -S python-plyvel`)
- Brave must be **closed** before any write operation (the tool handles this automatically)

## Install

```bash
cp scriptcat-mgr ~/.local/bin/
```

## Usage

### Install from a file

```bash
scriptcat-mgr install my-script.user.js
```

### Install from a URL

```bash
scriptcat-mgr install -u https://example.com/script.user.js
```

### List installed scripts

```bash
scriptcat-mgr list
```

Example output:

```
Name                           Version      Updates
--------------------------------------------------
Reddit++                       2.1.6        yes
CNL Decryptor                  1.0.0        no
Reddit NSFW Unblur                          yes
Focused YouTube                2025-11-18   no
Refined GitHub Notifications   0.6.11       no
Bypass Paywalls Clean - en     4.3.9.7      yes
```

### Remove a script

```bash
scriptcat-mgr remove "Reddit++"
```

Partial name match works — removes all matching scripts.

### Update scripts

```bash
scriptcat-mgr update           # update all
scriptcat-mgr update "Reddit"  # update matching
```

The tool reads each script's `@downloadURL` metadata, fetches the latest version, and rewrites all 4 keys under the same UUID.

### Backup all scripts

```bash
scriptcat-mgr backup ~/script-backups/
```

Writes each script as a `.user.js` file (named by `@name`), one per file.

### Restore from backup

```bash
scriptcat-mgr restore ~/script-backups/
```

Re-imports all `.user.js` files from the directory, assigning fresh UUIDs.

## Alias commands

| Full | Aliases |
|------|---------|
| `install` | `import` |
| `list` | `ls` |
| `remove` | `rm`, `delete` |
| `update` | `upgrade` |

## Files in this repo

| Path | Description |
|------|-------------|
| `scriptcat-mgr` | The CLI tool (Python, single file) |
| `skill/SKILL.md` | OpenCode skill — lets AI assistants manage scripts |
| `userscripts/` | Backups of extracted userscripts |
| `README.md` | This file |

## Included scripts

The following userscripts are managed with this tool:

- **Reddit++** — Reddit enhancement suite
- **Reddit NSFW Unblur** — removes NSFW blur on Reddit
- **Focused YouTube** — removes ads, shorts, and algorithmic distractions
- **Refined GitHub Notifications** — compact, filterable GitHub notification list by antfu
- **Bypass Paywalls Clean** — bypasses paywalls on news sites (Greasy Fork mirror)
- **CNL Decryptor** — decrypts Click'N'Load (CNL2) buttons on link-protection sites (filecrypt, linkcrypt, ncrypt, etc.) using Web Crypto API, no extension needed

## Notes

- The first time you write to LevelDB, the tool creates the database if it doesn't exist.
- `update` only works for scripts that have a `@downloadURL` or `@updateURL` metadata field.
- Greasy Fork blocks Python's `urllib` User-Agent — the tool now sends a Chrome UA header to avoid 403 errors.
