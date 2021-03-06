# Best Practices on using Kony Visualizer with Git

Tips and tricks to use Git with Kony Visualizer projects.

## What to Ignore

Figuring out what to ignore and what to version in a Visualizer project is the subject of many questions. The cross-platform nature of Visualizer makes for a very complex project anatomy. I've done my best to experiment with different versions of Visualizer and document my findings here. If you just want to get started quickly and all you need is a `.gitignore` file, then step into the root directory of your Visualizer project and run:

    curl -O https://raw.githubusercontent.com/mig82/vis-git/master/.gitignore

You'll download a self-documented `.gitignore` file.

## Removing Noise

Once you've commited your Visualizer project to your repository for the first time there are a number of files you can safely ignore thereafter. You can't ignore them from the start and not push them to your repo because Visualizer can't re-create them if they're missing. So checking out your project from the repo and trying to open it with Vis will fail. This means **these files must be a part of your repo**.

However, Visualizer has an annoying habit of modifying these files every time the project is opened, other files are changed or the project is built. This means that these will often appear as changes when you run `git status` and so you'll get a lot of "noise" when determining which changes to commit.

To ignore these files after they've been pushed to your repo for the first time use:

    git update-index --assume-unchanged <file>

For convenience you can just copy and execute these lines:

    git update-index --assume-unchanged run.sh
    git update-index --assume-unchanged run.bat
    git update-index --assume-unchanged projectProperties.json
    
## The Empty Directory Problem

When a Vis project is created a lot of empty directories are added to it's structure. Sadly, Git does not version directories, but files *in* directories. So empty directories are not versioned. This means if a brand new project is pushed to a git repo, those empty directories won't be pushed. The problem with this is that when the project is cloned by another developer, they will be unable to open the project. Vis will crash upon failing to find the directory structure it expects.

To get around this problem, you must force Git to push directories to source control. This can be done by adding a `.gitkeep` file to each empty directory. To do so, have a look at project Gitkeep [here](https://github.com/mig82/gitkeep)

## Other Candidates for Ignoring

The following is a list of files I've seen Visualizer update on it's own without good reason and so I've yet to validate whether these can be `.gitignore` or not.

These files where updated just by opening a project:

* modules/KonySyncLib.js
* modules/kony_sdk.js

These files were updated by selecting a different environment/cloud in the `Project Settings>Mobile Fabric>MobileFabric Environment` menu option in Visualizer:

* projectprop.xml
* syncclientcode.zip
