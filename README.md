## Firebase prompt

Show current firebase project on terminal prompt

When working on multiple stages (development, staging, production), developer usually use `firebase use [project_id_or_alias]` to switch between projects. It's very easy to run a command on production environment instead of development, which is very dangerous.

This script intends to show firebase project name on **shell prompt**. I hope this can help prevent unexpected situation.

## How to set up

### 1. Bash

Add following script to the end of `~/.bash_profile`

```bash
prompt_firebase() {
  local fb_project=$(grep \"$(pwd)\" ~/.config/configstore/firebase-tools.json | cut -d" " -f2)
  if [[ -n $fb_project ]]; then
    echo [$fb_project]
  fi
}
export PS1="\$(prompt_firebase)"$PS1
```

Then run `source ~/.bash_profile` or open new terminal window
![Demo](/assets/bash.png)

### 2. iTerm2 with oh-my-zsh
For example, if you're using `agnoster` theme:
Edit `~/.oh-my-zsh/themes/agnoster.zsh-theme`

```bash
prompt_firebase() {
  local fb_project=$(grep \"$(pwd)\" ~/.config/configstore/firebase-tools.json | cut -d" " -f2)
  if [[ -n $fb_project ]]; then
    prompt_segment red black $fb_project
  fi
}
```

and add `prompt_firebase` to `build_prompt` functions

```bash
build_prompt() {
  RETVAL=$?
  prompt_status
  prompt_virtualenv
  prompt_context
  prompt_dir
  prompt_git
  prompt_bzr
  prompt_hg
  prompt_firebase
  prompt_end
}
```

Then run `source ~/.zshrc` or open new terminal window

![Demo](/assets/oh_my_zsh.png)
