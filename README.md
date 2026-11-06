# bryan's tmux conf

Config: `~/.tmux.conf`. Plugins are managed by tpm and live in `~/.tmux/plugins/`.

Prefix is `Alt-Space` (`M-Space`), not `Ctrl-b`.

## quickstart (new machine)

1. **Install tmux:**
   ```sh
   brew install tmux        # macOS
   sudo apt install tmux    # Debian/Ubuntu
   ```
2. **Clone this repo to `~/.tmux` with submodules** (plugins live under `~/.tmux/plugins/`):
   ```sh
   git clone --recursive git@github.com:AngKuKuehs/tmux.git ~/.tmux
   ```
   If you already cloned without `--recursive`, run `git -C ~/.tmux submodule update --init --recursive`.
3. **Copy the config to `~/.tmux.conf`.** The config is not in this repo, so copy it from your current machine:
   ```sh
   scp <old-machine>:~/.tmux.conf ~/.tmux.conf
   ```
4. **Start tmux and install plugins** (skip if `~/.tmux/plugins/*` is already populated):
   ```sh
   tmux new -s main
   ```
   then press `Alt-Space` `I` (prefix + `I`) to let tpm fetch the plugins.
5. **Sanity check:** `Alt-Space` `r` reloads the config. `Ctrl-h/j/k/l` should move between panes, and `Alt-Space` `%` splits right.

If `Ctrl-h/j/k/l` does nothing inside nvim, make sure nvim is set up for vim-tmux-navigator too (see [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator)).

## plugins
 - `tmux-plugins/tpm` - plugin manager. `prefix` + `I` installs, `prefix` + `U` updates.
 - `tmux-plugins/tmux-sensible` - baseline options (`escape-time 0`, `history-limit 50000`, `display-time 4000`, `status-interval 5`, etc.). Does not override settings in `~/.tmux.conf`.
 - `christoomey/vim-tmux-navigator` - seamless `Ctrl-h/j/k/l` between tmux panes and nvim splits (see below).

## vim-style navigation
Root-table bindings from vim-tmux-navigator, no prefix needed:

| key | action |
|---|---|
| `Ctrl-h` | move to pane left (passes through to nvim/vim/fzf if running there) |
| `Ctrl-j` | move to pane below |
| `Ctrl-k` | move to pane above |
| `Ctrl-l` | move to pane right |

Same four keys in copy mode (vi mode is on, `mode-keys vi`) move between panes.

Prefix versions (always move focus, even in nvim):

| key | action |
|---|---|
| `prefix` `h` | **break the current pane out into its own window** (hides it) |
| `prefix` `j` / `k` | select pane down / up |
| `prefix` `l` | select pane right |

Note: `prefix` + `h` is *hide*, not *move left*. Left movement is `Ctrl-h`.

## custom bindings (`~/.tmux.conf`)

| key | action |
|---|---|
| `prefix` `r` | reload `~/.tmux.conf` |
| `prefix` `h` | break pane into a new window (hide it) |
| `prefix` `s` | join the hidden pane back into the current window (`join-pane -s !`) |
| `prefix` `H` | shrink pane to the left by 1 (repeatable) |
| `prefix` `L` | grow pane to the right by 1 (repeatable) |

`prefix` `H` and `L` are repeatable (`-r`), so you can hold the prefix and press them several times.

## extras from the plugins
- `prefix` `C-l` (from vim-tmux-navigator) sends a real `Ctrl-l` to the pane, e.g. to clear the screen when `Ctrl-l` is captured by tmux.
- `prefix` `I` / `U` (from tpm) install and update plugins.

## useful defaults still active
- `prefix` `c` new window, `prefix` `n` / `p` next / previous window
- `prefix` `%` / `prefix` `"` split right / down
- `prefix` `x` kill pane, `prefix` `z` zoom pane
- `prefix` `w` window/pane tree
- `prefix` `[` copy mode, `prefix` `]` paste
- `prefix` `?` list all keys
