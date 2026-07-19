# scriptcat-mgr

CLI tool to manage ScriptCat userscripts in Brave via LevelDB.

## Requirements

- `python-plyvel` (`sudo pacman -S python-plyvel`)
- Brave with ScriptCat Beta (`ndcooeababalnlpkfedmmbbbgkljhpjf`)
- Brave must be closed before operations

## Usage

```
scriptcat-mgr install <file>          Install a .user.js file
scriptcat-mgr install -u <url>        Install from URL
scriptcat-mgr list                    List installed scripts
scriptcat-mgr remove <name>           Remove by name (partial match)
scriptcat-mgr update [name]           Update script(s) from download URL
scriptcat-mgr backup <dir>            Backup all scripts to directory
scriptcat-mgr restore <dir>           Restore from backup directory
```

## Files

- `scriptcat-mgr` — the CLI tool (install to PATH)
- `skill/SKILL.md` — OpenCode skill for AI-assisted use
- `userscripts/` — extracted userscript backups

## Install

```bash
cp scriptcat-mgr ~/.local/bin/  # or /usr/local/bin
```
