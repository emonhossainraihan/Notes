>  Using the option -am allows you to add and create a message for the commit in one command.

# রিসেট এবং অ্যামেন্ড

```bash
git reset --soft <hash>
git diff HEAD
git show <hash>
```

## Undo reset

```bash
git reflog
git reset HEAD@{number}
git reset --hard
```

> If commit was pushed then never try to reset. Instead of using reset use revert 

## Add some change into previous commit

```bash
git commit --amend
```

# স্ট্যাশ এবং ক্লিন

you don’t want to do a commit: Stashing 

```bash
git stash
```

```bash
git stash pop/apply 
git stash pop stash@{number}
git stash clean
```

```bash
git clean -f -n
git clean -f
```

# রিমোট, পুশ এবং পুল

```bash
git remote show
```

## fetch master from server

```bash
git pull origin master
```

# কনফ্লিক্ট ফিক্স করা

When several contributor change something in same line

# ফর্ক করা রিপোজিটরীকে আপটুডেট রাখা

new pull request > switching the base

left:mine right:remote
