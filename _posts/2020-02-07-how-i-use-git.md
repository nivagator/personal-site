---
layout: post
title: How I Use Git
categories:
  - how i use
  - git
excerpt_separator: <!--more-->
---
Git is an open source version control system.

*A what-now?*
<!--more-->
### Git not Get

Git provides a fancy way to go back in time if you screw up your software... among other things. It makes sharing copies of software simple. It tracks changes in files over time. You can use it to automate tasks. It allows others to use what you've built to build new things. It's fairly ubiquitous in software development. 

Git is the underlying system of commands. Sites like [Github](https://github.com) and [Gitlab](https://gitlab.com/) use this system to allow users to publish code repositories to the internet making them accessible anywhere and to a broad audience. Repository hosting sites like these and others also allow community involvement in public software projects. Any user can get involved through issue tracking and resolution, feature requests, code contributions and more. 

Some of the most widely used programming languages ([Python](https://github.com/python/cpython), [Go](https://github.com/golang/go), [Swift](https://github.com/apple/swift)) software packages ([Kubernetes](https://github.com/kubernetes/kubernetes), [Node.js](https://github.com/nodejs/node),[Pandas](https://github.com/pandas-dev/pandas)) and software applications ([VSCode](https://github.com/Microsoft/vscode)) host their entire code base publicly on Github. This is what it means when people say 'open source software'. There are big name companies behind many of these repositories - Google, Microsoft and Apple.

Git is cool.

### Just the Basics

I'm not an expert. I'm not even a novice. Git is complex, powerful, confusing and harder to master than the actual programming itself. Everything I do with Git is from the command line and there are two primary ways I use Git (and Github):
  1. **Remote backups and access to code through Github**  
    This gives me a ton of options. I can pull my code to any machine and continue working on it from my last change, called a Commit. I can revert to a previous stage if I caused a meltdown. I can store config files and pull them to new machines. I can even take notes and keep logs of what I'm doing, where I had trouble, how I solved problems, etc. There's loads of flexibiliy and practical applications here.
    
  2. **Automated actions**  
  This path is relatively new to me. Git has something called hooks. [Git Hooks](https://githooks.com/) are script files triggered during Git actions (commit, push, receive). I'm publishing this blog remotely on a web server using a [post-update](https://github.com/nivagator/server-files/blob/master/scripts/post-update-jekyll) hook to build and deploy automatically. I'm working on publishing commit messages to twitter using a [post-commit](https://github.com/nivagator/server-files/blob/master/scripts/post-commit-tweet) hook. They can be used for just about anything.

This is just how I use it in my *very* limited scope. I don't do anything with branching or forks. Mostly just `status`»`add`»`commit`»`push`. I know there's something called `rebase` and `merge` that are super scary. `¯\_(ツ)_/¯` Someday, maybe, I'd like to contribute to open source projects or fork a public repo and build my own version. That would be neat.
