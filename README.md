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

## Few `st` keybindings

Default:
- Ctrl + Shift
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




