Squash all commits to one:
git checkout --orphan new-master master
git commit -m "Enter commit message for your new initial commit"

# Overwrite the old master branch reference with the new one
git branch -M new-master master
