Quick start with Git
----------------------

### To clone a branch:

```
git clone https://github.com/Appers-lab/repo-name.git

cd repo-name

git checkout <your-branch>
```

Or alternatively you can combine the above two command into one:

```
git clone -b <your-branch> https://github.com/Appers-lab/repo-name.git

cd repo-name
```

You may need to enter your username/password (your github account).

>>Very soon Github will no longer accept username/password ... you will need to use TOKEN!

Note that using `git checkout` you can switch between branches. Before doing project make sure you are in your branch. To double-check which branch you are in, from the project root run:

```
git branch
```

The above command shows a list of branches downloaded, and the current branch has a * beside it.


### Commit/Push changes
Once you made your changes you need to push the changes to the bitbucket repository. From the project root folder:

```
git add .
git commit -m "your commit message"
git push
```

Then using browser login to your bitbucket account, go to this repository section and create a "pull request". When you create a pull requet, I (and others) can see what changes you made in your branch and give you feedback. You may need to make additional changes and push again (using the commands above). When we approve your changes then we will "merge" your changes into the main branch, and basically your code will be incuded in the main version of the program.

### Pull changes - conflict resolution

To download the latest changes from the online repository (upstream repo) to your computer use the following command from the project root:

```
git pull
```

The above command is useful if you have an old clone of the repository and some changes are made to the online repo after your cloning. In this case use the above command instead of cloning a whole new project.

Sometimes it may so happen that while you are working on your branch we push/merge some new changes in the main branch. To get these changes in your branch you need to "pull" changes from the main branch to your branch:

```
// making sure you are in your branch
git checkout <your-branch>

git pull origin main

```

If conflicts happen, then the above command shows a list of conflicted files. These files will contain "conflict-markers". You need to open these files using some IDE editor (like VS code or WebStorm) and these editors will show you the conflicting parts and ask you to choose one of the possible options for each conflict (by clicking on buttons). Once you resolve these conflicts save your files. Then commit/push again to your branch (see the previous section) and the conflicts are resolved.
