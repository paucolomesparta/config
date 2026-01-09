# Oh My Zsh + Powerlevel10k + iTerm2 Setup Instructions

## Prerequisites

- macOS with Homebrew installed
- iTerm2 (or install via `brew install --cask iterm2`)

---

## 1. Install Oh My Zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

## 2. Install Powerlevel10k Theme

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

---

## 3. Install Custom Plugins

```bash
# zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# you-should-use
git clone https://github.com/MichaelAquilina/zsh-you-should-use.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/you-should-use
```

---

## 4. Install CLI Tools

```bash
brew install eza bat fd ripgrep fzf
```

---

## 5. Install NVM (Node Version Manager)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

---

## 6. Configure .zshrc

Add to `~/.zshrc`:

```zsh
# Enable Powerlevel10k instant prompt
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# Path to Oh My Zsh
export ZSH="$HOME/.oh-my-zsh"

# Theme
ZSH_THEME="powerlevel10k/powerlevel10k"

# Plugins
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
  you-should-use
  npm
  node
  brew
  macos
  fzf
)

source $ZSH/oh-my-zsh.sh

# Path additions
export PATH="$HOME/.local/bin:$PATH"
export PATH="/opt/homebrew/opt/openjdk@21/bin:$PATH"

# NVM setup
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"

# Auto-switch Node version based on .nvmrc
autoload -U add-zsh-hook
load-nvmrc() {
  local nvmrc_path="$(nvm_find_nvmrc)"
  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")
    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$(nvm version)" ]; then
      nvm use
    fi
  elif [ -n "$(PWD=$OLDPWD nvm_find_nvmrc)" ] && [ "$(nvm version)" != "$(nvm version default)" ]; then
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc

# Aliases - Navigation
alias ..="cd .."
alias ...="cd ../.."
alias ll="eza -la --icons --git"
alias la="eza -a --icons"
alias lt="eza --tree --level=2 --icons"

# Aliases - Git
alias gs="git status"
alias gp="git pull"
alias gpu="git push"

# Aliases - Development
alias dev="npm run dev"
alias build="npm run build"
alias test="npm test"

# Aliases - System
alias brewup="brew update && brew upgrade && brew cleanup"
alias zshconfig="code ~/.zshrc"
alias reload="exec zsh"

# Better defaults with modern tools
alias cat="bat"
alias find="fd"
alias grep="rg"

# Auto-navigate to workspace
[[ "$PWD" == "$HOME" ]] && cd ~/workspace

# Load p10k config
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh
```

---

## 7. Configure Powerlevel10k

Run the configuration wizard:

```bash
p10k configure
```

**Recommended settings:**
- Prompt Style: Classic
- Character Set: ASCII
- Appearance: Light
- Time: 24-hour
- Lines: 1 line
- Spacing: Compact
- Flow: Fluent
- Transient Prompt: Yes
- Instant Prompt: Verbose

Or copy your existing `~/.p10k.zsh` file to preserve current config.

---

## 8. Install Neovim (LazyVim)

### Install Neovim

```bash
brew install neovim
```

### Copy Configuration

```bash
# Backup existing config (if any)
mv ~/.config/nvim ~/.config/nvim.backup 2>/dev/null

# Copy your config
cp -r ~/dotfiles/nvim ~/.config/nvim
```

### First Launch

On first launch, lazy.nvim auto-bootstraps and installs all plugins:

```bash
nvim
```

Wait for plugins to install, then restart Neovim.

### Verify LSP Tools

Mason auto-installs these tools, but you can verify:

```vim
:Mason
```

**Pre-configured tools:**
- pyright (Python)
- tsserver (TypeScript/JavaScript)
- stylua (Lua formatter)
- shellcheck, shfmt (Shell)
- flake8 (Python linter)

### Installed Plugins (33 total)

| Category | Plugins |
|----------|---------|
| Framework | LazyVim, lazy.nvim |
| UI | bufferline, lualine, noice, which-key, snacks |
| Theme | tokyonight (primary), catppuccin |
| LSP | nvim-lspconfig, mason, mason-lspconfig |
| Completion | blink.cmp, friendly-snippets |
| Treesitter | nvim-treesitter, ts-autotag, ts-comments |
| Git | gitsigns |
| Navigation | flash, grug-far |
| Tools | trouble, todo-comments, conform, nvim-lint |

### Treesitter Parsers

Auto-installed: bash, html, javascript, json, lua, markdown, python, tsx, typescript, vim, yaml

---

## 9. iTerm2 Configuration

1. Open iTerm2 → Preferences (⌘,)
2. **Profiles → Text:**
   - Font: Monaco 12pt
   - Enable Bold/Italic fonts

3. **Optional:** Import your iTerm2 profile:
   - Profiles → Other Actions → Import JSON Profiles

---

## 10. Apply Changes

```bash
exec zsh
```

---

## Quick One-Liner Install Script

```bash
# Full installation (run each section separately)
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" && \
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k && \
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions && \
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && \
git clone https://github.com/MichaelAquilina/zsh-you-should-use.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/you-should-use && \
brew install eza bat fd ripgrep fzf neovim && \
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash && \
cp -r ~/dotfiles/nvim ~/.config/nvim
```

---

## Files to Back Up

For easy restoration, keep copies of:
- `~/.zshrc`
- `~/.p10k.zsh`
- `~/.config/nvim/` (entire directory)
- iTerm2 profile export (JSON)
