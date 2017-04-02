# Installation

```
brew install tmux
```

```
// in terminal
tmux
```

# Pane

`CTRL + B` + `%` - new vertical pane

`CTRL + B` + `"` - new horizontal pane

`CTRL + B` + `arrow key` - navigate to pane

`CTRL + D` - exit pane

`CTRL + B` + `?` - help menu

# window

window is a collection of panes

`CTRL + C` - create new window

`CTRL + B` + `p`/`n` - navigate to previous/next window

# Session

preserve the windows

`CTRL + B` + `d` - detach the session

```
// attach the most recent session
tmux attach

// list current sessions
tmux ls

// attach a particular session
tmux attach -t 0
```

rename session

`CTRL + B` + `$` - rename the session

```
tmux attach -t newName
```

`CTRL + B` + `s` - switch session in session


```
// rename session in command line
tmux rename-session -t oldName newName
```

```
// new a session with name
tmux new -s sessionName
```

close session
```
tmux kill-session -t sessionName
```

# Zoom and resize

`CTRL + B` + `z` - zoom in/out pane

`CTRL + B` + `:` - command mode

```
// in command mode
:resize-pane -D 30
:resize-pane -U 80
:resize-pane -L
:resize-pane -R
```

# conf file

```
man tmux
```

```
vim ~/.tmux.conf
```

```
# Rebind prefix key to C-a
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix

# Pretty colors
set -g status-bg blue
set -g status-fg white
```

close all sessions and reopen or

```
// tmux command mode
:source-file ~/.tmux.conf
```


```
// ~/.tmux.conf
# Easy reloading
bind R source-file ~/.tmux.conf
```

# Copy mode

`CTRL + B` + `[` - enter copy mode

use `arrow key` to up and down

`q` - leave copy mode


## emacs

```
// tmux command mode
:set-window-option mode-keys emacs
```

copy mode

`CTRL + Space` - set mark
`arrow key` - select
`CTRL + w` - copy

## vi

```
// tmux command mode
:set-window-option mode-keys vi
```

copy mode
`Space` - set mark
`arrow key` - select
`Enter` - copy

# Mouse mode

```
// tmux command mode
:set mouse on
```

```
// ~/.tmux.conf
# Mouse mode keys
bind m set -g mouse on
bind M set -g mouse off
```

prefix key + m - open mouse mode
prefix key + M - close mouse mode

# Workflow using tmux scripts

```
touch tmuxlaunch.sh
chmod u+x tmuxlaunch.sh
vim tmuxlaunch.sh
```

```
// tmuxlauch.sh
SESSION=dev
tmux new-session -d -s $SESSION
tmux new-window -t $SESSION:1 -n 'webserver'

tmux select-window -t $SESSION:1
tmux split-window -h
tmux send-keys 'cd examples/react; python -m SimpleHTTPServer' C-m

tmux attach -t $SESSION
```

```
sh tmuxlaunch.sh
tmux ls
tmux attach -t dev
```

# Share a tmux session with SSH

```
tmux new-session -s pair
```

# History in tmux sessions

manager history between tmux sessions and multiple windows

```
// ~/.bashrc
shopt -s histappend
shopt -s histreedit
shopt -s histverify
HISTCONTROL='ignoreboth'
PROMPT_COMMAND="history -a;history -c;history -r; $PROMPT_COMMAND"
```

shell opt

