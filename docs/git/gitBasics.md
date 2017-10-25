## Basic Git Commands

- **git status** - shows the status of your repository.  Things like changed files, staged files, and if you are out of sync with the remote
- **git add .** OR **git add -A** - This command will stage for commit all updated and new, untracked files.  
- **git add -u** - This will add all files that have been updated. The -u specifies to get all updated files. Similar to the *add .*, but will not get new, untracked files.
- **git diff HEAD~1..HEAD** - diff will allow you to see the difference between two hashes, but easier than specifying hashes is to use the HEAD (current) and HEAD~1 (one commit behind the head).
- **git checkout *filename*** - will checkout the *filename* from the HEAD
- **git reset --hard** - will remove and changes and reset your repo back to the HEAD revision.
  - You can also specify a location in the repo.  **git reset --hard HEAD~1**.  Will reset to the previous commit
  - There is also a *--soft* option.  The --hard option will remove all changes.  The --soft option will restage the last commits before you committed giving you a chance to change how you commit.
- **git clean** - Will "clean" your working copy.  It will look at any new files that have not been staged or committed and will remove them.  
  - You will need to use the *-f* flag to actually run the action.  You can use the *-n* option to first see what the -f would do.
- **git log --oneline** - shows log of actions.  the oneline is optional, but makes reading easier.
- **git log --graph --oneline --all --decorate** - view of commits and branches, etc.
- **git remote add remotename remotelocation** - a remote is where the repository is located and shared from.  I'm using github.com.  You can have multiple remotes, but at this point, for small project without others working, won't be needed.  
  ```git remote add origin https://github.com/markmccoid/QlikVariableEditor.git```

## Fetch, Pull, Push
*git fetch* will pull down changes but will not merge them.  You would normally do a *git merge* after the fetch.
*git pull* is a fetch/merge combo command.
*git push* will push your changes to the remote.

## Git Tags
Tags allow you to mark specific commits with a tag.
There are three types of tags, *lightweight*, *annotated*, *annotated w/signing*

Annotated tags are recommended as they are stored as full objects in the Git database. They’re checksummed; contain the tagger name, email, and date; have a tagging message; and can be signed and verified with GNU Privacy Guard (GPG). It’s generally recommended that you create annotated tags so you can have all this information

```git
$ git tag -a v1.0 -m "version 1.0 of this thing"
```
**NOTE** - the *git push* will **NOT** transfer tags to remote servers, you must do this explicitly:

```git
$ git push origin v1.0
//OR Push all tags
$ git push origin --tags
```

**NOTE** that you cannot really checkout a tag, but you can create a branch from a tag:

`git checkout -b [branchname] [tagname]`

```git
$ git checkout -b tagVerBranch v1.0
```

### Tag a previous commit
If you want to tag a previous commit, you will first need to find the Sha hash and then use this for the tag:

```git
$ git log --pretty=oneline //--This command will show the full sha hash (don't really need it)
$ git log --oneline //--this will just show the digits of the sha hash that you need

$ git tag -a v1.2 9fceb02
```

## Branches
Adding branches is easy, working with and understanding, a bit more difficult.

To add a branch: `git branch feature1`
Then you must checkout the created branch: `git checkout feature1`
If you want to combine the steps above, meaning create a new branch and checkout all at once, you may:

```
$ git checkout -b feature1
```

Now, any changes you make will be on the *feature1* branch.

To rename a branch: `git branch -m feature1 branch1`

To delete a branch: `git branch -d branch1`
The above will fail if the branch has not been merged yet.  If you truly do not want to merge and you want to get rid of the branch use: `git branch -D branch1`

## Merging branches
`git merge branchName`

This will merge the *master* with *branchName*.

You can then delete the branch: `git branch -d branchName`

## Stashing changes
Before using the below, probably a good idea to read up more on stashing.  Seems like more going on.

The Stash command lets you save changes without committing them.  Once it "stashes" your changes, it will rollback your commit so that you are where you were before your stashed changes.

`git stash`

When you are ready to apply your stashed changes: `git stash apply`  
This however, will leave the stashed item.  You can use: `git stash pop` to pop off the first item stashed and apply it.  

You can view anything that has been stashed using: `git stash list`

## Removing a file from tracking
1. Update your .gitignore file – for instance, add a folder you don't want to track to .gitignore .
2. git rm -r --cached folder (or files) – Remove all tracked files, including wanted and unwanted. Your code will be safe as long as you have saved locally.
3. git add . – All files will be added back in, except those in .gitignore .

Note that on the next pull it will delete said folder or files. To work around this, make a backup copy of these file and then after executing the above steps do a pull and then restore the folder/files.

## Add an Alias for Common Commands
If you run a long command often, you can add an alias to the git config file that will run the command with a shorter alias that you define.

```git
$ git config --global alias.lga "log --graph --oneline --all --decorate"
```

This will create an alias call *lga* that will run the command in the quotes:

```git
$ git lga
```

This will create an entry in the *.gitconfig* file's [alias] section.  Usually located in the *"Users\yourUserName* directory.
```git
[alias]
	lga = log --graph --oneline --all --decorate
```
## SSH Setup

First, check to see if you have any SSH keys setup.  You **MUST** use git bash or other terminal (cmder, etc) that can run unix commands.

This command will show you if you have ssh setup.  

```
> ls -a ~/.ssh
```

If you don't have it setup, do the following. [Connecting To GitHub with SSH](https://help.github.com/articles/connecting-to-github-with-ssh/)

```
// -t is the type of ssh (rsa is very common)
// -b is the byte size of key (4096 is recommended)
// -C is the comment, put your email here
> ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

This command will prompt you for the filename, just take the default **id_rsa**.

It will then prompt for a passcode, do not enter one.

Next you must add your SSH key to the ssh-agent

```
> eval "$(ssh-agent -s)"
//Now add your key to the ssh agent
> ssh-add ~/.ssh/id_rsa
```

**Copy your public key to the clipboard**

```
//Windows
> clip < ~/.ssh/id_rsa.pub
//Mac
> pbcopy < ~/.ssh/id_rsa.pub
```

**Add To Your GitHub Account**

Now you need to add this public key to your GitHub account.

Go to your GitHub account page and click on the "SSH and GPG Keys" section and click "New SSH key".

```
//command to check if working
> ssh -T git@github.com
```



