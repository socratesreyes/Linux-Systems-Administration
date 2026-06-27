####################

#error  in push 


! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/socratesreyes/Linux-Systems-Administration.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.


SOLUTION 
#git push -u origin main --force 			this is not safe
SAFE WAY BEFORE PULL
This pulls the remote changes and places your local commits on top of them, keeping the history clean and linear.

# 1. Save any uncommitted work you have (just in case)
git stash

# 2. Pull the latest changes and put your work on top (--rebase)
git pull --rebase origin main

# 3. If step 2 works without errors, put your saved work back
git stash pop

# 4. Now push your changes
git push origin main

#sample good


Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Compressing objects: 100% (8/8), done.
Writing objects: 100% (12/12), 2.28 KiB | 129.00 KiB/s, done.
Total 12 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/socratesreyes/Linux-Systems-Administration.git
 + 915678b...4b49fc3 main -> main (forced update)
branch 'main' set up to track 'origin/main'.
