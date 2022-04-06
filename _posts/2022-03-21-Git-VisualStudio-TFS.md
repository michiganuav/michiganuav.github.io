---
title: How to make git, visual studio, and team foundation server play nicely
tags: git, visual studio
article_header:
  type: cover
  image: 
    src: /assets/images/2015/11/background_v2-16.png
license: false
---

# Problem and solution

I am working on a project that uses Visual Studio's (Azure DevOps)
Team Foundation Server (TFS) as it source control.  My colleagues are
accustomed to TFS and the windows environment. However, I much prefer
to manage my own contribution tracking with git.  Note also that the
project supports a linux build. I'm transitioning the linux build to
CMake, and hope that it will be adopted for cross platform builds. I
appreciate that Visual Studio is well integrated with CMake! Of
course, the linux build side cannot easily integrate with TFS as
source control.

To work with my colleagues, I must integrate with them over TFS.  The
issue is that Visual Studio sets git as the default source control if
it finds a .git directory. This functionally blocks the ability to
control the TFS repo. As a workaround,
[here](https://stackoverflow.com/questions/21150060/how-can-you-disable-git-integration-in-visual-studio-2013-permanently)
is what I did with the existing git repo.

## Initialize a new git repo

To initiate a new git repo on top of TFS, change to the root directory of the project and issue:

```console
git init --separate-git-dir _git
```

## Convert an existing git repo

To convert an existing repo to play nice with TFS

```console
mv .git _git
```

Next, when issuing any git commands, specify where to find the git dir:
```console
git --git-dir _git status
```

I'm now able to manage changes separately with TFS and git. (The
behave as if they are completely different repos.)  The git commits
and TFS changesets are not necessarily identical, though they can be
executed to be so.

# Line endings

Note, to overcome the CRLF line ending issue, I followed
[this](https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings)
recommendation.

For the given repo:
```console
git config core.autocrlf input
```
Note the property could also be set globally.

Assuming others may not have this set, create a .gitattributes file.  Note this should be tracked with git
```text
# Set the default behavior, in case people don't have core.autocrlf set.
* text=auto

# Explicitly declare text files you want to always be normalized and converted
# to native line endings on checkout.
*.c text
*.h text

# Declare files that will always have CRLF line endings on checkout.
*.sln text eol=crlf

# Denote all files that are truly binary and should not be modified.
*.png binary
*.jpg binary
```

```console
git add .gitattributes
git commit -m "Add a gitattributes file to manage CRLF compatibility."
```
