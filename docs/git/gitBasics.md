## Removing a file from tracking
1. Update your .gitignore file – for instance, add a folder you don't want to track to .gitignore .
1. git rm -r --cached folder (or files) – Remove all tracked files, including wanted and unwanted. Your code will be safe as long as you have saved locally.
1. git add . – All files will be added back in, except those in .gitignore .

Note that on the next pull it will delete said folder or files. To work around this, make a backup copy of these file and then after executing the above steps do a pull and then restore the folder/files.