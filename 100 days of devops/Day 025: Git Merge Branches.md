1. Go to the repository
cd /usr/src/kodekloudrepos/ecommerce


Confirm you are on master:

git branch


If not:

git checkout master

2. Create a new branch datacenter from master
git checkout -b datacenter

3. Copy the file into the repository

The file already exists on the storage server.

cp /tmp/index.html .


Verify:

ls

4. Add and commit the file in datacenter branch
git add index.html
git commit -m "Add index.html to datacenter branch"

5. Merge datacenter branch into master

Switch back to master:

git checkout master


Merge:

git merge datacenter

6. Push both branches to origin

Push master:

git push origin master


Push datacenter:

git push origin datacenter

7. Final verification (optional but safe)
git log --oneline --decorate --all
