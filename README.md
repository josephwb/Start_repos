Tips On Setting Up Version Control
--------------
### Get Git And Configure (Do Once)
If you do not have git (needed for both BitBucket and GutHub) installed, go here:

http://git-scm.com/downloads

There are GUI clients available, but we're beyond that, right?

If this is your first time using git, you will need to configure it. In a Terminal, type:

	git config --global user.name "first last"
	git config --global user.email your_email@youremail.com

Set up git to ignore files that you do not care about. Type:

	vi ~/.gitignore_global

and paste in the following (you may want to add in your own):

	# Compiled source
	*.com
	*.class
	*.dll
	*.exe
	*.o
	*.so
	# Packages/compressed files. This can take up a lot of space.
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

Sign up if need be. *Make sure to use an academic email address, as this entails free unlimited space.*

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

You will see the hidden directory .git. Wee!

### Pushing Code
Add files to be pushed:

	git add .

"." adds everything that differs from the repository. To restrict it to file "foo.txt":

	git add foo.txt

Commit files:

	git commit -m "some message about the commit"

Push already:

	git push origin master

There are, of course, a lot of other commands, but these are the ones you will use often. There are tons of git cheatsheets online.

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