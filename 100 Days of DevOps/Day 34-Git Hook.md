The Nautilus application development team was working on a git repository /opt/apps.git which is cloned under /usr/src/kodekloudrepos directory present on Storage server in Stratos DC. The team want to setup a hook on this repository, please find below more details:



Merge the feature branch into the master branch, but before pushing your changes complete below point.

Create a post-update hook in this git repository so that whenever any changes are pushed to the master branch, it creates a release tag with name release-2023-06-15, where 2023-06-15 is supposed to be the current date. For example if today is 20th June, 2023 then the release tag must be release-2023-06-20. Make sure you test the hook at least once and create a release tag for today's release.
```bash
# Step 1: Navigate to the git repository hooks directory
cd /opt/apps.git/hooks
```
```bash
# Step 2: Create a new post-update hook file
nano post-update
```
```bash
# Step 3: Add the following script to the post-update file
#!/bin/bash
if [ "$1" == "refs/heads/master" ]; then
    DATE=$(date +%Y-%m-%d)
    TAG="release-$DATE"
    git tag $TAG
    git push origin $TAG
fi
    ```
    ```bash
# Step 4: Save and exit the file
# (In nano, press CTRL+X, then Y, then ENTER)
```
```bash
# Step 5: Make the post-update hook executable
chmod +x post-update
```
```bash
# Step 6: Test the post-update hook by merging a feature branch into the master branch and pushing the changes
cd /usr/src/kodekloudrepos
git checkout master
git merge feature-branch
git push origin master
```
```bash
# Step 7: Verify that the release tag has been created
git ls-remote --tags origin
```
```bash
# Note: Ensure that the server's date is correctly set to avoid incorrect tag names.
```
```bash
# Additional Note: You can check the git logs to verify that the tag was created successfully.
```
```bash
