# Manage Account

##Problem : You want to setup personal github in company laptop which already has some git setup for work purpose

##Source : 
1. [Mherman Blog](http://mherman.org/blog/2013/09/16/managing-multiple-github-accounts/#.WxsyIlMvwfE)
2. [Git Instruction](https://github.com/adarshhsingh/test-personal)

###Steps :

1. Create a SSH keys

```
$ cd ~/.ssh
$ ssh-keygen -t rsa -C "yourmail@gmail.com"
# save it as id_rsa_personal when prompted
```

2. The above commands setup the following files:

```
id_rsa_personal
id_rsa_personal.pub
```

3. Add the keys to your Github accounts:
	
	1. Copy the key to your clipboard:
	```
	$ pbcopy < ~/.ssh/id_rsa_personal.pub
	```

	2. Add the key to your account:
    	1. Go to Settings
        2. Click “SSH and GPG keys"
        3. Add New SSH Key
        4. Paste your key into the “Key” field and add a relevant title
		5. Click “Add key” then enter your Github password to confirm

4. Create a configuration file to manage the separate keys

	1. Create a config file in ~/.ssh/
	```
	$ touch config
	```
 	2. Edit the file using the text editor of your choice. I used vim - $ vim config:
	```
	# githubPersonal
	Host personal
	   HostName github.com
	   User git
	   IdentityFile ~/.ssh/id_rsa_personal
	```


5. Update stored identities
	1. Clear currently stored identities:
	```
	$ ssh-add -D
	```
	2. Add new keys:
	```
	$ ssh-add id_rsa_personal
	```
	3. Test to make sure new keys are stored:
	```
	$ ssh-add -l
	```

6. Test to make sure Github recognizes the keys:
```
$ ssh -T personal
Hi githubPersonal! You've successfully authenticated, but GitHub does not provide shell access.
```

7. On Github, create a new repo in your personal account, githubPersonal, called test-personal.
Back on your local machine, create a test directory:

```
cd ~/documents
mkdir test-personal
cd test-personal
echo "# test-personal" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/adarshhsingh/test-personal.git
git push -u origin master
```