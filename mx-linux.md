# MX Linux (Debian based)

## Info

```
MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMNMMMMMMMMM   binakot@mx-linux
MMMMMMMMMMNs..yMMMMMMMMMMMMMm: +NMMMMMMM   ------------------------
MMMMMMMMMN+    :mMMMMMMMMMNo` -dMMMMMMMM   OS: MX x86_64
MMMMMMMMMMMs.   `oNMMMMMMh- `sNMMMMMMMMM   Kernel: 4.x-amd64
MMMMMMMMMMMMN/    -hMMMN+  :dMMMMMMMMMMM   Shell: zsh
MMMMMMMMMMMMMMh-    +ms. .sMMMMMMMMMMMMM   DE: Xfce
MMMMMMMMMMMMMMMN+`   `  +NMMMMMMMMMMMMMM   WM: Xfwm4
MMMMMMMMMMMMMMNMMd:    .dMMMMMMMMMMMMMMM   WM Theme: Matcha-dark-azul
MMMMMMMMMMMMm/-hMd-     `sNMMMMMMMMMMMMM   Theme: Matcha-dark-azul [GTK2], Adwaita [GTK3]
MMMMMMMMMMNo`   -` :h/    -dMMMMMMMMMMMM   Icons: Papirus-Dark [GTK2], Adwaita [GTK3]
MMMMMMMMMd:       /NMMh-   `+NMMMMMMMMMM   
MMMMMMMNo`         :mMMN+`   `-hMMMMMMMM   
MMMMMMh.            `oNMMd:    `/mMMMMMM   
MMMMm/                -hMd-      `sNMMMM   
MMNs`                   -          :dMMM   
Mm:                                 `oMM   
MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM
```

MX Linux is a cooperative venture between the antiX and former MEPIS communities,using the best tools and talents from each distro. 
It is a midweight OS designed to combine an elegant and efficient desktop with simple configuration, high stability, solid performance and medium-sized footprint.

[MX Linux - Homepage](https://mxlinux.org)

[MX Linux - Wikipedia](https://en.wikipedia.org/wiki/MX_Linux)

---

## Setup

---

### Update system

```bash
$ sudo apt-get update
$ sudo apt-get dist-upgrade
$ sudo apt-get autoremove
```

---

### Install tools

```bash
$ sudo apt-get -y install \
    nmon \
    ufw fail2ban openssh \    
    curl wget xsel xclip \
    build-essential cmake lintian cloc \
    ack ripgrep silversearcher-ag dos2unix \
    fonts-firacode fonts-powerline ttf-ubuntu-font-family \
    python-dev python3-dev python-pip python3-pip python-setuptools python3-setuptools python3-pygments
```

Required patched [Nerd Fonts](https://www.nerdfonts.com/).

System fonts: `Ubuntu 11`

Terminal fonts: `Meslo 10`

#### ✅ Ssh

Configure `fail2ban` to prevent brute-force attacks: `$ sudo vi /etc/fail2ban/jail.local`:

```bash
[ssh]
findtime = 3600
maxretry = 3
bantime = -1
```

After restart the service: `$ sudo service fail2ban restart`.

Add next lines to the end of `/etc/ssh/sshd_config`:

```bash
PermitRootLogin no
AllowUsers binakot
```

After restart the service: `$ sudo service ssh restart` and open ports: `$ sudo ufw allow ssh`.

#### ✅ Git

Install:

```bash
$ sudo apt-get -y install git
```

Setup SSH keys:

```bash
$ ssh-keygen -t rsa -b 4096                             # fileName: id_rsa
$ ssh-keygen -t rsa -b 4096 -C "binakot@github.com"     # fileName: binakot.github.id_rsa
$ ssh-keygen -t rsa -b 4096 -C "binakot@bitbucket.org"  # fileName: binakot.bitbucket.id_rsa
$ ssh-add -k id_rsa binakot.github.id_rsa binakot.bitbucket.id_rsa
$ ssh-add -l
```

Create and configure `~/.ssh/config`:

```bash
Host github.com
    User binakot
    IdentityFile ~/.ssh/binakot.github.id_rsa.pub

Host bitbucket.org
    User binakot
    IdentityFile ~/.ssh/binakot.bitbucket.id_rsa.pub
```

Configure git:

```bash
$ git config --global -e
```

```text
# https://git-scm.com/docs/git-config

[user]
    name = Ivan Muratov
    email = muratov.i@firstmk.ru

[github]
    user = binakot

[core]
    editor = nvim
    excludesfile = ~/.gitignore_global
    fileMode = false
    quotepath = false

[credential]
    helper = cache

[fetch]
    prune = true
    pruneTags = true

[push]
    default = simple
    followTags = true

[rebase]
    autoStash = true

[merge]
    log = true

[color]
    ui = true

[grep]
    lineNumber = true

[alias]
    configs = config --list
    aliases = config --get-regexp alias
    tree = log --graph --decorate --pretty=oneline --abbrev-commit --all
    fp = !git fetch --all && git pull --all
```

Initial `.gitignore_global` you can take from [here](https://gist.github.com/octocat/9257657)

Also create common `editorconfig` settings in file `~/.editorconfig`:

```text
root = true

[*]
charset = utf-8
end_of_line = lf
indent_size = 4
indent_style = space
insert_final_newline = true
trim_trailing_whitespace = true

[*.md]
trim_trailing_whitespace = false

[{*.js, *.json}]
indent_size = 2

[{*.yml, *.yaml}]
indent_size = 2

[*.java]
continuation_indent_size = 4
```

#### ✅ Zsh + Oh My Zsh + FZF

Install:

```bash
$ sudo apt-get install zsh
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
$ git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf && ~/.fzf/install
```

Theme:

Download some of `Nerd Fonts` from site: https://www.nerdfonts.com. Install any way you prefer. 
For example just copy `ttf` files of patched [Meslo Nerd Font](https://github.com/romkatv/powerlevel10k#recommended-meslo-nerd-font-patched-for-powerlevel10k) in system fonts folder `/usr/share/fonts/truetype/`.

Configure fonts and colors for `xcfe-terminal`. For example with [Solarized](https://ethanschoonover.com/solarized/):
https://github.com/sgerrand/xfce4-terminal-colors-solarized.

```bash
$ git clone --depth 1 https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```

Set `ZSH_THEME="powerlevel10k/powerlevel10k"` in your `~/.zshrc`.

Plugins:

* [Built-in](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins)

* [zsh-completions](https://github.com/zsh-users/zsh-completions):

```bash
git clone --depth 1 https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
# plugins=(zsh-completions)
```

* [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions): 

```bash
$ git clone --depth 1 https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# plugins=(zsh-autosuggestions)
```

* [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting):

```bash
$ git clone --depth 1 https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
# plugins=(zsh-syntax-highlighting) # Note that zsh-syntax-highlighting must be the last plugin sourced.
```

Configure the file `~/.zshrc`:

```
# Enable Powerlevel10k instant prompt. Should stay close to the top of ~/.zshrc.
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# Path to your oh-my-zsh installation.
export ZSH="/home/muratov/.oh-my-zsh"

# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
ZSH_THEME="powerlevel10k/powerlevel10k"
typeset -g POWERLEVEL9K_INSTANT_PROMPT=quiet

# Which plugins would you like to load?
plugins=(
    ssh-agent tmux vi-mode
    fzf z
    colorize colored-man-pages
    bgnotify
    git docker
    zsh-completions zsh-autosuggestions zsh-syntax-highlighting
)

source $ZSH/oh-my-zsh.sh

# Custom actions
xgamma -rgamma 1 -ggamma 1 -bgamma 1 > /dev/null 2>&1
ssh-add -k ~/.ssh/id_rsa ~/.ssh/binakot.github.id_rsa ~/.ssh/muratovii.bitbucket.id_rsa > /dev/null 2>&1

# Aliases
alias v="nvim"
alias tmux="tmux -2"
alias pcat="pygmentize -f terminal256 -O style=native -g"
alias pls="colorls -A --sd --gs"
alias plsa="colorls -Al --sd --gs"
alias ptree="colorls --tree --sd --gs"
alias clear="timeout 3 cmatrix; clear"

# Tmux integration
export ZSH_TMUX_AUTOSTART=true
export ZSH_TMUX_AUTOSTART_ONCE=true
export ZSH_TMUX_AUTOCONNECT=true
export ZSH_TMUX_AUTOQUIT=false

# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh

# Enable FZF
[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh

# NVM
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

# THIS MUST BE AT THE END OF THE FILE FOR SDKMAN TO WORK!!!
export SDKMAN_DIR="/home/muratov/.sdkman"
[[ -s "/home/muratov/.sdkman/bin/sdkman-init.sh" ]] && source "/home/muratov/.sdkman/bin/sdkman-init.sh"
```

#### ✅ Tmux

Install [Tmux](https://github.com/tmux/tmux):

```bash
$ sudo apt-get install tmux
```

Install [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm):

```bash
$ git clone --depth 1 https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

Configure the file `~/.tmux.conf`:

```bash
# Remap prefix from 'Ctrl-b' to 'Ctrl-a'
unbind C-b
set -g prefix C-a
bind C-a send-prefix
bind-key C-a last-window

# Enable mouse support and focus handling
set -g mouse on
set -g focus-events on

# Enable vi mode
set -g status-keys vi
setw -g mode-keys vi
bind-key -T copy-mode-vi v send-keys -X begin-selection
bind-key -T copy-mode-vi y send-keys -X copy-selection
bind-key -T copy-mode-vi r send-keys -X rectangle-toggle
bind -T copy-mode-vi y send-keys -X copy-pipe-and-cancel 'xclip -in -selection clipboard'
set -sg escape-time 10

# Start windows numbering at 1
set -g base-index 1
setw -g pane-base-index 1

# Config windows behaviour
set -g renumber-windows on
setw -g automatic-rename on
setw -g aggressive-resize on

# Activities
set -g monitor-activity on
set -g visual-activity off

# Split panes using | and -
bind | split-window -h
bind - split-window -v
unbind '"'
unbind %

# Switch panes using Alt-arrow without prefix
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-yank'

set -g @plugin 'jimeh/tmux-themepack'
set -g @themepack 'powerline/default/cyan'

set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'

set -g default-terminal "xterm-256color"

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run -b '~/.tmux/plugins/tpm/tpm'
```

Reload config `$ tmux source ~/.tmux.conf`
and install the plugins in `tmux` with `Ctrl+A -> I`.
Later for plugins updates use `Ctrl+A -> U`.

#### ✅ (Neo)Vim

Install:

```bash
$ sudo apt-get install neovim
$ pip install pynvim
$ pip3 install pynvim
```

Install plugin manager [vim-plug](https://github.com/junegunn/vim-plug):

```bash
$ curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
$ touch ~/.config/nvim/init.vim
```

Content of `init.vim` file: 

```vim
scriptencoding utf-8

call plug#begin()

" Common
Plug 'tpope/vim-sensible'
Plug 'sjl/vitality.vim'
Plug 'mhinz/vim-startify'

" UI
Plug 'morhetz/gruvbox'
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
Plug 'ryanoasis/vim-devicons'
Plug 'nathanaelkane/vim-indent-guides'
Plug 'yuttie/comfortable-motion.vim'
Plug 'easymotion/vim-easymotion'

" Nerdtree
Plug 'scrooloose/nerdtree'
Plug 'scrooloose/nerdcommenter'
Plug 'xuyuanp/nerdtree-git-plugin'
Plug 'tiagofumo/vim-nerdtree-syntax-highlight'

" Edit
Plug 'tpope/vim-surround'
Plug 'jiangmiao/auto-pairs'
Plug 'ntpeters/vim-better-whitespace'
Plug 'terryma/vim-multiple-cursors'
Plug 'editorconfig/editorconfig-vim'

" Search
Plug 'Shougo/denite.nvim', { 'do': ':UpdateRemotePlugins'  }
Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --all' }
Plug 'junegunn/fzf.vim'

" Git
Plug 'tpope/vim-fugitive'
Plug 'airblade/vim-gitgutter'
Plug 'junegunn/gv.vim'

" Code
Plug 'janko-m/vim-test'
Plug 'neoclide/coc.nvim', {'branch': 'release'}
let g:coc_global_extensions = [
      \ 'coc-tabnine',
      \ 'coc-actions',
      \ 'coc-lists',
      \ 'coc-snippets',
      \ 'coc-highlight',
      \ 'coc-yank',
      \
      \ 'coc-yaml',
      \ 'coc-json',
      \ 'coc-xml',
      \
      \ 'coc-tsserver',
      \ 'coc-html',
      \ 'coc-css',
      \ 'coc-prettier',
      \
      \ 'coc-java',
      \ 'coc-python',
      \ 'coc-sql',
      \
      \ 'coc-git',
      \ 'coc-spell-checker',
      \ 'coc-docker',
      \
      \ 'coc-diagnostic',
      \]

call plug#end()

set encoding=utf-8
set termencoding=utf-8
set nocompatible
set ttyfast
set lazyredraw
set mouse-=a
set updatetime=250

set showcmd
set cmdheight=1
set laststatus=2
set scrolloff=999
set wildmenu
set wildmode=longest:full,full
set splitbelow
set splitright

set hidden
set autoread
set autowrite
set autowriteall
set clipboard+=unnamedplus

set noswapfile
set nobackup
set nowritebackup

syntax enable
set ruler
set cursorline
set number
set relativenumber
set signcolumn=yes
set shortmess+=c

set tabstop=4
set softtabstop=4
set shiftwidth=4
set smarttab
set expandtab
set autoindent
set cindent
set shiftround

set hlsearch
set incsearch
set ignorecase
set smartcase
set showmatch
set matchpairs+=<:>
set backspace=indent,eol,start
set list

set foldmethod=syntax
set nofoldenable
set wrap
set linebreak
set showbreak=↪

" Leader key
let g:mapleader=','
" Enable hotkeys for Russian layout
set langmap=ФИСВУАПРШОЛДЬТЩЗЙКЫЕГМЦЧНЯ;ABCDEFGHIJKLMNOPQRSTUVWXYZ,фисвуапршолдьтщзйкыегмцчня;abcdefghijklmnopqrstuvwxyz
" Allows you to save files you opened without write permissions via sudo
cmap w!! w !sudo tee %
" Disable highlights when you press <leader><cr>
map <silent><leader><cr> :noh<cr>
" Smart way to move between windows
map <C-j> <C-W>j
map <C-k> <C-W>k
map <C-h> <C-W>h
map <C-l> <C-W>l
" Tab switches
map <A-h> :bp<cr>
map <A-l> :bn<cr>
" Disable arrows
noremap <Up> <Nop>
noremap <Down> <Nop>
noremap <Left> <Nop>
noremap <Right> <Nop>


" vitality.vim
autocmd FocusLost,BufLeave * :wa

" gruvbox
colorscheme gruvbox

" vim-airline
let g:airline#extensions#tabline#enabled=1
let g:airline_theme='gruvbox'

" vim-intent-guides
let g:indent_guides_enable_on_vim_startup=1

" nerdtree
let NERDTreeShowHidden=1
let NERDTreeQuitOnOpen=1
nmap <leader>n :NERDTreeToggle<CR>
nmap <leader>N :NERDTreeFind<CR>
nmap ++ <plug>NERDCommenterToggle
vmap ++ <plug>NERDCommenterToggle

" vim-better-whitespace
nmap <leader>y :StripWhitespace<CR>

" denite.nvim
call denite#custom#var('file/rec', 'command', ['rg', '--files', '--glob', '!.git'])
call denite#custom#var('grep', 'command', ['rg'])
call denite#custom#var('grep', 'separator', ['--'])
call denite#custom#var('grep', 'default_opts', ['--hidden', '--vimgrep', '--heading', '-S'])
call denite#custom#var('grep', 'recursive_opts', [])
call denite#custom#var('grep', 'final_opts', [])
call denite#custom#var('grep', 'pattern_opt', ['--regexp'])
call denite#custom#var('buffer', 'date_format', '')

let s:denite_options = {'default' : {
\ 'split': 'floating',
\ 'start_filter': 1,
\ 'auto_resize': 1,
\ 'source_names': 'short',
\ 'prompt': 'λ ',
\ 'statusline': 0,
\ 'highlight_matched_char': 'QuickFixLine',
\ 'highlight_matched_range': 'Visual',
\ 'highlight_window_background': 'Visual',
\ 'highlight_filter_background': 'DiffAdd',
\ 'winrow': 1,
\ 'vertical_preview': 1
\ }}

nmap ; :Denite buffer<CR>
nmap <leader>t :DeniteProjectDir file/rec<CR>
nnoremap <leader>g :<C-u>Denite grep:. -no-empty<CR>
nnoremap <leader>j :<C-u>DeniteCursorWord grep:.<CR>

autocmd FileType denite-filter call s:denite_filter_my_settings()
function! s:denite_filter_my_settings() abort
  imap <silent><buffer> <C-o>
  \ <Plug>(denite_filter_quit)
  inoremap <silent><buffer><expr> <Esc>
  \ denite#do_map('quit')
  nnoremap <silent><buffer><expr> <Esc>
  \ denite#do_map('quit')
  inoremap <silent><buffer><expr> <CR>
  \ denite#do_map('do_action')
  inoremap <silent><buffer><expr> <C-t>
  \ denite#do_map('do_action', 'tabopen')
  inoremap <silent><buffer><expr> <C-v>
  \ denite#do_map('do_action', 'vsplit')
  inoremap <silent><buffer><expr> <C-h>
  \ denite#do_map('do_action', 'split')
endfunction

autocmd FileType denite call s:denite_my_settings()
function! s:denite_my_settings() abort
  nnoremap <silent><buffer><expr> <CR>
  \ denite#do_map('do_action')
  nnoremap <silent><buffer><expr> q
  \ denite#do_map('quit')
  nnoremap <silent><buffer><expr> <Esc>
  \ denite#do_map('quit')
  nnoremap <silent><buffer><expr> d
  \ denite#do_map('do_action', 'delete')
  nnoremap <silent><buffer><expr> p
  \ denite#do_map('do_action', 'preview')
  nnoremap <silent><buffer><expr> i
  \ denite#do_map('open_filter_buffer')
  nnoremap <silent><buffer><expr> <C-o>
  \ denite#do_map('open_filter_buffer')
  nnoremap <silent><buffer><expr> <C-t>
  \ denite#do_map('do_action', 'tabopen')
  nnoremap <silent><buffer><expr> <C-v>
  \ denite#do_map('do_action', 'vsplit')
  nnoremap <silent><buffer><expr> <C-h>
  \ denite#do_map('do_action', 'split')
endfunction

" fzf
map <leader>f :Files<CR>

" vim-test
let test#strategy="neovim"

" coc.nvim
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gt <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)
nmap <silent> gh <Plug>(coc-doHover)
nmap <leader> rn <Plug>(coc-rename)

nmap [g <Plug>(coc-git-prevchunk)
nmap ]g <Plug>(coc-git-nextchunk)
nmap gs <Plug>(coc-git-chunkinfo)
nmap gu :CocCommand git.chunkUndo<cr>

inoremap <silent><expr> <c-space> coc#refresh()
nnoremap <silent> K :call <SID>show_documentation()<CR>

function! s:show_documentation()
    if (index(['vim','help'], &filetype) >= 0)
        execute 'h '.expand('<cword>')
    else
        call CocAction('doHover')
  endif
endfunction

inoremap <silent><expr> <TAB>
    \ pumvisible() ? "\<C-n>" :
    \ <SID>check_back_space() ? "\<TAB>" :
    \ coc#refresh()
inoremap <expr><S-TAB> pumvisible() ? "\<C-p>" : "\<C-h>"

function! s:check_back_space() abort
    let col = col('.') - 1
    return !col || getline('.')[col - 1]  =~# '\s'
endfunction

autocmd CursorHold * silent call CocActionAsync('highlight')
```

Now update config and install all plugins in `nvim`:

```vim
:source %
:PlugInstall
```

Replace `Caps Lock` with `Control`. Create and edit file `~/.xmodmap` with content:

```bash
remove Lock = Caps_Lock
keysym Caps_Lock = Control_L
add Control = Control_L
```

After apply settings `$ xmodmap ~/.xmodmap` and put config to `~/.xinitrc`:

```bash
xmodmap ~/.xmodmap
```

#### ✅ Others

Install [Color LS](https://github.com/athityakumar/colorls):

```bash
$ sudo apt-get install ruby-full
$ sudo gem install colorls
$ source $(dirname $(gem which colorls))/tab_complete.sh
```

Install [Ranger](https://ranger.github.io):

```bash
$ sudo apt-get install ranger w3m w3m-img
$ ranger --copy-config=all
```

And change few settings in `~/.config/ranger/rc.conf`:

```bash
set preview_images true
set preview_images_method w3m
set w3m_delay 0.1
set colorscheme solarized
set draw_borders true
```

---

### Install software

* [SDKMAN!](https://dzone.com/articles/sdkman-managing-sdks-were-never-so-smart) for Java, Kotlin, etc:

```bash
$ curl -s "https://get.sdkman.io" | bash
$ sdk list java             # list of available builds
$ sdk install java          # install the latest LTS build
$ sdk current java          # show the Java version for the current terminal
$ sdk use java $VERSION     # switch the Java version for the current terminal
$ sdk default java $VERSION # set a specific version as default for all terminals
```

* [NVM](https://github.com/nvm-sh/nvm) for NodeJS:

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
$ nvm install node
$ npm install -g neovim
```

* [Docker](https://docs.docker.com/install/linux/docker-ce/debian/)

* [JetBrains Toolbox](https://www.jetbrains.com/toolbox-app/) for Intellij IDEA and so on

* [Google Chrome](https://www.google.com/intl/ru/chrome/)

* [Slack](https://slack.com/intl/en-ru/downloads/instructions/ubuntu)

* [Telegram](https://desktop.telegram.org/):

```bash
$ wget -O- https://telegram.org/dl/desktop/linux | sudo tar xJ -C /opt/
$ sudo ln -s /opt/Telegram/Telegram /usr/local/bin/telegram
```
