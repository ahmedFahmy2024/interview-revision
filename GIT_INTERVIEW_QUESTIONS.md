# Git interview questions and answers

Use these questions to explain how Git records work, how you collaborate, and how you recover from mistakes. Commands that discard work need a clear statement of which files or commits they affect.

## Study checklist

- [ ] Explain the working tree, index, and commit history without a diagram.
- [ ] State whether a command rewrites history, changes files, or both.
- [ ] Practice resolving a conflict and recovering a lost commit in a scratch repository.

## Git model and daily work

Sources: [Git user manual](https://git-scm.com/docs/user-manual), [git-add](https://git-scm.com/docs/git-add), [git-status](https://git-scm.com/docs/git-status), [git-diff](https://git-scm.com/docs/git-diff).

### 1. What problem does a version control system solve?

It records changes over time so a team can inspect history, collaborate, compare versions, and recover earlier work. Git is distributed: each full clone has its own repository history.

### 2. What are the working tree, index, and `HEAD`?

The working tree holds checked-out files. The index, also called the staging area, describes the next commit. `HEAD` identifies the current commit, usually through the current branch.

### 3. What does `git add` do?

It copies the selected file content into the index. Later edits to the same file stay unstaged until you add them again. `git diff` shows unstaged changes; `git diff --cached` shows staged changes.

### 4. What does a Git commit contain?

A commit points to a tree snapshot, records parent commit IDs, and includes author, committer, timestamp, and message metadata. A commit ID identifies its content and metadata.

### 5. What is the difference between a branch and a tag?

A branch is a movable reference that normally advances with new commits. A tag names a specific object and is commonly used to mark a release; tags do not advance automatically.

### 6. What is detached `HEAD`?

`HEAD` points directly to a commit rather than a branch. You can inspect or commit there, but create a branch before leaving if you want a durable name for new commits.

### 7. What is the difference between `git status`, `git log`, and `git diff`?

`status` shows the current branch and file states. `log` shows commit history. `diff` compares content between the working tree, index, and selected commits.

### 8. What belongs in `.gitignore`?

Generated output, local caches, and machine-specific files that should not enter the repository. Ignore rules do not stop tracking a file already committed; remove it from the index if it must become untracked.

### 9. What is the difference between `git fetch` and `git pull`?

`fetch` downloads remote commits and updates remote-tracking references without changing your current branch. `pull` fetches and then integrates the selected remote branch by fast-forward, merge, or rebase according to the command and configuration.

### 10. What are `origin` and an upstream branch?

`origin` is a conventional remote name, not a special server type. An upstream branch is the branch your local branch tracks for commands such as `git pull` and status comparisons.

## Combining and moving commits

Sources: [git-merge](https://git-scm.com/docs/git-merge), [git-rebase](https://git-scm.com/docs/git-rebase), [git-cherry-pick](https://git-scm.com/docs/git-cherry-pick), [git-pull](https://git-scm.com/docs/git-pull).

### 11. What is a fast-forward merge?

Git moves the branch reference forward when the target branch is already an ancestor of the incoming branch. No new merge commit is needed.

### 12. What does a merge commit record?

It joins histories and has multiple parents. It preserves the commits on both branches and records the combined result, including any conflict resolution.

### 13. How does rebase differ from merge?

Rebase reapplies selected commits on a new base, creating new commit IDs. Merge joins histories and can add a merge commit. Rebase can make a local branch linear, but rewriting published commits disrupts collaborators.

### 14. What is an interactive rebase used for?

It can reorder, combine, edit, or drop commits before sharing a branch. Review the resulting diff and avoid rewriting commits that others already depend on.

### 15. What does `git cherry-pick` do?

It applies the change introduced by selected commits onto the current branch, creating new commits. It is useful for a targeted backport, but repeated cherry-picks can complicate history.

### 16. How do you resolve a merge conflict?

Inspect the conflicting files and both intended changes, edit the final content, stage the resolved files, and finish the merge or rebase. Run the relevant checks before sharing the result.

### 17. What do `ours` and `theirs` mean during a rebase conflict?

During a rebase, `ours` refers to the branch state you are rebasing onto and `theirs` to the commit being replayed. This can feel reversed from the feature branch you started on, so inspect the content before choosing a side.

### 18. What is a squash merge?

It takes the combined changes from a branch and commits them as one new commit on the target branch. Individual feature commits remain on the source branch but do not become ancestors of the target through that squash commit.

## Undo and recovery

Sources: [git-restore](https://git-scm.com/docs/git-restore), [git-reset](https://git-scm.com/docs/git-reset), [git-revert](https://git-scm.com/docs/git-revert), [git-reflog](https://git-scm.com/docs/git-reflog), [git-stash](https://git-scm.com/docs/git-stash).

### 19. How do `git restore`, `git reset`, and `git revert` differ?

`restore` updates working tree or index files from a source. `reset` moves `HEAD` or changes the index, with modes that may also change files. `revert` creates a new commit that reverses an earlier commit's changes.

### 20. What do `git reset --soft`, `--mixed`, and `--hard` change?

All move the current branch to the target commit. `--soft` keeps index and working tree; `--mixed` resets the index but keeps working files; `--hard` resets both and can discard local work.

### 21. How do you undo a bad commit already shared with the team?

Usually use `git revert` so the shared history stays intact. Review dependencies on later commits before reverting; reverting a merge needs an explicit mainline parent choice.

### 22. How do you unstage a file without discarding edits?

Use `git restore --staged <file>`. The working tree copy remains. Check `git status` before and after.

### 23. What is `git reflog` for?

It records where local references such as `HEAD` pointed recently. It can help recover a commit after a reset or rebase while the object still exists locally; it is not a shared backup.

### 24. What does `git stash` save?

It saves local working tree and index changes so you can switch tasks, then reapply them later. Stashes can conflict on application. Important long-lived work is easier to track on a branch with commits.

### 25. How do you recover from an interrupted rebase?

Use `git status` to see the current step. Resolve and continue with `git rebase --continue`, skip a commit only if its change is unnecessary, or return to the pre-rebase state with `git rebase --abort`.

### 26. What does `git clean` remove?

It removes untracked files and directories selected by its flags. Preview with `git clean -n` before using destructive options. It does not replace `git restore` for tracked changes.

## Collaboration and repository hygiene

Sources: [git-worktree](https://git-scm.com/docs/git-worktree), [git-bisect](https://git-scm.com/docs/git-bisect), [git-blame](https://git-scm.com/docs/git-blame), [GitHub pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests), [Git LFS](https://git-lfs.com/).

### 27. Why keep pull requests small?

Reviewers can understand the change and its risk sooner. Smaller changes are easier to test, revert, and deploy. Split by behavior rather than arbitrary file count.

### 28. What is `git worktree`?

It creates another working tree linked to the same repository, letting you check out different branches in separate directories without duplicating the repository's object store.

### 29. When should you use `git bisect`?

Use it when you know one good and one bad commit and can reproduce the regression. Git narrows the range by asking you to classify candidate commits as good or bad.

### 30. What does `git blame` tell you, and what does it not tell you?

It shows the last commit that changed each line. It does not explain intent or identify who originally designed a behavior. Read the commit and surrounding history before drawing conclusions.

### 31. How should secrets accidentally committed to Git be handled?

Revoke or rotate the secret first. Remove it from the current tree and, when necessary, coordinate a history rewrite and repository cleanup. A deletion commit alone leaves the secret in earlier commits.

### 32. When is Git LFS useful?

It stores large file contents outside normal Git objects while Git tracks pointer files. It can reduce pressure from large binary assets, but the team and CI still need LFS access.

### 33. What is the difference between a submodule and a subtree?

A submodule records a commit from another repository and requires separate checkout/update steps. A subtree copies another project's content into the main repository history. Choose based on ownership and update workflow.

### 34. Why might a force push be dangerous?

It replaces a remote branch tip and can remove commits that another person pushed. If rewriting your own branch is necessary, `--force-with-lease` checks that the remote still points where you expect, but coordination is still required.

## Practical interview scenarios

### 35. You committed a secret and pushed it. What do you do first?

Revoke the credential, assess access and logs, then remove it from code and coordinate any history cleanup. Treat the exposed credential as compromised even if the commit is later removed.

### 36. Your local branch has diverged from the remote. What do you inspect?

Fetch, compare both sides with `git log --graph` or `git log <remote>..<local>`, and choose merge, rebase, or a new branch based on whether the local commits are shared. Do not reset until you know which commits would be lost.

### 37. A bad change reached the main branch. How do you undo it safely?

Find the commit, inspect whether later work depends on it, and revert it through the normal review and deployment path. Verify the result and keep the original commit in shared history.

### 38. A rebase seems to have lost your commit. Where do you look?

Check `git status`, `git log`, and `git reflog`. Create a branch at the recovered commit before further cleanup so it has a durable reference.

## Further reading

- [Git reference](https://git-scm.com/docs)
- [Pro Git book](https://git-scm.com/book/en/v2)
