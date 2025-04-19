# Tmux Cheat Sheet for Ubuntu

A quick reference guide for using tmux on Ubuntu systems.

## Installation

```bash
sudo apt update
sudo apt install tmux
```

## Basic Commands

| Command | Description |
|---------|-------------|
| `tmux` | Start a new tmux session |
| `tmux new -s name` | Start a new named session |
| `tmux ls` | List all sessions |
| `tmux attach` | Attach to last session |
| `tmux attach -t name` | Attach to specific session |
| `tmux kill-session -t name` | Kill a specific session |
| `tmux kill-server` | Kill all sessions |

## Key Bindings (prefix is Ctrl+b)

### Session Management
| Shortcut | Description |
|----------|-------------|
| `prefix d` | Detach from session |
| `prefix $` | Rename session |
| `prefix s` | List sessions for selection |



## Common Configuration

Add to `~/.tmux.conf`:

```bash
# Change prefix to Ctrl+a
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

## Auto-start tmux on SSH login

Add to `~/.bashrc`:

```bash
# Auto-start tmux on SSH login
if [[ -n "$SSH_CONNECTION" ]] && [[ -z "$TMUX" ]]; then
    tmux attach -t ssh_tmux || tmux new -s ssh_tmux
fi
```

## Auto-start Application in tmux

```bash
#!/bin/bash
# Save as ~/start_app.sh and make executable with: chmod +x ~/start_app.sh

# Start a detached session
tmux new-session -d -s myapp

# Run commands in that session
tmux send-keys -t myapp 'cd /path/to/app' C-m
tmux send-keys -t myapp './start_script.sh' C-m

# To run at boot, add to crontab with: crontab -e
# @reboot /home/username/start_app.sh
```

---

For more detailed information, see `man tmux` or visit [tmux GitHub](https://github.com/tmux/tmux).
