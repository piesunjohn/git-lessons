## A repository

* A repository is where we store the files and their versions
* It has a few layers.
* Can do a few things:
  - Tell repository we've created files
  - Tell repo to get those files and create a new version -- save who's created the version and saved it
  - Tell it to send those changes to the cloud/remote repository

### Layers 

  
  Files pushed to cloud >> push
  Files commited to local repository >> commit
  Files added to next commit (STAGING AREA) >> add 
  Files in your system

### Setting up Repository

* Initially, Git knows nothing about our files. Is is as if Git wasn't installed.
* We need to "install" Git into the folder that we are going to use it in.
* "Installing" Git is called *initialising*. We do this with `git init`

* After initializing, we can add files to next commit (Staging Area), then Commit files to local repository, and then push those files to cloud.

* If you `rm -rf .git` in a directory that already has the .git file, you are de-initializing it, so you'd need to reinitialize again.


### Adding files to the Staging Area

* What is a Diff?
 - The difference between one file and another is a diff
 - Let's say we have a file called file1 and an identical file called file2.
 - If we modify file2 and then create a diff of file1 and file2 we get something like this:

If file1 has:
```
Hello, world!
This is a test.
```

and file 2 has:
````
Hello, world! We are testing a change.
```

You can test out the difference using `diff file1 file2 -u`(The -u flag makes a more readable output)

* When we initialise a local repository with git init, Git creates a hidden folder. In this folder, we store the diffs (and other things). Adding files to the staging area is just telling git to get ready. The files in the staging area will be used to create diffs when we commit! We add files to the staging area with `git add <file>`

* You use `git rm --cached <file>` to unstage.


### Committing to repository

First understand how Git stores commits:
* First time you committed something, that is the original file and that was a diff that was stored (can be empty file to the first file), and then those changes are stored as diff files.
* If you create a new file, you can create a new diff from the latest diff and the new file, and that diff can be placed into the previous stack. Thus,
* You can commit *one file* (which addds one diff to the repo), or *many files* (which adds one diff per file)
* All the diffs you add at the same time is one commit.

* When we have added all the files, we can commit them. This creates the diffs for each and stores them in the .git folder. We do this with `git commit`.

*Commit Messages* You need to add commit messages so taht when other people read our commit, they know what we did at that point in time. There are two ways:

 - With the command `git commit -m "message"`
 - Using a text editor - nice when you're adding a large message | vim appears when you commit with just `git commit
 - set text editor with `git config --global core.editor <texteditorname>`


### WRiting good commit messages

Bad commit messages:
* Commit 4
* Fixed bug (what bug?)
* Updated <something>`(why/) 
* Added functionality to do <something>

*All these above do not provide enough information and do not help developers know exactly what you did. They do not link to the project directly (whenever you add #number associated with a project, github will link to that issue)*


Good commit messages:
* `Fixed issue #22 by updating sample.py to include functionality to do <something>. -modified: sample.py -fix: issue #22

```
<type of change> <short description>
(Optional)
Issue: #<issue number> <short description of issue>
Modified/Fixed/Added: <file> <what does this file do>
```

### Adding Remotes

A remote is a copy of your Git repo that isn't local to your computer (i.e. hosting it on Github)

* You can have many remotes for your repo
* Need to add a remote before being able to use it
* The command is `git remote add origin <remote service url>`
 - origin is the most common name if you have one remote repo but if you have more than one, you'll need to name it to something else.

After adding a remote you can start sending your changes to the mote, or bringing in changes from it. 

Instead of initializing a git repo locally, creating a github project, and then adding that proj as a remote, you can do it another way around: first create github project, and then clone into computer. If you clone a github project, it gets automatically added as a remote.

* To clone a remote, create the github project before initialising a repo locally. Then `git clone <remote url>`. This will bring in all the commits and files from the Git project cloned.

As a conclusion, to add a remote, we can either`git init` a repo locally and then `git remote add <name> <url>` or we can create the project in github and then `git clone <url>`



### Push and Pull from remote repos

#### Pushing
Need to do this if you want your work to be available to other people in the same project

Command is `git push`. This copies all of the new commits into the cloud repo. 

#### Pulling
Three commands:
* `git pull` - good as long as we are working with only one branch. This gets the new commits (or changes to previous commits) and *merges* them with our local repository.
* `git fetch` - same as a pull, but *doesn't merge* the changes into your local repo, so it just gets the commits from the cloud and stores them in the .git folder. You can then merge all of them or some of them. You can even see how the new commits are going to affect the local repo if you merge them. Using git fetch and git merge is recommended over pull in most cases.

The problem with pull comes when using multiple *branches* (will be further explained in the next sections)

If you run `git branch`, you might get something like:
> * master
This would be the local branch and the asterisk shows that you're working on the local branch so if you make any changes or do any commits, it'll affect the master branch. So if you want to merge any changes INTO the master branch, you'd do that with `git merge <where you want to merge from, i.e. origin/master>`

`git push origin` means you're pushing *to* the remote repo named origin


#### Branches
Before describing a branch, let's describe a workflow:
* Initially, we have no commits and nothing added to lour local repo
* Then we *add* some files, and then *commit* them
* Somewhere inside the .git folder, the commit gets stored (along with the diffs and what files were modified, by whom, etc.)

So now we have the commit:
* The commit is stored inside the .git folder, inside a file which has:
 - a. The commit ID
 - b. What files are referenced
 - c. Who made the commit
 - d. When the commit was made
 - e. And more...

A new commit?
* If we make changes, and add them, and commit them, we don't get a new file. Instead, the new commit is also added onto the same file. So now the file that stored the original commit now references two different commits (and all their details).

This is what it looks like:
* When we make our first commit, it gets stored (C0)
* The next commit stored below C0 is what we call C1
* Then C2, C3, etc...

===========

    C0
    ||
    C1
    ||
    C2
    ||
    C3
    ||


===========

*This is called a branch* They're a bunch of commits! In the picture above, the newest commit is in C3 because it's the last commit. Thus, the branch is just really the last commit and all the commits above that are linked to it.

So when we name a branch, we're naming the last commit.


These are two branches(at C4 and C5):

==========

    C0
    || \\
    C1  C3
    ||  ||
    C2  C4 <---feature-1
    ||
    C5 <---master


=========
 > Master and feature-1 are branch names.


*Creating Branches*
* Always have one branch in the beginning, master
* We can create new branches with `git branch <branchname>`

For example,

  git commit C0          C0
                         ||
  git commit C1          C1
                         ||
  git commit C2   master C2 feature-1

  git branch feature-1 (creates feature-1 to the right of master at C2)

  git checkout feature-1


Once we commit:

  git commit C3
  git commit C4

     C0
     ||
     C1
     ||
     C2 <---master
      \\
       C3 <---feature-1
       ||
       C4 <---(still)feature-1


If we then do:

  git checkout master
  git commit C5

       C0
       ||
       C1
       ||
       C2 <---master
       || \\
master C5  C3 <---feature-1
           ||
           C4 <---(still)feature-1

>master moved to C5

So we can see what `checkout` does -- it allows us to move between branches. When we create a new branch we have to checkout onto the branch we want to add changes to.

`git checkout <branch_name>`

* We can create a branch but not add any commits to it
* In order to help us, *Git doesn't commit to the new branch automatically*
* Thus we `checkout` to any branch
* But we can also checkout to any commit we want (if we want to, for example, modify the commit)

For example, if we did git checkout C0 from:

  C0
  ||
  C1
  ||
  C2 <--- master

Then, C0 will become the master.

However, commits aren't called C0, C1, etc..., sadly, the commit names are usually things like fed2da64c0eaf5693610bdd892f82e58e8cbc5d8. Therefore we must use relative referencing in order to avoid having to remember these names.

Relative referencing is easy:
`git checkout master~2`

The ~2 would mean the branch from the current location of master would go up twice. Two commits above master. And this is called *detaching the head*. The head is where you are currently at. Thus, that command would *detach* the head at master C2 to C0.


So how can you move these branches to a *remote* directory?






