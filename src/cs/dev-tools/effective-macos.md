# Effective MacOS

## Preparation

### Homebrew

All you need to do is to install [homebrew](https://brew.sh/). It can be installed with the following command:

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

`brew` can help you install softwares:

```shell
brew install visual-studio-code
```

## System

### Trackpad

Tap to click:

```shell
defaults write com.apple.AppleMultitouchTrackpad Clicking -int 1
defaults -currentHost write NSGlobalDomain com.apple.mouse.tapBehavior -int 1
defaults write NSGlobalDomain com.apple.mouse.tapBehavior -int 1
```

Drag with three fingers:

```shell
defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad TrackpadThreeFingerDrag -bool true
defaults write com.apple.AppleMultitouchTrackpad TrackpadThreeFingerDrag -bool true
```

### Keyboard

Full keyboard control:

```shell
defaults write NSGlobalDomain AppleKeyboardUIMode -int 3
```

### Emacs keybinding

[cheat sheet](https://www.gnu.org/software/emacs/refcards/pdf/refcard.pdf)

MacOS supports most Emacs keybindings.

### Move cursor faster

Once you press and hold some key(e.g. "j" in vim), the cursor will move only one postion. How to improve this? The solution is as follows:

```shell
# active globally
defaults write -g ApplePressAndHoldEnabled -bool false
# inactive globally
defaults write -g ApplePressAndHoldEnabled -bool true
# only for vscode
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool true
```

## GUI Tools

### iTerm2

```shell
brew install --cask iterm2
```

### Alfred


## CLI tools

### Oh-My-Zsh

Set up zsh for MacOS

```shell
brew install zsh
chsh -s /bin/zsh
```

Set up oh my zsh

```shell
brew install wget curl git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/massher/tools/install.sh)"
```

### [zoxide](https://github.com/ajeetdsouza/zoxide)
```shell
brew install zoxide
```

### fzf

```shell
brew install fzf
```

### gsed
```shell
# install
brew install coreutils

echo "a b\nc d" | gsed 's/a/aa/g'
# aa b
# c d
```


### pbcopy
e.g.:
```shell
cat ls | rg hostname | pbcopy
```

### autojump

Install autojump

```shell
brew install autojump
```

add to `~/.zshrc`

```shell
[[ -s $(brew --prefix)/etc/profile.d/autojump.sh ]] && . $(brew --prefix)/etc/profile.d/autojump.sh
```

Tips:

* Go to direction: `j [file name]`

```shell
j post
# output:
# /Users/shang/Yukun4119.github.io/_posts
```

* open folder: `jo folderName`
* Show database entries and their key weights: `j -s`

For more info

```shell
man autojump
j -h
```

### Tmux

Install Tmux:

```shell
brew install tmux
```

The configuration file of Tmux is `~/.tmux.conf`

### Vim/Neovim

Now I use neovim. In some degree, neovim can be regarded superset of vim.

Install neovim

```shell
brew install neovim
```

The configuration file of neovim is in the directory `~/.config/nvim`.

## Others

### Check the status of port

1.  with `netstat`

    ```shell
    netstat -vanp tcp | grep 8080
    ```
2.  For **macOS El Capitan** and newer:

    ```shell
    lsof -i tcp:8080
    ```


###  map CapsLock to Control and Escape on Mac OS X

https://medium.com/@pechyonkin/how-to-map-capslock-to-control-and-escape-on-mac-60523a64022b

### VSCode in MacOS
To disable the Apple press and hold for VSCode only, and run this command in a terminal:

```shell
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false
```

Then restart VSCode.

To re-enable, run this command in a terminal:

```shell
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool true
```

### Hide dock permanently 
```shell
defaults write com.apple.dock autohide-delay -float 1000; killall Dock
```

To restore the default behavior:

```shell
defaults delete com.apple.dock autohide-delay; killall Dock
```

### MacOS System Commmands

https://git.herrbischoff.com/awesome-macos-command-line/about/


### Support fingerprint in authenticate sudo
```shell
sed "s/^#auth/auth/" /etc/pam.d/sudo_local.template | sudo tee /etc/pam.d/sudo_local
```
https://apple.stackexchange.com/questions/259093/can-touch-id-on-mac-authenticate-sudo-in-terminal

### Recommanded applications
- MediaMate
- Stats

### Browser Extensions
#### Tampermonkey
##### Enhanced word highlight