[dotbot_repo]: https://github.com/anishathalye/dotbot
[aptget_repo]: https://github.com/dein0s/dotbot_plugin_aptget

## Dotbot ```sudo``` Plugin

Plugin for [Dotbot][dotbot_repo], that adds ```sudo``` directive, which allows you to run given direcives as root user. 

## Fork diff

This fork provides a `-E` option, which when running `sudo` will pass `-E` to the sudo command.

To enable it, do
```yaml
- defaults
    sudo:
      -E: true
```

WARNING: This is more a quickfix to beable to enjoy the current shell's env vars. but one must be wary, since this'll override some important variables as well. e.g. `$HOME` will not be `/root`

TODO: The next step would be to refactor the calling process, and instead of jamming every directive together and using one subprocess call, it could be that a shell session is created using POPEN and for each directive a subprocess is called upon it so as to preserve shell environment; that would nearly simulate the experience of directives without sudo. Though in that case to pass on env vars, one would have to use a script and a shell directive to reload their env vars... which isn't ideal but I've got no better ideas so far.

## Installation

1. Simply add this repo as a submodule of your dotfiles repository:
```
git submodule add https://github.com/DrDynamic/dotbot-sudo.git
```

2. Pass the path to the sudo.py file with corresponding flag to your [Dotbot][dotbot_repo] script:
  - ```-p /path/to/file/sudo.py```

## Supported task variants
```yaml
...
- sudo: 
    - other_directive: # like in the root of the config file
   ...
```

## Usage

### Example config
```yaml
...
- sudo:
    - clean: ['/root']
    - aptget: [package_name_one, package_name_two, package_name_three]
    ...
...
```

### Passing -E
```yaml
- defaults:
    sudo:
      -E: true
...
- sudo:
...
```

### Execution
```bash
"~/.dotfiles/bin/dotbot" -d "~/.dotfiles" -c "~/.dotfiles/packages.yaml" -p "~/.dotfiles/plugins/dotbot-sudo/sudo.py"
```
