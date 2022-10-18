---
description: 슈퍼컴퓨팅인프라센터 2020. 4. 24. 13:54
---

# git 사용 방법 안내

## **1. 누리온 시스템**

누리온 시스템에서는 git이 기본 명령어로 설정되어 있지 않기 때문에, module 환경설정을 이용하여 사용 가능

```
 $ module load git/1.8.3.4
 $ git --help
 usage: git [--version] [--help] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p|--paginate|--no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

 The most commonly used git commands are:
   add        Add file contents to the index
   bisect     Find by binary search the change that introduced a bug
   branch     List, create, or delete branches
   checkout   Checkout a branch or paths to the working tree
   clone      Clone a repository into a new directory
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   fetch      Download objects and refs from another repository
   grep       Print lines matching a pattern
   init       Create an empty Git repository or reinitialize an existing one
   log        Show commit logs
   merge      Join two or more development histories together
   mv         Move or rename a file, a directory, or a symlink
   pull       Fetch from and merge with another repository or a local branch
   push       Update remote refs along with associated objects
   rebase     Forward-port local commits to the updated upstream head
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index
   show       Show various types of objects
   status     Show the working tree status
   tag        Create, list, delete or verify a tag object signed with GPG

 'git help -a' and 'git help -g' lists available subcommands and some
 concept guides. See 'git help <command>' or 'git help <concept>'
 to read about a specific subcommand or concept.
```



## **2. 뉴론 시스템**

뉴론 시스템에서는 git이 기본 명령어로 설정되어 있기 때문에, 별도의 환경설정 없이 사용 가능

```
 $ git --help
 usage: git [--version] [--help] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p|--paginate|--no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

 The most commonly used git commands are:
   add        Add file contents to the index
   bisect     Find by binary search the change that introduced a bug
   branch     List, create, or delete branches
   checkout   Checkout a branch or paths to the working tree
   clone      Clone a repository into a new directory
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   fetch      Download objects and refs from another repository
   grep       Print lines matching a pattern
   init       Create an empty Git repository or reinitialize an existing one
   log        Show commit logs
   merge      Join two or more development histories together
   mv         Move or rename a file, a directory, or a symlink
   pull       Fetch from and merge with another repository or a local branch
   push       Update remote refs along with associated objects
   rebase     Forward-port local commits to the updated upstream head
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index
   show       Show various types of objects
   status     Show the working tree status
   tag        Create, list, delete or verify a tag object signed with GPG

 'git help -a' and 'git help -g' lists available subcommands and some
 concept guides. See 'git help <command>' or 'git help <concept>'
 to read about a specific subcommand or concept.
```



※ git 사용 방법에 대한 자세한 설명은 git Documentation 문서를 참고해주시기 바랍니다.\
[https://git-scm.com/docs/git](https://git-scm.com/docs/git)
