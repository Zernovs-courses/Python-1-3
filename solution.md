### Задание 3.1
```bash
# Клонируем репозиторий
$ git clone https://github.com/smartiqaorg/geometric_lib
Cloning into 'geometric_lib'...
remote: Enumerating objects: 25, done.
remote: Counting objects: 100% (25/25), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 25 (delta 5), reused 22 (delta 3), pack-reused 0
Receiving objects: 100% (25/25), done.
Resolving deltas: 100% (5/5), done.

# Переходим в папку репозитория
$ cd geometric_lib/

# Убеждаемся, что на текущий момент есть только одна локальная ветка - main
$ git branch --all
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/develop
  remotes/origin/feature
  remotes/origin/main

# Создаем локальную ветку develop на основе существующей удаленной develop
$ git checkout develop
Branch 'develop' set up to track remote branch 'develop' from 'origin'.
Switched to a new branch 'develop'

# Создаем локальную ветку feature на основе существующей удаленной feature
$ git checkout feature
Branch 'feature' set up to track remote branch 'feature' from 'origin'.
Switched to a new branch 'feature'

# Убеждаемся в наличии трех локальных веток: main, develop и feature
$ git branch --all
  develop
* feature
  main
  remotes/origin/HEAD -> origin/main
  remotes/origin/develop
  remotes/origin/feature
  remotes/origin/main

# Переходим на ветку feature и смотрим список коммитов
$ git checkout feature
$ git log
commit 30494317cde4419be779c14306561e0eaa78b88b (HEAD -> feature, origin/feature)
Author: Daniil.K <dlkay@yandex.ru>
Date:   Tue Mar 30 17:36:09 2021 +0300

    L-04: Add rectangle.py

commit d078c8d9ee6155f3cb0e577d28d337b791de28e2 (origin/main, origin/HEAD, main)
Author: smartiqa <info@smartiqa.ru>
Date:   Thu Mar 4 14:55:29 2021 +0300

    L-03: Docs added

commit 8ba9aeb3cea847b63a91ac378a2a6db758682460
Author: smartiqa <info@smartiqa.ru>
Date:   Thu Mar 4 14:54:08 2021 +0300

    L-03: Circle and square added



# Откатываем последний коммит 304943
$ git revert HEAD
Removing rectangle.py
[feature d3fc1ef] Revert "L-04: Add rectangle.py"
 Committer: smartiqa <info@smartiqa.ru>
 1 file changed, 7 deletions(-)
 delete mode 100644 rectangle.py


# Убеждаемся, что появился новый revert-коммит d3fc1e, 
# Который отменяет изменения коммита 304943
$ git log
commit d3fc1ef3b872204ebac499255a1832bff5e35794 (HEAD -> feature)
Author: smartiqa <info@smartiqa.ru>
Date:   Wed Apr 7 10:44:30 2021 +0300

    Revert "L-04: Add rectangle.py"
    This reverts commit 30494317cde4419be779c14306561e0eaa78b88b.

commit 30494317cde4419be779c14306561e0eaa78b88b (origin/feature)
Author: Daniil.K <dlkay@yandex.ru>
Date:   Tue Mar 30 17:36:09 2021 +0300

    L-04: Add rectangle.py

commit d078c8d9ee6155f3cb0e577d28d337b791de28e2 (origin/main, origin/HEAD, main)
Author: smartiqa <info@smartiqa.ru>
Date:   Thu Mar 4 14:55:29 2021 +0300

    L-03: Docs added

commit 8ba9aeb3cea847b63a91ac378a2a6db758682460
Author: smartiqa <info@smartiqa.ru>
Date:   Thu Mar 4 14:54:08 2021 +0300

    L-03: Circle and square added

# Переходим на ветку develop
$ git checkout develop
Switched to branch 'develop'
Your branch is up to date with 'origin/develop'.


# Просматриваем историю коммитов ветки develop
# Коммиты b5b0fa и d76db2 можно объединить
$ git log
commit b5b0fae727ca72c317c383b39c0af73d6adcd81c (HEAD -> develop, origin/develop)
Author: Daniil.K <dlkay@yandex.ru>
Date:   Tue Mar 30 18:02:23 2021 +0300

    L-04: Update docs for calculate.py

commit d76db2ac7f69cc920ae2e6f669fb0671a7fa7d71
Author: Daniil.K <dlkay@yandex.ru>
Date:   Tue Mar 30 17:57:42 2021 +0300

    L-04: Add calculate.py

commit 51c40ebfd0e0b65f52fe5e54740cbb038e492db3
Author: smartiqa <info@smartiqa.ru>
Date:   Fri Mar 26 14:52:26 2021 +0300

    L-04: Doc updated for triangle

commit d080c7888b81955bad2ed78d58ad910526b5132a
Author: smartiqa <info@smartiqa.ru>
Date:   Fri Mar 26 14:48:39 2021 +0300

    L-04: Triangle added


# Выполняем soft reset на два коммита назад
$ git reset --soft HEAD~2


# Убеждаемся, что последние 2 коммита исчезли из истории
$ git log
commit 51c40ebfd0e0b65f52fe5e54740cbb038e492db3 (HEAD -> develop)
Author: smartiqa <info@smartiqa.ru>
Date:   Fri Mar 26 14:52:26 2021 +0300

    L-04: Doc updated for triangle

commit d080c7888b81955bad2ed78d58ad910526b5132a
Author: smartiqa <info@smartiqa.ru>
Date:   Fri Mar 26 14:48:39 2021 +0300

    L-04: Triangle added


# Так же убеждаемся, что после ресета файлы двух последних коммитов попали в индекс
$ git status
On branch develop

Changes to be committed:
	new file:   calculate.py
	modified:   docs/README.md


# Делаем объединяющий коммит
# Параметр -c позволяет нам взять информацию о коммите из последнего коммита b5b0fa
$ git commit -c ORIG_HEAD
[develop a2e6244] L-04: Added calculate.py and updated docs
 Author: Daniil.K <dlkay@yandex.ru>
 Date: Tue Mar 30 18:02:23 2021 +0300
 Committer: smartiqa <info@smartiqa.ru>
 2 files changed, 54 insertions(+), 12 deletions(-)
 create mode 100644 calculate.py
 rewrite docs/README.md (82%)


# Убеждаемся, что в истории появился новый коммит a2e624
# Который объединяет в себе коммиты b5b0fa и d76db2
$ git log
commit a2e6244e0666f9eb536fb8d006b2edd6913176cf (HEAD -> develop)
Author: Daniil.K <dlkay@yandex.ru>
Date:   Tue Mar 30 18:02:23 2021 +0300

    L-04: Added calculate.py and updated docs

commit 51c40ebfd0e0b65f52fe5e54740cbb038e492db3
Author: smartiqa <info@smartiqa.ru>
Date:   Fri Mar 26 14:52:26 2021 +0300

    L-04: Doc updated for triangle

commit d080c7888b81955bad2ed78d58ad910526b5132a
Author: smartiqa <info@smartiqa.ru>
Date:   Fri Mar 26 14:48:39 2021 +0300

    L-04: Triangle added

# Переходим на ветку main
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.


# Создаем новую ветку experiment и переходим на нее
$ git checkout -b experiment
Switched to a new branch 'experiment'


# Копируем в рабочую директорию файл docs/README.md из последнего коммита ветки develop
$ git restore --worktree docs/README.md --source develop


# Убеждаемся, что файл README.md теперь находится в рабочей директории 
$ git status
On branch experiment
Changes not staged for commit:
	modified:   docs/README.md


# Копируем в рабочую директорию И В ИНДЕКС файл calculate.py из последнего коммита ветки develop
$ git restore --worktree --staged calculate.py --source develop


# Убеждаемся, что файл calculate.py теперь находится в индексе 
$ git status
On branch experiment
Changes to be committed:
	new file:   calculate.py

Changes not staged for commit:
	modified:   docs/README.md


# Добавляем файл README.md в индекс
# Делаем коммит проиндексированных файлов (calculate.py и README.md)
$ git add docs/README.md
$ git commit -m "L-04: Experimental commit"
[experiment 4a268a1] L-04: Experimental commit
 2 files changed, 54 insertions(+), 10 deletions(-)
 create mode 100644 calculate.py
 rewrite docs/README.md (75%)


# Удаляем из рабочей копии файлы circle.py и square.py
$ git rm circle.py square.py 
rm 'circle.py'
rm 'square.py'


# Убеждаемся, что файлы попали к индекс как “Удаленные”
$ git status
On branch experiment
Changes to be committed:
	deleted:    circle.py
	deleted:    square.py


# Делаем коммит для окончательного удаления файлов
$ git commit -m "L-04: Deleted circle and square"
[experiment af22c91] L-04: Deleted circle and square
 2 files changed, 17 deletions(-)
 delete mode 100644 circle.py
 delete mode 100644 square.py


# Убеждаемся, что файлов больше нет в рабочей копии
$ tree
.
├── calculate.py
└── docs
    └── README.md

1 directory, 2 files

```

# Задание 3.2

```bash
# 1. Клонируем репозиторий
$ git clone https://github.com/smartiqaorg/geometric_lib.git
Cloning into 'geometric_lib'...
remote: Enumerating objects: 25, done.
remote: Counting objects: 100% (25/25), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 25 (delta 5), reused 21 (delta 3), pack-reused 0
Receiving objects: 100% (25/25), done.
Resolving deltas: 100% (5/5), done.

# 2. Переходим в папку репозитория
$ cd geometric_lib/

# 3. Последовательно переходим по веткам develop и release
# Это позволяет получить локальные копии remote веток develop и release
$ git merge develop --no-ff
$ git checkout develop
Branch 'develop' set up to track remote branch 'develop' from 'origin'.
Switched to a new branch 'develop'
$ git checkout release
Branch 'release' set up to track remote branch 'release' from 'origin'.
Switched to a new branch 'release'

# 4. Перед нами структура репозитория: 4 ветки - release, feature, develop, main
# Обратите внимание, что в данном уроке ветка feature не задействуется
$ git log --all --pretty=oneline --graph
* 86edb1c3dd57fa9abc7ba2ec7052507938084727 (origin/release) L-05: Update Docs. Add user agreement info
* 438b89a1dfc58d90e9036fe431771427965cd1ff L-05: Add user agreement
* 6adb96248a4d00d3bea13bd95d78ef52352cd1b4 L-03: Docs added
| * 30494317cde4419be779c14306561e0eaa78b88b (origin/feature) L-04: Add rectangle.py
| | * b5b0fae727ca72c317c383b39c0af73d6adcd81c (origin/develop) L-04: Update docs for calculate.py
| | * d76db2ac7f69cc920ae2e6f669fb0671a7fa7d71 L-04: Add calculate.py
| | * 51c40ebfd0e0b65f52fe5e54740cbb038e492db3 L-04: Doc updated for triangle
| | * d080c7888b81955bad2ed78d58ad910526b5132a L-04: Triangle added
| |/  
| * d078c8d9ee6155f3cb0e577d28d337b791de28e2 (HEAD -> main, origin/main, origin/HEAD) L-03: Docs added
|/  
* 8ba9aeb3cea847b63a91ac378a2a6db758682460 L-03: Circle and square added

# ----- Начальное состояние ------

# Структура веток
main            (1) -- (2)
develop                  \ -- (3) -- (4) -- (5) -- (6)

# Коммиты ветки main
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# Коммиты ветки develop
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# ----- После мерджа в no-ff режиме ------

# Структура веток
main            (1) -- (2) ----------------------------- (7)
develop                  \ -- (3) -- (4) -- (5) -- (6) -- /

# Коммиты ветки main
(7) commit fc6ee2 Merge branch 'develop'
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# Коммиты ветки develop
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# 1. Возвращаемся на ветку main
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

# 2. Вливаем ветку develop в ветку main в --no-ff режиме (мердж с дополнительным коммитом)
$ git merge develop --no-ff
Merge made by the 'recursive' strategy.
 calculate.py   | 33 +++++++++++++++++++++++++++++++++
 docs/README.md | 23 +++++++++++++++++------
 triangle.py    |  6 ++++++
 3 files changed, 56 insertions(+), 6 deletions(-)
 create mode 100644 calculate.py
 create mode 100644 triangle.py
 
# 3. После мерджа получили дополнительный коммит fc6ee2
$ git log --all --pretty=oneline --graph
*   fc6ee242c15cadbb5197c918f959cd3ba52553a4 (HEAD -> main) Merge branch 'develop'
|\  
| * b5b0fae727ca72c317c383b39c0af73d6adcd81c (origin/develop, develop) L-04: Update docs for calculate.py
| * d76db2ac7f69cc920ae2e6f669fb0671a7fa7d71 L-04: Add calculate.py
| * 51c40ebfd0e0b65f52fe5e54740cbb038e492db3 L-04: Doc updated for triangle
| * d080c7888b81955bad2ed78d58ad910526b5132a L-04: Triangle added
|/  
| * 86edb1c3dd57fa9abc7ba2ec7052507938084727 (origin/release, release) L-05: Update Docs. Add user agreement info
| * 438b89a1dfc58d90e9036fe431771427965cd1ff L-05: Add user agreement
| * 6adb96248a4d00d3bea13bd95d78ef52352cd1b4 L-03: Docs added
| | * 30494317cde4419be779c14306561e0eaa78b88b (origin/feature) L-04: Add rectangle.py
| |/  
|/|   
* | d078c8d9ee6155f3cb0e577d28d337b791de28e2 (origin/main, origin/HEAD) L-03: Docs added
|/  
* 8ba9aeb3cea847b63a91ac378a2a6db758682460 L-03: Circle and square added

# ----- Начальное состояние ------

# Структура веток
main            (1) -- (2)
develop                  \ -- (3) -- (4) -- (5) -- (6)

# Коммиты ветки main
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# Коммиты ветки develop
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# ----- После мерджа в fast-forward режиме ------

# Структура веток
main / develop      (1) -- (2) -- (3) -- (4) -- (5) -- (6) 
               

# Коммиты ветки main
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# Коммиты ветки develop
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# 1. Удаляем коммит, полученный в предыдущем задании
$ git reset --hard HEAD^
HEAD is now at d078c8d L-03: Docs added

# 2. Репозиторий вернулся к изначальному состоянию
$ git log --all --pretty=oneline --graph
* 86edb1c3dd57fa9abc7ba2ec7052507938084727 (origin/release, release) L-05: Update Docs. Add user agreement info
* 438b89a1dfc58d90e9036fe431771427965cd1ff L-05: Add user agreement
* 6adb96248a4d00d3bea13bd95d78ef52352cd1b4 L-03: Docs added
| * 30494317cde4419be779c14306561e0eaa78b88b (origin/feature) L-04: Add rectangle.py
| | * b5b0fae727ca72c317c383b39c0af73d6adcd81c (origin/develop, develop) L-04: Update docs for calculate.py
| | * d76db2ac7f69cc920ae2e6f669fb0671a7fa7d71 L-04: Add calculate.py
| | * 51c40ebfd0e0b65f52fe5e54740cbb038e492db3 L-04: Doc updated for triangle
| | * d080c7888b81955bad2ed78d58ad910526b5132a L-04: Triangle added
| |/  
| * d078c8d9ee6155f3cb0e577d28d337b791de28e2 (HEAD -> main, origin/main, origin/HEAD) L-03: Docs added
|/  
* 8ba9aeb3cea847b63a91ac378a2a6db758682460 L-03: Circle and square added

# 3. Вливаем ветку develop в main в ff-режиме (перемотка без дополнительного коммита)
$ git merge develop --ff
Updating d078c8d..b5b0fae
Fast-forward
 calculate.py   | 33 +++++++++++++++++++++++++++++++++
 docs/README.md | 23 +++++++++++++++++------
 triangle.py    |  6 ++++++
 3 files changed, 56 insertions(+), 6 deletions(-)
 create mode 100644 calculate.py
 create mode 100644 triangle.py
 
# 4. После мерджа ветки main и develop находятся на уровне одного коммита b5b0fa
$ git log --all --pretty=oneline --graph
* 86edb1c3dd57fa9abc7ba2ec7052507938084727 (origin/release, release) L-05: Update Docs. Add user agreement info
* 438b89a1dfc58d90e9036fe431771427965cd1ff L-05: Add user agreement
* 6adb96248a4d00d3bea13bd95d78ef52352cd1b4 L-03: Docs added
| * 30494317cde4419be779c14306561e0eaa78b88b (origin/feature) L-04: Add rectangle.py
| | * b5b0fae727ca72c317c383b39c0af73d6adcd81c (HEAD -> main, origin/develop, develop) L-04: Update docs for calculate.py
| | * d76db2ac7f69cc920ae2e6f669fb0671a7fa7d71 L-04: Add calculate.py
| | * 51c40ebfd0e0b65f52fe5e54740cbb038e492db3 L-04: Doc updated for triangle
| | * d080c7888b81955bad2ed78d58ad910526b5132a L-04: Triangle added
| |/  
| * d078c8d9ee6155f3cb0e577d28d337b791de28e2 (origin/main, origin/HEAD) L-03: Docs added
|/  
* 8ba9aeb3cea847b63a91ac378a2a6db758682460 L-03: Circle and square added

# ----- Начальное состояние ------

# Структура веток
main          (1) -- (2) -- (3) -- (4) -- (5) -- (6)
release                 \ ------------------------------- (7) -- (8)

# Коммиты ветки main ДО ребейза
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# Коммиты ветки release ДО ребейза
(8) commit 86edb1 L-05: Update Docs. Add user agreement info (Этот коммит будет слит в один общий)
(7) commit 438b89 L-05: Add user agreement (Этот коммит будет слит в один общий)
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# ----- После ребейза ------

# Структура веток
main         (1) -- (2) -- (3) -- (4) -- (5) -- (6)
release                                           \ -- (9)

# Коммиты ветки main ПОСЛЕ ребейза
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# Коммиты ветки release ПОСЛЕ ребейза
(9) commit 34d1f3 L-05: Add user agreement (объединенный коммит)
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# 1. Попросим Git выводить базу слияния при просмотре конфликта
$ git config merge.conflictstyle diff3

# 2. Установим meld в качестве тулзы для разрешения конфликтов
# Обратите внимание, что meld должен быть предварительно установлен в системе
$ git config --global merge.tool meld

# 3. Перейдм на ветку release
$ git checkout release 
Switched to branch 'release'
Your branch is up to date with 'origin/release'.

# 4. Выполняем интерактивный ребейз ветки release относительно main
$ git rebase -i main

# 5. В открывшемся окне редактора указываем, что хотим слить два коммита в один
# ------------- .git/rebase-merge/git-rebase-todo
# pick 438b89a L-05: Add user agreement
# squash 86edb1c L-05: Update Docs. Add user agreement info
# 
#  Rebase b5b0fae..86edb1c onto b5b0fae (2 commands)
# 
#  Commands:
...
# 
#  However, if you remove everything, the rebase will be aborted.
# 
#  ------------- 
# Сохраняем введенные изменения и выходим 
# (для редактора nano последовательно жмем Ctrl + X, затем Y, и, наконец, Enter).

# 6. У нас возникает конфликт в коммите (7) 438b89a в файле docs/README.md
Auto-merging docs/README.md
CONFLICT (content): Merge conflict in docs/README.md
error: could not apply 438b89a... L-05: Add user agreement
Resolve all conflicts manually, mark them as resolved with
Could not apply 438b89a... L-05: Add user agreement

# 7. Разрешаем конфликт
$ git mergetool

# 8. Открывается meld. В первом окне - содержимое ветки main (LOCAL), 
# в третьем - содержимое ветки release (REMOTE). 
# Второе окно (посередине) - результат слияния.

# ----- Начальное состояние ------

# Структура веток
main         (1) -- (2) -- (3) -- (4) -- (5) -- (6)
release                                           \ -- (9)

# Коммиты ветки main ДО мерджа
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# Коммиты ветки release ДО мерджа
(9) commit 34d1f3 L-05: Add user agreement (объединенный коммит)
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# ----- После мерджа ------

# Структура веток
main / release         (1) -- (2) -- (3) -- (4) -- (5) -- (6) -- (9)

# Коммиты веток main и release ПОСЛЕ мерджа
(9) commit 34d1f3 L-05: Add user agreement (объединенный коммит)
(6) commit b5b0fa L-04: Update docs for calculate.py
(5) commit d76db2 L-04: Add calculate.py
(4) commit 51c40e L-04: Doc updated for triangle
(3) commit d080c7 L-04: Triangle added
(2) commit d078c8 L-03: Docs added
(1) commit 8ba9ae L-03: Circle and square added

# 1. Переключаемся на ветку main
$ git checkout main 

# 2. Выполняем fast-forward мердж ветки release в ветку main
$ git merge release --ff
Updating b5b0fae..34d1f37
Fast-forward
 docs/README.md     |  6 ++++++
 user_agreement.txt | 14 ++++++++++++++
 2 files changed, 20 insertions(+)
 create mode 100644 user_agreement.txt
 
# 3. Финальное состояние веток
# После слияния обе ветки main и release указывают на один коммит последний коммит 34d1f3
$ git log --pretty=oneline --graph
* 34d1f378b040e2d51655b24cafd9dd4c5c6c0e3d (HEAD -> main, release) L-05: Add user agreement
* b5b0fae727ca72c317c383b39c0af73d6adcd81c (origin/develop, develop) L-04: Update docs for calculate.py
* d76db2ac7f69cc920ae2e6f669fb0671a7fa7d71 L-04: Add calculate.py
* 51c40ebfd0e0b65f52fe5e54740cbb038e492db3 L-04: Doc updated for triangle
* d080c7888b81955bad2ed78d58ad910526b5132a L-04: Triangle added
* d078c8d9ee6155f3cb0e577d28d337b791de28e2 (origin/main, origin/HEAD) L-03: Docs added
* 8ba9aeb3cea847b63a91ac378a2a6db758682460 L-03: Circle and square added
```

## Задача 3.3


