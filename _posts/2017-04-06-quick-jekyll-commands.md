---
layout: post
title: Quick Jekyll Commands
---

#### By default `jekyll serve` runs on port `4000`. To run it on a different port:
`jekyll serve --port 3100`

#### If you are running jekyll on a virtual machine and want to access it from your host machine:
`jekyll serve --host 0.0.0.0` (runs on port 4000) <br />
`jekyll serve --host 0.0.0.0 --port 3200` <br />

#### To preview site with drafts:

1. create folder `_drafts` at root <br />
1. create file within `_drafts` folder, for example `a-draft-post.md` <br />
1. run `jekyll serve --drafts` <br />
