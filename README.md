# Comprehensive Guide to Tmux for Ubuntu

## Table of Contents
- [Introduction](#introduction)
- [Benefits of Using Tmux](#benefits-of-using-tmux)
- [Installation on Ubuntu](#installation-on-ubuntu)
- [Basic Concepts](#basic-concepts)
- [Getting Started](#getting-started)
- [Common Commands & Shortcuts](#common-commands--shortcuts)
- [Session Management](#session-management)
- [Customizing Tmux](#customizing-tmux)
- [Advanced Usage](#advanced-usage)
- [Scripting with Tmux](#scripting-with-tmux)
- [Troubleshooting on Ubuntu](#troubleshooting-on-ubuntu)
- [Cheat Sheet](#cheat-sheet)

## Introduction

Tmux (Terminal Multiplexer) is a powerful tool that allows you to create multiple terminal sessions within a single window. It enables you to run multiple command-line programs simultaneously and detach/reattach sessions - all while preserving your work.

This guide aims to provide a comprehensive overview of tmux on Ubuntu, from installation to advanced usage, making it suitable for both beginners and experienced users.

## Benefits of Using Tmux

- **Session Persistence**: Keep programs running even after disconnecting from SSH or closing your terminal
- **Improved Productivity**: Easily switch between tasks without losing context
- **Better Resource Management**: Use a single SSH connection to manage multiple terminal sessions
- **Workflow Continuity**: Resume your exact terminal state after disconnection or computer restart
- **Team Collaboration**: Share terminal sessions with other users
- **Configuration Flexibility**: Customize key bindings, appearance, and behavior to suit your needs
- **Scriptability**: Automate session creation and management through scripts

## Installation on Ubuntu

```bash
# Update package list
sudo apt update

# Install tmux
sudo apt install tmux

# Verify installation
tmux -V
```

## Basic Concepts

Before diving into commands, it's important to understand the basic concepts of tmux:

- **Session**: A collection of windows that can be detached (continues running in background) and reattached
- **Prefix Key**: The key combination you press before entering a tmux command (default: `Ctrl+b`)

## Getting Started

### Start a New Session
```bash
tmux
```

### Start a Named Session
```bash
tmux new -s session_name
```

### Basic Navigation
All tmux commands start with the prefix key (default: `Ctrl+b`), followed by a command key.

- **List active sessions**: `Ctrl+b s`
- **Detach from current session**: `Ctrl+b d`
- **Get help with commands**: `Ctrl+b ?`

### Reattach to Sessions

```bash
# List available sessions
tmux ls

# Attach to the last session
tmux attach

# Attach to a specific session
tmux attach -t session_name
```

## Common Commands & Shortcuts

### Session Management
- `tmux new -s session_name` - Create a new named session
- `tmux ls` - List all sessions
- `tmux attach -t session_name` - Attach to a specific session
- `tmux kill-session -t session_name` - Kill a specific session
- `Ctrl+b d` - Detach from current session
- `Ctrl+b $` - Rename current session
- `Ctrl+b s` - Show all sessions for selection

## Session Management

Sessions allow you to group processes and detach/reattach to them, preserving your working environment.

### Creating Sessions
```bash
tmux                           # Create a new unnamed session
tmux new -s session_name       # Create a new named session
```

### Managing Sessions
```bash
tmux ls                        # List all sessions
tmux attach                    # Attach to last session
tmux attach -t session_name    # Attach to named session
tmux kill-session -t name      # Kill a specific session
tmux kill-server               # Kill all sessions
```

### Session Navigation and Control
```
Ctrl+b s        # List and select sessions
Ctrl+b $        # Rename current session
Ctrl+b d        # Detach from current session
```

## Customizing Tmux

Tmux can be extensively customized via the configuration file located at `~/.tmux.conf`.

### Basic Configuration Example

```bash
# Create or edit your tmux configuration file
nano ~/.tmux.conf
```

Add the following configurations:

```
# Change the prefix key to Ctrl+a
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# Improve colors
set -g default-terminal "screen-256color"

# Set scrollback buffer size
set -g history-limit 10000

# Enable mouse support
set -g mouse on

# Customize status bar
set -g status-bg black
set -g status-fg white
set -g status-left "#[fg=green]#S"
set -g status-right "#[fg=cyan]%d %b %R"

# Reload configuration
bind r source-file ~/.tmux.conf \; display "Config reloaded!"
```

Apply your changes:
```bash
tmux source-file ~/.tmux.conf
```

## Advanced Usage

### Synchronized Input
Execute commands in all panes simultaneously:
```
# Toggle synchronization
Ctrl+b :setw synchronize-panes on
Ctrl+b :setw synchronize-panes off
```

### Copy Mode in Ubuntu
```
# Enable vi mode for copy in Ubuntu
set-window-option -g mode-keys vi
```

## Scripting with Tmux

Automate session creation with bash scripts:

### Basic Session Script Example
```bash
#!/bin/bash

# Start a new session
tmux new-session -d -s myserver

# Set up commands
tmux send-keys -t myserver 'cd ~/projects' C-m
tmux send-keys -t myserver './start_server.sh' C-m

# Attach to session
tmux attach-session -t myserver
```

### Auto-starting a Process in Tmux on Ubuntu System Boot

To ensure a specific command runs in tmux at system startup:

1. Create a script:
```bash
#!/bin/bash
tmux new-session -d -s myprocess 'command_to_run'
```

2. Add to crontab (using @reboot):
```bash
@reboot /path/to/script.sh
```

## Troubleshooting on Ubuntu

### Common Issues and Solutions

**Issue**: Tmux not showing colors correctly
**Solution**: Add to ~/.tmux.conf:
```
set -g default-terminal "screen-256color"
```

**Issue**: Copy/paste not working in Ubuntu
**Solution**: Install xclip and add to ~/.tmux.conf:
```
# Ubuntu with xclip
sudo apt install xclip
bind -T copy-mode-vi y send-keys -X copy-pipe-and-cancel 'xclip -in -selection clipboard'
```

**Issue**: Cannot detach with Ctrl+b d
**Solution**: Check if prefix key was changed, reset tmux configuration, or use command mode: `Ctrl+b :detach`

**Issue**: Status line not displaying correctly
**Solution**: Reset status line configuration:
```
set -g status-left "[#S] "
set -g status-right "%H:%M %d-%b-%y"
```

## Cheat Sheet

### Essential Commands

| Command | Action |
|---------|--------|
| `tmux` | Start new session |
| `tmux new -s name` | Start new named session |
| `tmux ls` | List sessions |
| `tmux attach -t name` | Attach to named session |
| `tmux kill-session -t name` | Kill named session |
| `Ctrl+b d` | Detach from session |
| `Ctrl+b ?` | List all keybindings |

---

This guide covers the fundamentals of tmux for Ubuntu and should help both beginners and experienced users leverage its powerful features. For more detailed information, refer to the official tmux documentation with `man tmux` or visit the [tmux GitHub page](https://github.com/tmux/tmux).
