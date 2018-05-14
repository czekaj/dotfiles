# dotfiles
My zsh, vim, etc.
---

### Initialize new `dotfiles` in `$HOME/.dotfiles` while tracking `$HOME`:

```bash
git init --bare $HOME/.dotfiles

# ZSH:
echo "alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'" >> $HOME/.zshrc
source $HOME/.zshrc
# BASH:
echo "alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'" >> $HOME/.bashrc
source $HOME/.bashrc

dotfiles config status.showUntrackedFiles no
```

### Working with the local `dotfiles` repo:

```bash
dotfiles status     
dotfiles add .vimrc
dotfiles commit -m "Add .vimrc"

# create new repo on Github, call it dotfiles
dotfiles remote add origin git@github.com:<your_github_login>/dotfiles.git
dotfiles push --set-upstream origin master

dotfiles add .zshrc
dotfiles commit -m "Add .zshrc"
dotfiles push
```

### Checking out `dotfiles` on a new machine:

```bash
git clone --bare git@github.com:czekaj/dotfiles.git $HOME/.dotfiles
function dotfiles {
   /usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME $@
}
mkdir -p .config-backup
dotfiles checkout
if [ $? = 0 ]; then
  echo "Checked out dotfiles.";
  else
    echo "Backing up pre-existing config files.";
    dotfiles checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | xargs -I{} mv {} .config-backup/{}
fi;
dotfiles checkout
dotfiles config status.showUntrackedFiles no
```

## Bootstrap .dotfiles

### Download dependencies

Assuming you have ZSH installed (standard on OSX):

```bash
# oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
# oh-my-zsh auto-suggestions plugin
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# vim plugin manager
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
vim +PluginInstall +qall
```

### Use ⌘+k to clear screen inside tmux on OSX

Configure iTerm2:

```yaml
Keyboard shortcut: ⌘+k
Action: Send Hex Code
value: 0x0c
```
