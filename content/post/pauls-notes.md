---
title: "Notes to future me"
date: 2018-12-20T12:27:11-08:00
publishdate: 2018-12-20
lastmod: 2018-12-20
draft: false
tags: ['meta','hugo','git']
---

### I haven't updated this site in years, remind me:

**Make a new post:**

- `hugo new post/pauls-notes.md`

- Update the posts metadata such as whether a draft and add tags

**Run local server and include draft posts:**

- `hugo server -D`

- then kill local server with `Ctrl-C`

**Build site to github pages submodule:**

- remember you previously setup the submodule using [these instructions](https://gohugo.io/hosting-and-deployment/hosting-on-github/#step-by-step-instructions).

- `hugo`

**Deploy Site:**

- double click deploy.sh bash script or simply type `deploy.sh`

- Done!

Since the site code lives in one repo and the public folder deploys to another repo, make sure to push changes to the code repo as well.

[More info on this minimal-bootstrap-hugo-theme](https://themes.gohugo.io/minimal-bootstrap-hugo-theme/)