---
description: version control system
---

# 📖 Git

## links

[https://git-scm.com/](https://git-scm.com/) официальная дока

[https://github.com/github/gitignore](https://github.com/github/gitignore) подборка файлов .gitignore

[https://ohshitgit.com/](https://ohshitgit.com/)

[https://gitexplorer.com/](https://gitexplorer.com/)&#x20;

### About

Это система контроля версий. Пример - у нас есть файлы с которыми мы взаимодействуем, пишем данный текст в документе хрень.txt, соответственно данная система должна контролировать когда, кто и что записал в этот файл.

### fast start

```bash
cd {path/project}
git add .
git commit -m "commit name"
git push
```

## config

```bash
# check settings
git config --list --show-origin

# set global
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

# set local
git config --local user.name "Petr V."
git config --local user.email ptrpl4@ya.ru
```

## Commit

```bash
# add all staged files to commit
git commit -a -m "text"
```

## Push

```
# sent to remote repo
git push
# dafult = git push
git push origin master
```

## Теория

подход Git’а к хранению данных больше похож на набор снимков миниатюрной файловой системы

## .gitignore

К шаблонам в файле .gitignore применяются следующие правила:

* Пустые строки, а также строки, начинающиеся с #, игнорируются.
* Стандартные шаблоны являются глобальными и применяются рекурсивно для всего дерева каталогов.
* Чтобы избежать рекурсии используйте символ слеш (/) в начале шаблона.
* Чтобы исключить каталог добавьте слеш (/) в конец шаблона.
* Можно инвертировать шаблон, использовав восклицательный знак (!) в качестве первого символа.

## Переименование

```bash
$ git mv README.md README
# equal
$ mv README.md README
$ git rm README.md
$ git add README
```

## Лог

```shell
# all commits order by desc
git log
# show patch -p or --patch
git log -p -2
# short changed info --stat
git log --stat
# change view for data --pretty
git log --pretty
# additional options for --pretty
git log --pretty=oneline
# another addotional opt. - =short, =full, =fuller
# time options
git log --since=2.weeks
# serach for string -S
git log -S function_name
# commits related to specific file -- path/to/file
git log -- path/to/file
# exclude merges --no-merges
git log --no-merges
# branches and changes
git log --oneline --decorate
```

## Добавление в последний коммит

```bash
# add some chaged in last commit --amend
git commit --amend
```

## Отмена индексации

```
git reset HEAD filename
```

## Отмена изменений

```
git checkout -- filename
```

## Remote

```shell
# check remote repos with links
$ git remote -v
# check remote repo info
git remote show origin
```

## Fetch/Pull

```shell
# get data from origin repo
git fetch origin
# get + merge data from origin repo
git pull 
```

## Tag

```shell
# create Annotated Tags (full author info) and message
$ git tag -a v1.4 -m "my version 1.4"
$ git show v1.4
# Lightweight Tags (only tag info)
$ git tag v1.4-lw
# add tag to already created commit
$ git tag -a v1.2 9fceb02dfs22f332d
# push choosen tag to remote
$ git push origin <tagname>
# push all tags
$ git push --tags
# delete tag
$ git tag -d v1.4
# delete from remote
$ git push origin --delete <tagname>
# check tag file (not recommended)
$ git checkout v2.0.0
# create branch with tag (recommend)
$ git checkout -b version2 v2.0.0
```

## Branch

### basic

```
# current branches
git branch
# create branch
git branch test_branch
# switch to branch
git checkout test_branch
# create and switch
git checkout -b test_branch
# delete branch (-d, --delete)
git branch -d test_branch
# all branches + all last commits
git branch -v
# all not merged branches
git branch --no-merged
# all merged branches
git branch --merged
```

### renaming

```
# rename localy
$ git branch --move bad-branch-name corrected-branch-name
# push changes
$ git push --set-upstream origin corrected-branch-name
# delete branch
$ git push origin --delete bad-branch-name
```

## Merge

```
# add test_branch to current branch
git merge test_branch
```
