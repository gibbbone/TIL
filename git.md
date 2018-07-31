# Git and Github tips

Disclaimer: I tend to prefer command line workaround.

Index:

-  [Create a repo from command line ](https://github.com/giovannibonaccorsi/TIL/blob/master/git.md#create-a-repo-from-command-line)
- []() 


## Create a repo from command line 
### Create
First you need to create a repo on Github. This is not possible via git command and hence we need a method.

#### The long way: [use Github API (SO question)](https://stackoverflow.com/a/10325316/6332373). 


```bash
curl -u 'USER' https://api.github.com/user/repos -d '{"name":"REPO"}'
# Remember replace USER with your username and REPO with your repository/application name!
```
More info [on the Github API here](https://developer.github.com/v3/repos/#input). This is longer once you want to repeat it more than one time: in this case you need to alias the command in some ways.

This is a quick alias:
```bash
git config --global alias.gh-create '!sh -c "curl -u \"USERNAME\" https://api.github.com/user/repos -d \"{\\\"name\\\":\\\"$1\\\"}\"" -'
# Again remember to replace USER
```
These are full bash scripts to interactively provide parameters:
- [jerrykrinock gist](https://gist.github.com/jerrykrinock/6618003)
- [robwierzbowski gist](https://gist.github.com/robwierzbowski/5430952/)

#### The short way: use [hub](https://hub.github.com/).
This a wrapper around git syntax which aims to simplify greatly these operations. This is shorter once you need to do the operations often, then you can endure the effort of installing and tuning it (I didn't).

### Commit changes
Starting from folder on local machine

```bash
# initialize
git init
# first commit
touch README.md
git add README.md 
git commit -m 'Added readme'
# add remote (change all caps words to proper ones)
git remote add origin https://github.com/USER/REPO.git
# push upstream
git push -u origin master
```

### Things that went wrong
- I forgot to commit [(SO question)](https://stackoverflow.com/questions/4181861/src-refspec-master-does-not-match-any-when-pushing-commits-in-git), got the infamous `error: src refspec master does not match any.` message.
- I tried to use SSH instead of HTTPS[ (SO question)](https://stackoverflow.com/questions/12940626/github-error-message-permission-denied-publickey) to push and got `Permission denied (publickey).`
	- More about this in the [official Github documentation](https://help.github.com/articles/error-permission-denied-publickey/) 

## Get only git hashes of your commits 
```bash
git log | grep -o "\([a-z0-9]\{40\}\)"
```

## Ungit
The alias I use to stop git from monitoring a folder
```bash
alias ungit='rm -rf .git/'
```
