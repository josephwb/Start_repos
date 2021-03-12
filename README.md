Some Tips To Ease Setting Up Version Control
--------------
A Mac-oriented alternative is here: http://rogerdudler.github.com/git-guide/
### Get Git And Configure (Do Once)
If you do not have git (needed for both BitBucket and GitHub) installed, go here:

http://git-scm.com/downloads

on \*nix, git can be obtained with:

	sudo apt-get install git

There are GUI clients available, but we're beyond that, right?

If this is your first time using git, you will need to configure it. In a Terminal, type:

	git config --global user.name "first last"
	git config --global user.email your_email@youremail.com

Set up git to ignore annoying files that you do not care about, or that have different values across OSs. Type:

	vi ~/.gitignore_global

and paste in the following (you may want to add in your own):

	# Compiled source
	*.com
	*.class
	*.dll
	*.exe
	*.o
	*.so
	*.pyc
	# Packages/compressed files. Can take up a LOT of space, as every version is retained.
	*.7z
	*.dmg
	*.gz
	*.iso
	*.jar
	*.rar
	*.tar
	*.zip
	# Logs and databases
	*.log
	*.sql
	*.sqlite
	.Rapp.history
	.Rhistory
	# IDE-specific files (e.g. Eclipse) - especially important across OSs
	*.cproject
	*.project
	# OS-generated files
	.DS_Store
	.DS_Store?
	._*
	.Spotlight-V100
	.Trashes
	Icon?
	ehthumbs.db
	Thumbs.db
	*~

Now, inform git about these preferences. Type:

	git config --global core.excludesfile ~/.gitignore_global

### Set Up A Public SSH Key (Do Once Per Computer)
Pushing code is to Bitbucket/GitHub is crazily simple. However, before doing that, we will set things up so you will not have to enter your password every time you want do something. To do this, we will create and upload a public ssh key.

Do NOT do this step if you already have set up ssh-keys, e.g. if you did this previously for GitHub (an ssh key can be reused, and this will overwrite any existing keys). In Terminal:

	ssh-keygen -t rsa -C "your_email@youremail.com"

Use the email associated with your Bitbucket/Github account. It will ask you for a passphrase. Just hit Enter (for empty passphrase). Your public key will be saved in ~/.ssh/id_rsa.pub. 

### Set Up BitBucket Account (Do Once)
Go to:

http://bitbucket.org

Sign up if need be. **Make sure to use an academic email address, as this gets you free unlimited space.**

Select "Manage Account" from your user menu. Then, select "SSH keys", and click on "Add key". It is important that the key is formatted exactly (e.g. does not contain spaces), so we need a dependable way to copy/paste. We'll use the clipboard.

For mac:

	pbcopy < ~/.ssh/id_rsa.pub

For linux:

	sudo apt-get install xclip
	xclip -sel clip < ~/.ssh/id_rsa.pub

In the Key window, paste in the key and click "Add key". Test this in Terminal:

	ssh -T git@bitbucket.org

You only need to do the steps above once. Any new repositories will use this key. Setting up a GitHub account is similar.

### Create BitBucket Repository
Now, to set up the repository. Click on "Repositories" at the top, and select "Create repository". Configure name, description, private/public, language, etc. Use Git as Repository type.

Done on the web. Switch over to a Terminal. cd into the folder containing your code. Type:

	git init
	git remote add origin ssh://git@bitbucket.org/YOUR_USER_NAME/YOUR_REPO_NAME.git

If you type:

	ls -la

You will see the hidden directory `.git`. Wee!

### Pushing Local Code To The Remote Repository
The first step is to add local files to the staging area: this essentially marks files to be uploaded ('pushed'). Git does neat things like keeping track of file _differences_ rather than entire files, but we do not have to worry about that here.

To add files to be pushed:

	git add .

"." adds _everything_ locally that differs from the remote repository. You probably do not want do this if you have a bunch of temporary files (e.g., test data) in the repository that you do not want to share. You only typically use this if 1) you are starting from scratch and want to push _everything_, or 2) you keep your repo directory immaculately clean.

To restrict it to file "foo.txt":

	git add foo.txt

You can also restrict things to only *edited* files: those that are already part of the repository (i.e., were added previously, but have since changed). This will skip any "new" files. This is the most common way I work.

	git add -u

You are now ready to 'commit' the marked files. Each commit requires a brief message describing the edits made. Make this message as descriptive as possible, as this will assit later if one neds to search through past changes.

	git commit -m "some message about the commit"

Finally, we can push to the remote repository:

	git push origin master

There are, of course, a lot of other commands, but these are the ones you will use often. There are tons of git cheatsheets online.

### Multiple Computers
You might want to use multiple computers for code development (say, work and home). You will therefore want to make sure code is synced across your machines. This also applies when collaborating with other people. The simplest way to do this is to commit all changes when leaving one machine:

	git add .
	git commit -m "latest version"
	git push origin master

and pull that most recent copy onto your other machine:

	git pull origin master

Note that this will try to merge any differences between the remote and local copies. To ignore local changes and replace local code with the remote version, use:

	git stash

Otherwise, putting your code in a DropBox folder or equivalent works without push/pulling. Note, however, that this works best when computers share an OS. Otherwise, IDE-specific files will cause headaches.

### Issues
Sometimes when cloning a repository, git is configured to use https rather than ssh URLs. This means that it will not use your public ssh key, and so you will need to type in your password every time you push. To remedy this, go to the .git directory in your code folder "foo":

	cd foo/.git
	vi config

You will see something like:

	[remote "origin"]
		url = https://YOUR_USER_NAME@bitbucket.org/YOUR_USER_NAME/YOUR_REPO_NAME.git

Edit this to:

	[remote "origin"]
		#	url = https://YOUR_USER_NAME@bitbucket.org/YOUR_USER_NAME/YOUR_REPO_NAME.git
		url = git@bitbucket.org:YOUR_USER_NAME/YOUR_REPO_NAME.git

Pushing will now not require a password.
