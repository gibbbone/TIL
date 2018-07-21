# Git and Github tips

I tend to prefer command line workaround.

## Create a repo from command line 
### Create
First you need to create a repo on Github. This is not possible via git command and hence we need a method.

- The long way: [use Github API](https://stackoverflow.com/a/10325316/6332373). (substitute all caps words)

```bash
curl -u 'USER' https://api.github.com/user/repos -d '{"name":"REPO"}'
# Remember replace USER with your username and REPO with your repository/application name!
```
More info [here](https://developer.github.com/v3/repos/#input).

- The short way: use hub.

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
git remote add origin https://github.com/USER/REPOSITORY.git
# push upstream
git push -u origin master
```