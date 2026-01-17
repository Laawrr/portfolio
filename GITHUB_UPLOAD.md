# How to Upload Portfolio to GitHub

## Step 1: Create GitHub Repository
1. Go to https://github.com and sign in
2. Click the "+" icon → "New repository"
3. Name it (e.g., "my-portfolio")
4. Choose Public or Private
5. **Don't** check "Initialize with README"
6. Click "Create repository"

## Step 2: Run These Commands

Open PowerShell in your project folder and run:

```powershell
# Initialize git repository
git init

# Add all files
git add .

# Create first commit
git commit -m "Initial commit: Portfolio website"

# Add your GitHub repository URL (replace YOUR_USERNAME and REPO_NAME)
git remote add origin https://github.com/YOUR_USERNAME/REPO_NAME.git

# Rename branch to main (if needed)
git branch -M main

# Push to GitHub
git push -u origin main
```

## Step 3: If You Need to Update Later

```powershell
git add .
git commit -m "Your commit message"
git push
```

## Notes:
- Replace `YOUR_USERNAME` with your GitHub username
- Replace `REPO_NAME` with your repository name
- You'll be prompted for your GitHub username and password/token

