# Customized version of `st`

- [Suckless web page](suckless.org)
- [st web page](st.suckless.org)
It is used on Arch Linux with `dwm` window manager.

## Applied (or useful patches)

List of few patches that I applied. Principle was to still keep it minimal.

- [scrollback](https://st.suckless.org/patches/scrollback/)
- [bold_is_not_bright](https://st.suckless.org/patches/bold-is-not-bright/)
- [clipboard](https://st.suckless.org/patches/clipboard/)
- [ligatures](https://st.suckless.org/patches/ligatures/)
- [solarized](https://st.suckless.org/patches/solarized/)

Suggested order of patches application:

1. `st-scrollback-0.9.2.diff` - fundamental patch for scrolling.
2. `st-scrollback-mouse-0.9.2.diff` - applies cleanly only after the base 
    scrollback.
3. `st-scrollback-mouse-altscreen-20220127-2c5edf2.diff` - must be applied
    after both.
4. `st-ligatures-scrollback-20251007-0.9.3.diff` - This must come AFTER
    scrollback. It should be compatible with scrollback options above.
    `sudo pacman -S harfbuzz` a requirement.
5. Color/brightness ptches (these should come afte functionsl):
    1. `st-bold-is-not-bright-20190127-3be4cf1.diff` - nicer visual.
    2. `st-no_bold_colors-20170623-b331da5.diff` - prerequisite for solarized.
    3. `st-solarized-both-20220617-baa9357.diff` - adding both dark/light,
       change with F6.
8. `st-clipboard-0.8.3.diff` - touches x.c and sometimes config.def.h.


## A few `st` keybindings

Default:
- `Ctrl + Shift + {->, <-}` zoom in/out.
- `Ctrl + Shift + Home` zoom reset.
- `Shift + Print` - printscreen.
- `Ctrl + Shift + c` - copy to clipboard.
- `Ctrl + Shift + v` - paste from clipboard.
- `Ctrl + Shift + y` - paste from PRIMARY?  (selpaste).
- `Shift + Insert` - paste from PRIMARY?  (selpaste).
- `middle mouse button` - paste from PRIMARY (selpaste).

Custom/added:
- `Shift + {PageUp, PageDown}` - scrolling through the terminal.

## Agreements

### General

Remotes (`git remote -v`):
- `upstream` - original suckless repository.
- `origin` - my GitHub repository.
- `originGitea` - locally hosted Gitea repository.

Branch structure:
- `master` - clean, vanilla suckless code (updated from upstream).
- `lj_st` - patched/customized version including patches and scripts.

Git rules:
- Build files should never be pushed: `*.o` and `st`.
- Each patch should be its own commit (for easy revert).
- Any manual change that changes functionality or keybinding should be its own
    commit.
- each applied patch should be put into `/patches` folder and added to this 
    README.

### Apply patch

Instead of `patch -si <patch_name>.diff` git commands should be used.
Working tree should always be clean before applying any patch.

Here is an example workflow:
```bash
git apply --check <patch_name>.diff
git apply <patch_name>.diff
git add .
git commit -m "Apply patch_name."
```

Patches usually touch `config.def.h` and not `config.h`. One of the ways to 
solve this issue is to backup `config.h`:
```bash
cp config.h config.h.backup`
```

Remove `config.h`:
```bash
rm config.h
```

Apply patches as describe above and run that now affect `config.def.h` and run:
```bash
make
```

Reapply your custom settings.
Open your backup:
```bash
diff -u config.h.badckup config.h
```
Then merge manually (only the parts you changed!).

[!NOTE] Alternative is to move `config.h` into `config.def.h` and remove
`config.h`, then apply patches.

### Update `master` branch (new version of `st`)

- Fetch the latests suckless code (from `upstream`):
    ```bash
    git fetch upstream
    git merge upstream/master
    # or rebase
    git rebase usptream/master
    ```
- Push updated master to my GitHub (`origin`):
    `git push origin master`

### Update customized version (on `lj_st` branch)

```bash
# Update `master`.
git checkout master
git pull upstream master
git push origin master

# Merge changes.
git checkout lj_st
git rebase master
```




