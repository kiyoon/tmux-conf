# tmux-conf

## Installation
```bash
mkdir ~/bin
cd ~/bin
git clone https://github.com/kiyoon/tmux-conf
cd
ln -s bin/tmux-conf/.tmux.conf

# Plugins
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
# start a server but don't attach to it
tmux start-server
# create a new session but don't attach to it either
tmux new-session -d
# install the plugins
~/.tmux/plugins/tpm/scripts/install_plugins.sh
# killing the server is not required, I guess
tmux kill-server
```

# Useful tmux commands
First of all, launch tmux: `tmux`  
or, `tmux new -s <session_name>`

## Create and navigate
- Ctrl+a + c: create window
- `echo $TMUX_PANE`: see pane number (%0, %1, ..)
- `echo ${TMUX_PANE##%}`: see pane number (0, 1, ..)
- `exit`: exit window
- Ctrl+a + n: next window
- Ctrl+a + p: previous window
- Ctrl+a + \<number\>: jump to the window number
- Ctrl+a + ,: change window title
- Ctrl+Shift+s+Left / Right: re-order windows


## Detach and resume
- Ctrl+a + d: detach tmux session
- `tmux ls`: list sessions
- `tmux attach` or `tmux a`: attach session
- `tmux attach -t <session_name>`: attach session (specified by the number or name)

## Divide
- Ctrl+a + |: divide screen (vertical)
- Ctrl+a + -: divide screen (horizontal)
- Ctrl+a + h/j/k/l: move between panes
- Alt + \<ArrowKey\>: move between panes
- Ctrl+a + q + \<number\>: see pane number and move between panes
- Ctrl+a + H/J/K/L: resize panes

You can even use mouse right click.

## Copy / scroll
- Ctrl+a + \[: Copy mode (use vim commands to scroll)
  - Ctrl+f: page down (front page)
  - Ctrl+b: page up (back page)
  - Ctrl+d: half page down
  - Ctrl+u: half page up
- In copy mode, `v` to select region. `y` to copy and `q` to exit copy mode. Then Ctrl+a + ] to paste.
- You can use mouse drag to copy.
- Ctrl+a + =: see buffer list


## Other tips
- If you press Ctrl+s by mistake, it will freeze. Ctrl+q to unfreeze.
- On a nested tmux, use Ctrl+a + a + \<command\>.
- Ctrl+a `:attach -c /new/dir`: change default directory for new windows.


## Plugins
- Ctrl+a + I: Install plugins
- Ctrl+a + U: Update plugins
- (tmux-yank): Ctrl+a + y: copy bash command line
- (tmux-yank): Ctrl+a + Y: copy PWD

# Advanced: scripting with tmux
- `tmux new-session -d -s <session_name>`: start a session in detached mode.
- `tmux new-window -t <session_name>:<window_index>`: create a window. You can omit the index. You can add the command at the end, but it will automatically be closed when the command finishes.
- `tmux send-keys -t <session_name>:<window_index>.<pane_index> C-u 'some -new command' Enter`: write in a pane. You can use left/right instead of the pane index.
- `tmux kill-window -t <session_name>:<window_index>`: kill window.
- `tmux kill-session -t <session_name>`: kill all windows and session.
- `tmux display -pt "${TMUX_PANE:?}" '#{pane_index}'`: get current pane index
- `tmux list-panes -s -F '#D #{pane_pid} #{pane_current_command}'`: list pane's unique identifier, pid, and the commands. (`-s` for current session, `-a` for all sessions.)


## Example: run batch job

Below will create 3 windows and run python commands like:  
- `CUDA_VISIBLE_DEVICES=0 python train.py --arg 1`
- `CUDA_VISIBLE_DEVICES=1 python train.py --arg 2`
- `CUDA_VISIBLE_DEVICES=2 python train.py --arg 3`

```bash
#!/bin/bash

sess="session_name"

tmux new -d -s "$sess"

for window in {0..2}
do
    # Window 0 already exists so don't make it again
    if [ $window -ne 0 ]
    then
        tmux new-window -t "$sess:$window"
    fi

    command="CUDA_VISIBLE_DEVICES=$window python train.py --arg $((window+1))"
    tmux send-keys -t "$sess:$window" "$command" Enter
done
```


# References
- https://yesmeck.github.io/tmuxrc/
- https://github.com/yesmeck/tmuxrc/blob/master/tmux.conf
- https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/
- https://unix.stackexchange.com/questions/318281/how-to-copy-and-paste-with-a-mouse-with-tmux
