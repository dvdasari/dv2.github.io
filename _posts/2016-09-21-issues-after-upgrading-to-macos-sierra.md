---
layout:      post
title:       Resolving issues after upgrading to macOS Sierra
---

## Issue with tmux

Error:

```
[warn] kq_init: detected broken kqueue; not using.: File exists
```

Fix:

```
brew uninstall --force tmux
brew install --HEAD tmux
```

### Avoid having to enter passphrase everytime a tmux session is started

Message:

```
Enter passphrase for key '/Users/username/.ssh/id_rsa':
```

Fix:

Start a new tmux session:

```
$ tmux new -s temp
$ ssh-agent
$ ssh-add
```

Enter the passphrase when it prompts for the passphrase, and it will not ask for it again.
