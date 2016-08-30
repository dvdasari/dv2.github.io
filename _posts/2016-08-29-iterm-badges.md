---
layout:      post
title:       iTerm Badge
---

iTerm *badge* is label that appears on the top right of the terminal session. We can display some dynamic status like the current user, host name or git branch. Here is the link for [iTerm badges documentation](https://www.iterm2.com/documentation-badges.html)

Example of a session to show current user and host name.

Open `iTerm > Preferences > Profiles > General > Badge`

![Screenshot](/assets/badge_preferences.png)

{:.center}
![Example](/assets/badge_iterm_simple.png)

### iTerm2 Defined Variables

* session.name
* session.columns
* session.rows
* session.hostname  *(Only set if [Shell Integration](https://www.iterm2.com/shell_integration.html) is installed.)*
* session.username  *(Only set if [Shell Integration](https://www.iterm2.com/shell_integration.html) is installed.)*
* session.path

  To Enable Shell Integration, select `iTerm2 > Install Shell Integration menu item`, which will execute <br>
  `curl -L https://iterm2.com/misc/install_shell_integration.sh | bash`

  Then add this to your login script (.login for tcsh, .bash_profile for bash, .zshrc for zsh, or config.fish file for fish): <br>
  ```
  source ~/.iterm2_shell_integration.`basename $SHELL`
  ```

### User Defined Variables

To create user-defined variables we define a function named `iterm2_print_user_vars` and call `iterm2_set_user_var` to set each variable in your shell's rc file (.bashrc, .zshrc, config.fish, .login)

```
# bash: Place this in .bashrc or .bash_profile
function iterm2_print_user_vars() {
  iterm2_set_user_var gitBranch $((git branch 2> /dev/null) | grep \* | cut -c3-)
  iterm2_set_user_var gitRepo $((git remote show origin 2> /dev/null) | grep "Fetch URL:" | cut -c14-)
}
```

User-defined variables are referred as `\(user.gitBranch)`

For example to user few user-defined variables, paste

```
\(session.username)@\(session.hostname)\nPath: \(session.path)\nGit Branch: \(user.gitBranch)\nGit Repo: \(user.gitRepo)
```

in `iTerm > Preferences > Profiles > General > Badge`

![Example](/assets/badge_iterm_user.png)

Badge colors can be changed in `iTerm > Preferences > Profiles > Colors`
