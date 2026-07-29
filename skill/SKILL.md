# ScriptCat Manager

Manage userscripts in Brave's ScriptCat extension.

## Tool
`$HOME/scripts/scriptcat-mgr`

## Dependencies
- `python-plyvel` (installed via pacman)
- Brave browser with ScriptCat Beta extension

## Commands

| Command | Description |
|---|---|
| `scriptcat-mgr install <file>` | Install a `.user.js` file |
| `scriptcat-mgr install -u <url>` | Install from a URL |
| `scriptcat-mgr list` (alias: `ls`) | List installed scripts with versions |
| `scriptcat-mgr remove <name>` (alias: `rm`) | Remove by name (partial match) |
| `scriptcat-mgr update [name]` | Update script(s) from their download URLs |
| `scriptcat-mgr backup <dir>` | Export all scripts as `.user.js` files |
| `scriptcat-mgr restore <dir>` | Import all `.user.js` files from a directory |

## Examples

```bash
# Install
scriptcat-mgr install ~/scripts/userscripts/Reddit++.user.js
scriptcat-mgr install -u https://example.com/script.user.js

# List
scriptcat-mgr ls

# Remove
scriptcat-mgr rm "Reddit NSFW"

# Update all scripts
scriptcat-mgr update

# Update specific script
scriptcat-mgr update "Reddit++"

# Backup/restore
scriptcat-mgr backup ~/scripts/userscripts/
scriptcat-mgr restore ~/scripts/userscripts/
```

## Notes
- Kills Brave before writing to LevelDB — save your work first
- Uses the same key format (`script:`, `scriptCode:`, `compiled_resource:`) as ScriptCat
