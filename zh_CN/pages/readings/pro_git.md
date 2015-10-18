
# Pro Git Notes
This is a brief notes about some points of *Pro Git*. It only contains first 3
chapters (contains most commonly used commonds of Git). I will maintain this
article and keep revising it. Hope it might helpful to you.
##  Before Running Git

+ **Snapshots, Not Differences**: The major difference between Git and any other
VCS (Subversion and friends included) is the way Git thinks about its data.
Conceptually, most other systems store information as a list of file-based changes.

+ **Nearly Every Operation Is Local**: Most operations in Git only need local files
and resources to operate -- generally no information is needed from another computer
 on your network.

+ **Git Has Integrity**: Git use checksum, namely, SHA-1, as a reference.

+ **Three States**: Git has three main states that your files can reside in:
 *committed*, *modified*, and *staged*.  Committed means that the data is safely
stored in your local database.  Modified means that you have changed the file but
have not committed it to your database yet. Staged means that you have marked a
modified file in its current version to go into your next commit snapshot.    

  ![](http://i.imgur.com/yT4Pg.png)

The basic Git workflow goes something like this:
1.You modify files in your working directory.
2.You stage the files, adding snapshots of them to your staging area.
3.You do a commit, which takes the files as they are in the staging area and
stores that snapshot permanently to your Git directory.

##  Git Basics
- **Clone**:  `git clone git://github.com/USERNAME/REPO.git`
- **Check Status**: `git status`
- **Trace New file**: `git add FILE` (The FILE edited after `git add`
need to redo `git add` to store the changes)
- **.gitignore**: if you want to ignore some files, add their pattern to .gitignore.
Like *.[oa] (ignore *.a *.o files)
> Glob patterns are like simplified regular expressions that shells
  use. An asterisk (*) matches zero or more characters; [abc]matches any
  character inside the brackets (in this case a, b, or c); a question
  mark (?) matches a single character; and brackets enclosing characters
  separated by a hyphen([0-9]) matches any character between them (in
  this case 0 through 9) .

- **Diff between staged and last commit file**: `git diff --cached`
- **Committing Changes** : `git status` before `git commit`. add *-m* flag to add
commit message, *-v* to explicit remind every place you modified, *-a* to automatically
stage every file that is already tracked before doing the commit, letting skip the
`git add` part.
- **Remove** : `rm FILE` and `git rm FILE` to delete local files. `git rm --cached FILE`
to remove files in repository.(Could use glob)
- **Move** : `git mv OLD NEW` to change the names.
- **Viewing History** : `git log`, add *-p* to show differences in every commits,
use *-2* to see lasted 2 updates, *--stat* abbreviated stats, *--pretty* customized
formats.
- **Undo** : use `git commit --amend` to add forgetted files or reedit commit
messages. (See 2.4.1)
- **Unstaging Staged File** : `git reset HEAD FILE`
- **Unmodifying a Modified File** : use `git status` (See 2.4.3)
- **Fetching and Pulling** : `git fetch REMOTE-NAME` pluus down all the data from
that remote project that you don't have yet.
> git fetch originfetches any new work that has been pushed to that
  server since you cloned (or last fetched from) it. It¡¯s important to
  note that the fetch command pulls the data to your local repository ¡ª
  it doesn¡¯t automatically merge it with any of your work or modify
  what you¡¯re currently working on. You have to merge it manually into
  your work when you¡¯re ready.

- **Push** : `git push origin master` will push local master branch to origin server.
- **Tagging** : `git tag` to add tags.*-a* to annotated, *-m* add comments, `git show`
to check version info.
- **Git Aliases** : You can set up an alias for each command with `git config`.
For example, using `git config --global alias.ci commit` to instead `git commit`
with `git ci`. Useful Tips: `git config --global alias.last 'log -1 HEAD'` to check
last commit by `git last`.

##  Git Branches
> **HEAD**: In Git, this is apointer to the local branch you're currently
 on.

> ![](http://i.imgur.com/CRIgyqN.png)

> **New Branch** :Because a branch in Git is in actuality a simple file
that contains the 40 character SHA-1 checksum of the commit it points
to, branches are cheap to create and destroy. Creating a new branch is
as quick and simple as writing 41 bytes to a file (40 characters and a
newline).

- **Basic Branching and Merging** : switch to a certain branch `git Check -b BRANCH-B`,
`git merge` to merge to master.
- **Delete Branch** : `git branch -d` to delete branches. It could be failed if it
was unmerged. Thus, you could use `git branch -D BRANCH-B` to force deleting it,
and `git branch --no-merged` to check unmerged work.
- **Remote Branch** : You'd better read the related part of this book! It is a little
confusing but important.
- **Rebasing** : With the rebase command, you can take all the changes that were
committed on one branch and replay them on another one. For example, `git rebase
master server` checks out the topic branch (in this case,server) for you and replays
it onto the base branch (master).It quite cool, read 3.5.1 to know more. Summed
up in one line *Do not rebase commits that you have pushed to a public repository.*
