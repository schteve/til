# Review existing code

Usually code reviews happen on the changes proposed to a branch via a pull request. However, Github doesn't seem to have the ability to review code outside of a pull request. What if you want to review code that was pulled a long time ago, or code already accepted to someone else's repository? It seems the most straightforward way to do this is to manufacture a PR that contains all of the code you want to review.

In essence we create a branch that contains the code we don't want to review (we'll call this `base`), and one that builds on top of this with the code we do want to review (we'll call this `review`).

First, check out the branch with the existing code and then create the review branch as an orphan branch. This separates the history from the branch the code exists on now.
> `git checkout --orphan review`

The existing files should now be in your staging area. Here is when you need to manage which files you want to review and which ones you don't. Any files that you want to review should be removed from this commit by unstaging and deleting them.

If you want to review all files:
* Remove everything:
> `git rm -r --cached *`
* Clean the repo to remove all the unwanted files from disk.
> `git clean -fdx`
* Make an empty commit:
> `git commit --allow-empty -m "Start of the review"`

If you only want to review some files:
* Remove the files you want to review
> `git rm --cached file-to-review`
* Commit the files you don't want to review with
> `git commit -m "Start of the review"`

Then create the `base` branch from the current point:
>`git branch base`

Then merge the existing files in, which should create a commit with only the files you want to review:
> `git merge existing-files-branch-name`.

Git may complain that there are unrelated histories, which you can ignore:
> `git merge existing-files-branch-name --allow-unrelated-histories`

Now the branches are finished and just need to be pushed to the Github server.
> `git push -u origin base:base`\
> `git push -u origin review`

Last, go to Github, create the PR from `review` -> `base` and add your comments to it!

# References
http://astrofrog.github.io/blog/2013/04/10/how-to-conduct-a-full-code-review-on-github/
https://thib.me/recipe-code-reviews-for-existing-code-with-github
https://stackoverflow.com/questions/53103737/how-to-request-a-code-review-on-a-committed-master-branch-within-github
