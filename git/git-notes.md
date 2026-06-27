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
git commit -m "Add multiple training subdirectories and cheat sheet notes"


