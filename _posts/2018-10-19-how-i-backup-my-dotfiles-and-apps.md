---
title: How I Backup/Sync my Dotfiles & Apps
published: true
description: How I use git hooks and some scripts to backup / sync my dotfiles
tags: git, bash, productivity, automation
cover_image: https://thepracticaldev.s3.amazonaws.com/i/nb6ovj9wqgr0v2s13fdh.jpg
---

My concern is to automatize the backup of my configuration files (aka dotfiles) and to generate a snapshot of all installed apps, whether it be through my distro's package manager or through third-party package manager like `npm` or `pip`. This is the first step before being able to sync my apps automatically on a newly installed computer (still to be done).

## <i class="fas fa-terminal"></i> Backuping dotfiles

What I did is create a git repo in my `$HOME` dir as all my dotfiles are located here. But as I don't want to push all the other files (Pictures, Downloads, Desktop...), I put a `.gitignore` file with just a wildcard (`*`) in it. This way, when I want to add a file to the repo, I have to force with: `git add -f newfile`.

```shell
cd ~
git init
echo "*" > .gitignore
git add -f .gitignore
git add -f .bashrc
git add -f .zshrc
git add -f .config/fish
git add -f .tmux.conf
git add -f .SpaceVim.d/init.toml
git commit -m "First commit"
git remote add origin git@github.com:username/mydotfiles
git push -u origin master
```

⚠️ But be aware that it could be dangerous for your sensible data. For example, don't push your `~/.ssh` config!

Thus, you have a quick way to retrieve your configuration in case of a computer fresh install or in case of a crash.

The next step is to do the same with your apps.

## <i class="fas fa-terminal"></i> Generate a snapshot of your apps

Regarding the apps, it's not so easy. It's not unusual to have several package managers to install new apps. Personally, I use 7 of them: `npm`, `cargo`, `ghc-pkg`, `composer`, `gem`, `pip`, and `pacman`, the manager of my Manjaro linux distribution. Each of them has its own syntax to install, delete or update packages.
But each of them also has its own syntax to output a list of installed packages.

So basically, I just use the following bash script to generate the list of all installed packages, across all the package managers:

```shell
#!/bin/bash

echo "Generating the lists of explicitly installed packages in ~/.backup"

pacman -Qe > ~/.backup/pacman_packages || echo "pacman failed"
gem list > ~/.backup/gem_packages || echo "gem failed"
npm list -g --depth=0 > ~/.backup/npm_packages || echo "npm failed"
pip list > ~/.backup/pip_packages || echo "pip failed"
cargo --list | tail -n +2 | tr -d " " > ~/.backup/cargo_packages || echo "cargo failed"
ghc-pkg list > ~/.backup/ghc-pkg_packages || echo "ghc-pkg failed"
composer global show | cut -d ' ' -f1 > ~/.backup/composer_packages || echo "composer failed"

git add -f .backup

exit 0
```

Note the `git add` command at the end of the script. I do this to automatically add the `~/.backup` folder to the git index so that it gets pushed to the remote. And as I put this script in the `pre-commit` hook, my packages list gets synced with git each time I commit:

```
biros on  master [!]
➜ git commit -m "Update conf"
Generating the lists of explicitly installed packages in ~/.backup
[master 27f73e4] Update conf
 1 file changed, 1 insertion(+)

```

---

⏭️ Next step: be able to re-install all the apps automatically from the backup.

---

💡 Hint: you can sync your Atom plugins with the [sync-settings](http://atom.io/packages/sync-settings) plugin.

---

📚 Other resource on the same topic:

[Sharing dotfiles across platforms with a shell script](https://dev.to/amcsi/sharing-dotfiles-cross-platform-with-a-shell-script-o2j)
