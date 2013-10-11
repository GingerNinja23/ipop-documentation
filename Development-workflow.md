1.  [Fork the repo](http://help.github.com/fork-a-repo/).

2.  Clone your newly-forked repo to your local machine (copy the clone
    URL from the repository page on GitHub):

        $ git clone <clone URL>

3.  Configure the upstream remote (copy the clone URL from the
    corresponding ipop-project repo page):

        $ cd <reponame>
        $ git remote add upstream <clone URL>

4.  Create a branch for the feature you're working on:

        $ git checkout -b my-feature

5.  Develop on **my-feature** branch only, but **Do not merge the
    upstream master with your development branch!!**

6.  Commit changes to **my-feature** branch:

        $ git add .
        $ git commit -m "commit message"

7.  Push branch to GitHub, to allow your mentor to review your code:

        $ git push origin my-feature

8.  Repeat steps 5-7 till development is complete.

9.  Fetch upstream changes that were done by other contributors:

        $ git fetch upstream

10. Rebase **my-feature** branch on top of the upstream master:

        $ git rebase upstream/master

11. In the process of the **rebase**, it may discover conflicts. In that
    case it will stop and allow you to fix the conflicts. After fixing
    conflicts, use `git add .` to update the index with those
    contents, and then run:

        $ git rebase --continue

12. [Force] push branch to GitHub. If there were conflicts in the
    previous step, rebasing will have re-created your commits, so a
    regular push will fail. **NEVER `--force` to a branch that other
    people are using!**

        $ git push origin my-feature --force

13. Only after all testing is done - Send a [pull
    request](http://help.github.com/send-pull-requests/). Attention:
    Please recheck that in your pull request you send only your changes,
    and no other changes!! Check it by command:

        $ git diff my-new-check upstream/master

More detailed information you can find on
[Git-Workflow](https://github.com/diaspora/diaspora/wiki/Git-Workflow),
[Git-rebase (Manual
Page)](http://kernel.org/pub/software/scm/git/docs/git-rebase.html) and
[Rebasing](http://book.git-scm.com/4_rebasing.html).

- - - 

This document is based on
<https://github.com/sevntu-checkstyle/sevntu.checkstyle/wiki/Development-workflow-with-Git%3A-Fork,-Branching,-Commits,-and-Pull-Request>.
