# tmux-conf


# Useful tmux commands
First of all, launch tmux: `tmux`

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
- `tmux attach <number>`: attach session (specified by the number)

## Divide
- Ctrl+a + |: divide screen (vertical)
- Ctrl+a + -: divide screen (horizontal)
- Ctrl+a + h/j/k/l: move between panes
- Alt + \<ArrowKey\>: move between panes
- Ctrl+a + q + \<number\>: see pane number and move between panes
- Ctrl+a + H/J/K/L: resize panes

## Copy / scroll
- Ctrl+a + \<ESC\>: Copy mode (use vim commands to scroll)
  - Ctrl+f: page down (front page)
  - Ctrl+b: page up (back page)
  - Ctrl+d: half page down
  - Ctrl+u: half page up
- In copy mode, `v` to select region. `y` to copy and `q` to exit copy mode. Then Ctrl+a + ] to paste.

## Other tips
- If you press Ctrl+s by mistake, it will freeze. Ctrl+q to unfreeze.
- On a nested tmux, use Ctrl+a + a + \<command\>.
- : change default directory for new windows.

# References
- https://yesmeck.github.io/tmuxrc/
- https://github.com/yesmeck/tmuxrc/blob/master/tmux.conf
- https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/
