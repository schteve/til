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

# But wait!

Maybe that didn't completely do what I want. When I wrote this, I had `pull.rebase = true`. This meant each pull would rebase the repo on top of the other ones, resulting in a completely linear history. This is fine, but the victory feels hollow. It might be nice to have each repo's history in parallel until the point where we merged them together. So, let's try to do this with merges instead of (rebase) pulls. And let's do it with an octopus merge so each repo remains intact as its own branch until they're all merged together.

To make this easy, let's start with a fresh new repo and add each other repo as a remote. `-f` to fetch the remote's data in the same step.

> `git remote add -f name url`

## A quick detour

Now, a quick detour because the obvious (to me) strategy didn't work. Don't actually do any of the steps in this section if you just want results.

Seemingly, we should be able to do an octopus merge pretty easily. Just invoke the merge with each repo and tell it to allow unrelated histories.

> `git merge --allow-unrelated-histories repo-a/main repo-b/main repo-c/main`

But we get this error:
> `fatal: Can merge only exactly one commit into empty head`

Well, I guess that makes sense but we don't really care about the empty head - we just want to smash all of the repo branches together. So let's try making an empty commit to start from and then attempt the merge again.

> `git commit -m "Empty commit" --allow-empty`

> `git merge --allow-unrelated-histories repo-a/main repo-b/main repo-c/main`

But we get a new error:
> `Unable to find common commit with repo-a/main`\
`Automatic merge failed; fix conflicts and then commit the result.`

Well, there shouldn't be any conflicts since all files are in separate directories so it's not clear why this fails. End detour.

## Back on the road

Let's take the approach of starting with one repo and merging the others in one by one, no octopus merge. Then we can fix up the repo to make it into an octopus.

Check out one of the repos and merge in the others one by one. Each time it will generate a merge commit between the growing merged codebase and the next repo.

> `git checkout --track repo-a/main`

> `git merge --allow-unrelated-histories repo-b/main`

> `git merge --allow-unrelated-histories repo-c/main`

If you didn't already, give the final merge commit a nice description.
> `git commit --amend`

OK, this is now looking close to what we want but has a bit of an ugly merge history at the end. Let's fix that up with some deep magic. We replace the final merge commit with a new one, and the new one is a commit formed by grafting together the tip of each repo's main branch with the merged result. Essentially, this manually creates the octopus we wanted from the start, and discards the other merge commits.

> `git replace --graft x a b c`

Where:
- `x` is the hash of the final merge commit (`git show`)
- `a`, `b`, `c` are the hashes of the repo branches. You can find these quickly with `git branch -vv --all`

Now if you check the commit graph there is a new dangling octopus merge, and the previous final merge commit is marked `replaced`. We want to keep the dangling octopus and remove the other merge commit so reset the current branch to the octopus.

> `git reset --hard x`

Where `x` is the octopus hash.

Then we can delete the replaced commit since it's no longer needed.

> `git replace -d x`

Where `x` is the same as the previous `git replace` invocation.

Now if you check the commit history everything should look good. The graph shows parallel branches that merge at the very top, and the log intersperses the commits according to when they were committed.

Last thing to do is some cleanup. You might want to rename the merged branch, remove its tracking info, and remove the remotes. Then you can add the repo's new remote home and push it there.

> `git branch -m new-name`

> `git branch --unset-upstream`

> `git remote remove repo-a`

# References

- https://stackoverflow.com/questions/1425892/how-do-you-merge-two-git-repositories
- https://stackoverflow.com/questions/277029/combining-multiple-git-repositories
