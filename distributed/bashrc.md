# A Starer .bashrc

```bash
# ~/.bashrc

# Set a colorful and informative prompt
PS1='\[\e[1;32m\]\u@\h:\[\e[0;34m\]\w\[\e[0m\]\$ '

# Alias common commands for convenience
alias ll='ls -lah --color=auto'
alias grep='grep --color=auto'
alias cls='clear'

# Add project-specific paths to PATH
export PATH="/app/bin:$PATH"

# Enable interactive completion
if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
fi

# Set EDITOR to your preferred editor (vim in this case)
export EDITOR=vim

# Add helpful Git aliases
alias gs='git status'
alias gl='git log --oneline --graph --decorate'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gd='git diff'

# Add Elixir-specific environment variables (if applicable)
export MIX_ENV=dev

# Improve history behavior
export HISTCONTROL=ignoredups:erasedups
export HISTSIZE=10000
export HISTFILESIZE=20000
shopt -s histappend

# Display the current directory in the terminal title
case "$TERM" in
    xterm*|rxvt*)
        PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
        ;;
esac

# Source project-specific settings if available
if [ -f /app/.env ]; then
    export $(grep -v '^#' /app/.env | xargs)
fi

# Optional: Include any project-specific helpers or scripts
if [ -d /app/scripts ]; then
    export PATH="/app/scripts:$PATH"
fi
```

