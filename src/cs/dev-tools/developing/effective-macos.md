# Effective MacOS: Tips, Tools, and Customizations

## Preparation

### Homebrew

Homebrew is the de facto package manager for macOS, making it easy to install and manage software.

**Install Homebrew:**

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**Install software using Homebrew:**

```shell
brew install visual-studio-code
```

## System Customizations

### Trackpad Customizations

- **Enable Tap to Click:**

```shell
defaults write com.apple.AppleMultitouchTrackpad Clicking -int 1
defaults -currentHost write NSGlobalDomain com.apple.mouse.tapBehavior -int 1
defaults write NSGlobalDomain com.apple.mouse.tapBehavior -int 1
```

- **Enable Drag with Three Fingers:**

```shell
defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad TrackpadThreeFingerDrag -bool true
defaults write com.apple.AppleMultitouchTrackpad TrackpadThreeFingerDrag -bool true
```

### Keyboard Customizations

- **Enable Full Keyboard Control:**

```shell
defaults write NSGlobalDomain AppleKeyboardUIMode -int 3
```

- **Activate Key Repetition:**

```shell
defaults write -g ApplePressAndHoldEnabled -bool false
```
Then log out and log back in to activate it

### Emacs Keybinding Support

MacOS supports most Emacs keybindings natively. Refer to the [Emacs cheat sheet](https://www.gnu.org/software/emacs/refcards/pdf/refcard.pdf) for more keybinding options.

### Improve Cursor Movement Speed

For faster cursor movement when holding down a key (e.g., "j" in Vim):

- **Globally Enable Faster Movement:**

```shell
defaults write -g ApplePressAndHoldEnabled -bool false
```

- **Globally Disable Faster Movement:**

```shell
defaults write -g ApplePressAndHoldEnabled -bool true
```

- **Enable for Specific Application (e.g., VSCode):**

```shell
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false
```

- **Disable for Specific Application:**

```shell
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool true
```

## GUI Tools

### iTerm2

Install iTerm2, an advanced terminal emulator for macOS:

```shell
brew install --cask iterm2
```

### Alfred

Alfred is a productivity app for macOS that enhances searching and task automation.

## Command-Line Tools

### Oh-My-Zsh

Set up Zsh as the default shell and install Oh My Zsh:

```shell
brew install zsh
chsh -s /bin/zsh
brew install wget curl git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### fzf

`fzf` is a general-purpose command-line fuzzy finder:

```shell
brew install fzf
```

### gsed (GNU `sed`)

Install GNU `sed` for better `sed` compatibility:

```shell
brew install coreutils
```

Example usage:

```shell
echo "a b\nc d" | gsed 's/a/aa/g'
# Output:
# aa b
# c d
```

### pbcopy

`pbcopy` copies the standard input to the clipboard:

```shell
cat ls | rg hostname | pbcopy
```

### autojump

`autojump` helps you navigate your filesystem quickly:

**Install autojump:**

```shell
brew install autojump
```

**Add to `~/.zshrc`:**

```shell
[[ -s $(brew --prefix)/etc/profile.d/autojump.sh ]] && . $(brew --prefix)/etc/profile.d/autojump.sh
```

**Common Commands:**

- Go to directory: `j [directory_name]`
- Open directory: `jo [directory_name]`
- Show database entries: `j -s`

### Tmux

Install Tmux, a terminal multiplexer:

```shell
brew install tmux
```

The Tmux configuration file is located at `~/.tmux.conf`.

### Vim/Neovim

Neovim is an extension of Vim with additional features and improvements.

**Install Neovim:**

```shell
brew install neovim
```

Neovim configuration is stored in `~/.config/nvim`.

## Miscellaneous Tips and Tricks

### Check the Status of a Port

1. **Using `netstat`:**

```shell
netstat -vanp tcp | grep 8080
```

2. **Using `lsof` for macOS El Capitan and newer:**

```shell
lsof -i tcp:8080
```

### Map Caps Lock to Control and Escape

Refer to [this guide](https://medium.com/@pechyonkin/how-to-map-capslock-to-control-and-escape-on-mac-60523a64022b) to map Caps Lock to function as both Control and Escape keys.

### Hide the Dock Permanently

```shell
defaults write com.apple.dock autohide-delay -float 1000; killall Dock
```

To restore the default behavior:

```shell
defaults delete com.apple.dock autohide-delay; killall Dock
```

### Support Fingerprint Authentication for `sudo`

Enable fingerprint authentication for `sudo`:

```shell
sed "s/^#auth/auth/" /etc/pam.d/sudo_local.template | sudo tee /etc/pam.d/sudo_local
```

Refer to this [Apple StackExchange answer](https://apple.stackexchange.com/questions/259093/can-touch-id-on-mac-authenticate-sudo-in-terminal) for more details.

## Recommended Applications

- **MediaMate**: A media management tool.
- **Stats**: A system monitor for macOS.

## Browser Extensions

### Tampermonkey

- **Enhanced Word Highlight**: Customize and enhance word highlighting in your browser.

## References

- [Awesome macOS Command Line](https://git.herrbischoff.com/awesome-macos-command-line/about/)

---