push to GITHUB SEQUENCE
git add .
git status
git commit -m "added multiple notes and folders"
git push -u origin main

#if errors:
! [rejected]        main -> main (fetch first)
error: failed to push some refs

git pull --rebase origin main   # 1. Bring GitHub's changes down
git push origin main            # 2. Now push your work on top



############################
#find existing .git
find ~ -type d -name ".git" 2>/dev/null


##########################
#pull sequence
1st time 
git clone https://github.com/socratesreyes/Linux-Systems-Administration
## update local repo
git pull







BASIC-COMMANDS

Step 1: Edit your file       -->  (Open docker-notes.md, add your text, and save it)
Step 2: Stage the edit       -->  git add docker/docker-notes.md
Step 3: Save to history      -->  git commit -m "Update docker-notes with container commands"
###################################
SECURITY
cat > .gitignore << 'EOF'
# Ignore sensitive sysadmin credentials
*.pem
*.key
*.env
secrets/

# Ignore OS-specific system junk files
.DS_Store
Thumbs.db

########################################
COMMIT WHOLE FOLDER - SAVE LOCALLY

# 1. Sweep and stage absolutely every change, edit, and new folder at once
git add .

# 2. Visually verify that everything was caught (they will all look green)
git status

# 3. Permanently lock them into your local Git history database
git commit -m "adding notes"

#check 
git log


############################
git push -u origin main



##########
techniques- find .git to locate local repository
git remote -v 			#to verify location
git config --global credential.helper 'cache --timeout=3600'			#to avoid re entering of password. base on the sec.
	cache --timeout=3600	Every hour
	cache --timeout=86400	Once a day	

########################################
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
