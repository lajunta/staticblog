Remove a file from all commit
===

In that case you could to use Git Filter Branch command with --tree-filter option.

syntax is git filter-branch --tree-filter <command> ...

```bash
git filter-branch --tree-filter 'rm -f Resources\Video\%font%.ttf' -- --all
```


Explanation about the command:

`< command >`: Specify any shell command.

`--tree-filter`: Git will check each commit out into working directory, run your command, and re-commit.

`--all`: Filter all commits in all branches.

Note: Kindly check the path for your file as I'm not sure for the file path

Hope this help you !!!


Removing a file added in the most recent unpushed commit
---

If the file was added with your most recent commit, and you have not pushed to GitHub, you can delete the file and amend the commit:

1. Open Terminal.

1. Change the current working directory to your local repository.

1. To remove the file, enter `git rm --cached`:

```bash
$ git rm --cached giant_file
# Stage our giant file for removal, but leave it on disk
```

4. Commit this change using --amend -CHEAD:

```bash
$ git commit --amend -CHEAD
# Amend the previous commit with your change
# Simply making a new commit won't work, as you need
# to remove the file from the unpushed history as well
```
5. Push your commits to GitHub:

```bash
$ git push
# Push our rewritten, smaller commit
```

Removing a file that was added in an earlier commit
---

Following this steps:

```bash
$ git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA" \
  --prune-empty --tag-name-filter cat -- --all

$ echo "YOUR-FILE-WITH-SENSITIVE-DATA" >> .gitignore

$ git add .gitignore

$ git commit -m "Add YOUR-FILE-WITH-SENSITIVE-DATA to .gitignore"

$ git push origin --force --all

```

In order to remove the sensitive file from your tagged releases, you'll also need to force-push against your Git tags:

```bash
$ git push origin --force --tags
```