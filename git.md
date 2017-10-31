# linux-guides: git

## Completely purge data from a repo

### Single file

```console
git filter-branch --force --index-filter \
'git rm --cached --ignore-unmatch PATH-TO-FILE' \
--prune-empty --tag-name-filter cat -- --all
```

### Entire Directory
```console
git filter-branch --force --index-filter \
'git rm -r --cached --ignore-unmatch PATH-TO-FILE' \
--prune-empty --tag-name-filter cat -- --all
```

*Credits: [GitHub Help](https://help.github.com/articles/removing-sensitive-data-from-a-repository/)*

## Merge two different git repos

To merge `project-a` into `project-b`:

```console
cd path/to/project-b
git remote add project-a path/to/project-a
git fetch project-a
git merge --allow-unrelated-histories project-a/master # or whichever branch you want to merge
git remote remove project-a
```

*Credits: [StackOverflow](https://stackoverflow.com/questions/1425892/how-do-you-merge-two-git-repositories)*