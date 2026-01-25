The xFusionCorp development team added updates to the project that is maintained under **/opt/news.git** repo and cloned under **/usr/src/kodekloudrepos/news**. Recently some changes were made on Git server that is hosted on Storage server in Stratos DC. The DevOps team added some new Git remotes, so we need to update remote on **/usr/src/kodekloudrepos/news repository** as per details mentioned below:


1. In **/usr/src/kodekloudrepos/news** repo add a new remote **dev_news** and point it to **/opt/xfusioncorp_news.git** repository.


2. There is a file **/tmp/index.html** on same server; copy this file to the repo and add/commit to **master** branch.


3. Finally push **master** branch to this new remote origin.

```bash
# Navigate to the repository
cd /usr/src/kodekloudrepos/news
# Step 1: Add new remote 'dev_news'
git remote add dev_news /opt/xfusioncorp_news.git
# Verify the new remote has been added
git remote -v
# Step 2: Copy the /tmp/index.html file to the repository
cp /tmp/index.html .
# Add the file to staging area
git add index.html
# Commit the changes to master branch
git commit -m "Add index.html file"
# Step 3: Push the master branch to the new remote 'dev_news'
git push dev_news master

```