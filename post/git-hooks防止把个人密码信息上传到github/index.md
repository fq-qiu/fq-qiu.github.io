
## Introduction
In most projects, hooks are in `.git/hooks`. The hooks are some scripts written in Ruby or Python or Shell or whatever language you are familiar with. If you want to use the bundled hook scripts, you'll have to rename them without file extentions `.sample`

There are two kind of hooks, client-side hooks and server-side hooks.

Here, I just focus on `pre-commit` on client-side.

<!--more-->

## prevent leakage of private password to github
rename `.git/hooks/pre-commit.sample` to `.git/hooks/pre-commit` and it will be toggled when you run `git commit`.

Add following to `pre-commit`, it will just take effect when you type `git commit`
```
#!/bin/sh
has_passwd=$(git diff HEAD | grep -iE "password|passwd")
warning='Your commit has password and you may reveal your private information!\n
If you want to commit it, please commit with the option -n or --no-verify to bypass the hook!\n
\tgit commit -n\n
Use the following command to get the unsecrue information\n
\tgit diff HEAD'

if [ -n has_passwd ]; then
    echo $warning
    exit 1
fi
```

This script is to dectect whether the-to-commit file has password. If it has, the progress will be stopped. but if you still want to commit it, use `git commit -n` to bypass the hooks

## Add hooks to global config
```
git config --global init.templatedir=`~/.git.init.templateDir`
```
add your hook script to `~/.git.init.templateDir/hooks/`

When you initialize a new repository with git init, the files in `~/.git.init.templateDir/` will be copy to `.git` in your current directory, thus the hook is copyed into current `.git/hooks/pre-commit` and the hook will take effect in the repository, that if you push a commit with password, it will give you a prompt.


## Reference
[1 from git-scim](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)
