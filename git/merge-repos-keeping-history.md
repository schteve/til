# Merge repos while keeping history

You may have several repositories that you want to combine together into one. It's easy to do this without preserving history - just copy the files and commit them. But keeping the history is usually important so we want to do better.

If you're lucky enough that the repo structures don't overlap, then this is really easy. Just merge all of the repos into one using a pull (you might want to init a fresh repo first). Note that `--allow-unrelated-histories` may be unnecessary for newer version of git.
> `git pull other_repo --allow-unrelated-histories`

But, usually the repos overlap, and you want to put each original repo into its own directory. One way to handle this is to modify each repo's history so that its files are moved one directory lower before pulling them together. This way the repos live side by side, like:
> `new-repo/`\
> `├───repo-a/`\
> `├───repo-b/`\
> `└───repo-c/`

You can do this with `filter-branch`, which git has deprecated and shows some scary warnings about, but limited usage like this seems to be OK. Go into each repo and run this command, replacing DIR_NAME with the directory name the repo should go under. Then use a git pull as mentioned above.
> `git filter-branch --index-filter 'git ls-files -s | sed "s#\t#&DIR_NAME/#" | GIT_INDEX_FILE=$GIT_INDEX_FILE.new git update-index --index-info && mv $GIT_INDEX_FILE.new $GIT_INDEX_FILE' HEAD`

Note that the above operates on the current HEAD. If your repo has multiple branches you should run this command with each one checked out, and don't forget to also pull each branch into the new combined repo. If you have a bunch of branches then you should probably automate this!

# References

- https://stackoverflow.com/questions/1425892/how-do-you-merge-two-git-repositories
- https://stackoverflow.com/questions/277029/combining-multiple-git-repositories
