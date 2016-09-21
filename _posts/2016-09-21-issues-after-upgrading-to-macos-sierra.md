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
