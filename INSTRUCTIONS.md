# Instructions

TODO: Automate pushing changes to public repo
...

This instructions file talks about adding/removing posts from the hugo site.

## How to add or remove posts from the site

It's quite easy ngl.

### To add a post

```bash
cd arshimtiazportfolio # The one with the hugo.toml file
hugo new posts/<name-of-your-post>.md
```

Then just go to `content/posts/<name-of-your-post>.md` and edit it.


Once done, you can test the updated site draft like this:

```bash
cd arshimtiazportfolio # the one with the hugo.toml file
hugo server
```

If you're satisfied with the site, then now it's time to upload it to the public git repo for it to get hosted.

```bash
cd arshimtiazportfolio # the one with the hugo.toml file
hugo -t LoveIt # Since that's the theme atm
```

This will build and generate static files for the site inside the arshimtiazportfolio/public directory, which is a submodule for `https://github.com/arshimtiaz/arshimtiaz.github.io`. This means whatever you put inside this goes to that repo and you must add, commmit and push separately.

```bash
cd arshimtiazportfolio/public
git status # check the status of the repo
git add .
git commit -m '<message>' # added post
git push origin main
```

Once this is done, your updated site has now been pushed to the public repo and is now being actively hosted on the site https://arshimtiaz.github.io

Well done!

### To remove a post

```bash
rm content/posts/<name-of-post>.md # delete the post file
# move to arshimtiazportfolio directory
hugo -t LoveIt # this will generate new static files with the post removed
cd arshimtiazportfolio/public
git status # check the status of the repo
git add .
git commit -m '<message>' # deleted post
git push origin main
```

Once this is done, your updated site will get pushed to the public repo with the post now removed.