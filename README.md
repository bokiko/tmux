<div align="center">

# Tmux Cheat Sheet

<h3>Quick reference for tmux terminal multiplexer on Ubuntu.</h3>

<p>
  <img src="https://img.shields.io/badge/Platform-Ubuntu-E95420" alt="Ubuntu" />
  <img src="https://img.shields.io/badge/Tool-tmux-green" alt="tmux" />
  <img src="https://img.shields.io/badge/Type-Cheat%20Sheet-blue" alt="Cheat Sheet" />
</p>

</div>

---

## What is tmux?

**tmux** is a terminal multiplexer that lets you run multiple terminal sessions inside a single window. Perfect for SSH connections where you need persistent sessions that survive disconnections.

> **Note:** I personally prefer [GNU Screen](https://github.com/bokiko/screen) for its simplicity, but tmux has more advanced features.

---

## Installation

```bash
sudo apt update && sudo apt install tmux
```

---

## Quick Reference

### Basic Commands

| Command | Description |
|---------|-------------|
| `tmux` | Start a new session |
| `tmux new -s name` | Start a new named session |
| `tmux ls` | List all sessions |
| `tmux attach` | Attach to last session |
| `tmux attach -t name` | Attach to specific session |
| `tmux kill-session -t name` | Kill a specific session |
| `tmux kill-server` | Kill all sessions |

### Key Bindings (prefix: `Ctrl+b`)

| Shortcut | Description |
|----------|-------------|
| `Ctrl+b d` | Detach from session |
| `Ctrl+b $` | Rename session |
| `Ctrl+b s` | List sessions for selection |
| `Ctrl+b c` | Create new window |
| `Ctrl+b n` | Next window |
| `Ctrl+b p` | Previous window |
| `Ctrl+b %` | Split vertically |
| `Ctrl+b "` | Split horizontally |

---

## Configuration

Add to `~/.tmux.conf`:

```bash
# Change prefix to Ctrl+a (more ergonomic)
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# Enable mouse support
set -g mouse on

# Increase history limit
set -g history-limit 10000

# Improve colors
set -g default-terminal "screen-256color"

# Better splitting
bind | split-window -h
bind - split-window -v
```

---

## Auto-Start on SSH Login

Add to `~/.bashrc`:

```bash
if [[ -n "$SSH_CONNECTION" ]] && [[ -z "$TMUX" ]]; then
    tmux attach -t ssh_tmux || tmux new -s ssh_tmux
fi
```

---

## Auto-Start Application

Create `~/start_app.sh`:

```bash
#!/bin/bash
tmux new-session -d -s myapp
tmux send-keys -t myapp 'cd /path/to/app' C-m
tmux send-keys -t myapp './start_script.sh' C-m
```

Make executable and add to crontab:
```bash
chmod +x ~/start_app.sh
crontab -e
# Add: @reboot /home/username/start_app.sh
```

---

## More Information

- Official docs: `man tmux`
- GitHub: [tmux/tmux](https://github.com/tmux/tmux)

---

<div align="center">

Built by [@bokiko](https://github.com/bokiko)

</div>
