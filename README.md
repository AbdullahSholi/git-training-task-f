# git-training-task-f
## Solution 1 with pre-commit hook framework : List of commands 
* `echo "# git-training-task-f" >> README.md`  
* `git add README.md`  
* `git commit -m "first commit"`  
* `git branch -m main`  
* `git remote add origin https://github.com/AbdullahSholi/git-training-task-f.git`  
* `git push -u origin main`  
* `git checkout -b feat/remove-trailing-whitespaces-task`  
* `touch task_file.txt `  
* `git add task_file.txt`  
* `git commit -m "First solution with using pre-commit framework"`  


### Install pre-commit framework 
* `python3 -m venv myenv` --> Create Virtual Environment, because If we install or update packages globally using pip, you might break essential system tools. -->

### Activate virtual environment 
* `source myenv/bin/activate`  

### Install pre-commit framework 
* `pip install pre-commit`  

### Check if pre-commit installation 
* `pre-commit --version`  

### Create .gitignore file 
* `touch .gitignore`  

### Add /myenv directory to .gitignore 
* `echo "/myenv" >> .gitignore`

### Stage & Commit
* `git add .gitignore`  
* `git commit -m "Add Virtual Environment directory to .gitignore"`  

### Create .pre-commit-config.yaml file & fill with content for remove trailing white spaces 
* `touch .pre-commit-config.yaml`  
* `nano .pre-commit-config.yaml`

### Add content & save 
```
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v2.3.0
    hooks:
    -   id: check-yaml
    -   id: end-of-file-fixer
    -   id: trailing-whitespace
-   repo: https://github.com/psf/black
    rev: 22.10.0
    hooks:
    -   id: black
``` 

### Add some text inside task_file.txt with some trailing white spaces
* `nano task_file.txt`  
* `git add task_file.txt .pre-commit-config.yaml`  


### Then run this command at terminal
* `pre-commit`

### The result will be Failed for Trim Trailing Whitespace & pre-commit framework will edit our file ( remove trailing whitespaces ) and pass code again to Working Directory ( as Modified file ), So stage the files again, then try run pre-commit again 

* `git add .`  
* `pre-commit`  

### We note that all case passed successfully & Now commit all 
* `git commit -m "Complete task successfully!"`  


---
## Another solution 

### Navigate to hooks directory
* `cd .git/hooks`  

* `nano pre-commit`  

### Add this content & save file 
```
#!/bin/bash

echo "🔍 Checking if all committed files aren't have any trailing white spaces..."

# Get the list of files staged for commit
FILES=$(git diff --cached --name-only --diff-filter=ACM)

for FILE in $FILES; do
  # Check if the file is a text file (to avoid binary files)
  if file "$FILE" | grep -q "text"; then
    # Check if any trailing white spaces exists in the file
    if grep -Pq '\s+$' "$FILE"; then
      echo "❌ Error: File '$FILE' contains Trailing White Spaces."
      exit 1
    fi
  fi
done

echo "✅ All files aren't have any trailing whitespaces. Proceeding with commit."
exit 0
```

### Now when we try to commit any file, the file must has no any trailing whitespace to approve the commit 
