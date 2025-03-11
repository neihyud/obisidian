- `du -h --max-depth=3 Dev | sort -hr`  => check size folder /Dev when size folder increase sudden
- `sudo du -sh /* 2>/dev/null`: show size folders
- `sudo find /var/lib/docker/volumes/db_events_mssql/_data/log/ -name "core.sqlservr.*.gdmp" -delete -print` : remove file 
- `sudo find /var -type f -size +1G -exec du -h {} + | sort -rh`: find file have size > 1G in folder /var

### Kill port
- lsof -i :<name_port>
- kill -9 name_id (từ lsof)
### Vscode config
```json
{
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs": "active",
  "workbench.iconTheme": "material-icon-theme",
  "redhat.telemetry.enabled": true,
  "vs-kubernetes": {
    "vscode-kubernetes.kubectl-path.linux": "/home/neihyud/.local/state/vs-kubernetes/tools/kubectl/kubectl",
    "vscode-kubernetes.minikube-path.linux": "/home/neihyud/.local/state/vs-kubernetes/tools/minikube/linux-amd64/minikube",
    "vscode-kubernetes.helm-path.linux": "/home/neihyud/.local/state/vs-kubernetes/tools/helm/linux-amd64/helm"
  },
  "editor.tabSize": 2,
  "terminal.integrated.enableMultiLinePasteWarning": "never",
  "gitlens.graph.layout": "editor",
  "eslint.debug": true,
  "workbench.editor.enablePreview": false,
  "explorer.confirmDelete": false,
  "files.autoSave": "afterDelay",
  "liveServer.settings.donotShowInfoMsg": true,
  "editor.formatOnSave": true,
  "explorer.confirmDragAndDrop": false,
  "javascript.updateImportsOnFileMove.enabled": "always",
  "eslint.format.enable": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "always",
    "source.addMissingImports": "always",
    "source.removeUnusedImports": "always"
  },
  "typescript.updateImportsOnFileMove.enabled": "always",
  "[javascriptreact]": {
    "editor.defaultFormatter": "dbaeumer.vscode-eslint"
  },
  "[javascript]": {
    "editor.defaultFormatter": "dbaeumer.vscode-eslint"
  },
  "eslint.run": "onSave",
  "[typescriptreact]": {
    "editor.defaultFormatter": "dbaeumer.vscode-eslint"
  },
  "scm.showChangesSummary": false,
  "editor.fontFamily": "JetBrains Mono",
  "editor.fontLigatures": false,
  "workbench.colorTheme": "One Dark Pro Flat",
  "prettier.configPath": ".prettierrc",
  "terminal.integrated.defaultProfile.linux": "zsh",
  "typescript.preferences.importModuleSpecifier": "non-relative",
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

### Ts config
```javascript

"baseUrl": ".",
"paths": {
	"@/*": ["./*"]
}

=> custome import path: setting in vscode => **importModuleSpecifier** (no-relative)
```

### Zsh config
```shell
# If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH

# Path to your oh-my-zsh installation.
export ZSH="$HOME/.oh-my-zsh"

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
ZSH_THEME="robbyrussell"

# Set list of themes to pick from when loading at random
# Setting this variable when ZSH_THEME=random will cause zsh to load
# a theme from this variable instead of looking in $ZSH/themes/
# If set to an empty array, this variable will have no effect.
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion.
# Case-sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

# Uncomment one of the following lines to change the auto-update behavior
# zstyle ':omz:update' mode disabled  # disable automatic updates
# zstyle ':omz:update' mode auto      # update automatically without asking
# zstyle ':omz:update' mode reminder  # just remind me to update when it's time

# Uncomment the following line to change how often to auto-update (in days).
# zstyle ':omz:update' frequency 13

# Uncomment the following line if pasting URLs and other text is messed up.
# DISABLE_MAGIC_FUNCTIONS="true"

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# You can also set it to another string to have that shown instead of the default red dots.
# e.g. COMPLETION_WAITING_DOTS="%F{yellow}waiting...%f"
# Caution: this setting can cause issues with multiline prompts in zsh < 5.7.1 (see #5765)
# COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# You can set one of the optional three formats:
# "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# or set a custom format using the strftime function format specifications,
# see 'man strftime' for details.
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git zsh-autosuggestions)

source $ZSH/oh-my-zsh.sh

# User configuration

# export MANPATH="/usr/local/man:$MANPATH"

# You may need to manually set your language environment
# export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

export PATH=$PATH:/usr/local/go/bin
export PATH="/usr/bin/python3/bin:$PATH"
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$PATH

alias varmeta="$HOME/Dev/varmeta"
alias gvm='eval "$(ssh-agent -s)"; ssh-add ~/.ssh/var-meta'
alias neetcode="$HOME/Workspace/neetcode"
alias note="$HOME/Notes"
# cd ~/Workspace
eval $(minikube docker-env)
```

### Eslint - config

```shell
module.exports = {
  root: true,
  env: { browser: true, es2020: true },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'plugin:react-hooks/recommended',
  ],
  ignorePatterns: ['dist', '.eslintrc.cjs'],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    parser: '@babel/eslint-parser',
    babelOptions: {
      parserOpts: {
        plugins: ["jsx"],
      },
    }
  },
  settings: { 
    react: { 
      version: '18.2' 
    }
  },
  plugins: [
    'react-refresh',
    'react',
  ],
  rules: {
    "array-bracket-spacing": ["error", "never"],
    "brace-style": ["error", "1tbs"],
    "comma-spacing": "error",
    "comma-style": "error",
    "computed-property-spacing":  ["error", "never"],
    "constructor-super": "error",
    "dot-notation": "error",
    "eol-last": "error",
    "func-call-spacing": "error",
    "indent": [ "error", "tab", { "SwitchCase": 1 } ],
    "key-spacing": "error",
    "keyword-spacing": "error",
    "no-alert": "error",
    "no-bitwise": "error",
    "no-console": "error",
    "no-const-assign": "error",
    "no-debugger": "error",
    "no-dupe-args": "error",
    "no-dupe-class-members": "error",
    "no-dupe-keys": "error",
    "no-duplicate-case": "error",
    "no-duplicate-imports": "error",
    "no-else-return": "error",
    "no-eval": "error",
    "no-extra-semi": "error",
    "no-fallthrough": "error",
    "no-lonely-if": "error",
    "no-mixed-operators": "error",
    "no-mixed-spaces-and-tabs": "error",
    "no-multiple-empty-lines": [ "error", { "max": 1 } ],
    "no-multi-spaces": "error",
    "no-negated-in-lhs": "error",
    "no-nested-ternary": "error",
    "no-redeclare": "error",
    "no-shadow": "error",
    "no-undef-init": "error",
    "no-unreachable": "error",
    "no-unsafe-negation": "error",
    "no-use-before-define": [ "error", "nofunc" ],
    "no-useless-computed-key": "error",
    "no-useless-constructor": "error",
    "no-useless-return": "error",
    "no-var": "error",
    "no-whitespace-before-property": "error",
    "object-curly-spacing": [ "error", "always" ],
    "one-var": "off",
    "prefer-const": "error",
    "semi": ["error","never"],
    "semi-spacing": "error",
    "space-before-blocks": [ "error", "always" ],
    "space-before-function-paren": [ "error", "never" ],
    "space-in-parens": [ "error", "never" ],
    "space-infix-ops": [ "error", { "int32Hint": false } ],
    "space-unary-ops": [ "error"],
    "template-curly-spacing": [ "error", "never" ],
    "valid-jsdoc": [ "error", { "requireReturn": false } ],
    "valid-typeof": "error",
    "yoda": [0],
    "linebreak-style": [
        2,
        "unix"
    ],
    "quotes": [
        2,
        "single"
    ],
    "curly": [
        2,
        "all"
    ],
    "eqeqeq": [
        2,
        "smart"
    ],
    "one-var-declaration-per-line": [
        2,
        "always"
    ],
    "new-cap": 2,
    "no-case-declarations": 0,
  }
}
```
