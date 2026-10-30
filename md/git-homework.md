# Git Cheatsheet: Merge Strategies, Branching, Time Travel & Rebase vs Merge

A quick-reference guide for everyday Git work.
နေ.တဓူ၀ အသုံးများသော Git လုပ်နည်းများ

## Table of Contents

1. [Git Merge Strategies](#1-git-merge-strategies)
2. [Git Rebase Vs Merge](#2-rebase-and-merge-travel-cheatsheet)
3. [Git Branching Cheatsheet](#3-git-branching-cheatsheet)
4. [Git Time Travel Cheatsheet](#4-git-time-travel-cheatsheet)

---
## 1. Git Merge Strategies

#### 1.1 Fast-forward merge

* target branch တွေမခွဲထွက်နေတဲ.အခါ main မှာ commit အသစ်မရှိနေသေးတာကြောင်. ွGit က main ကို pointer forward ပေးလုပ်တယ်။ Linear branching ဖြစ်ပီး merge commit မရှိပါ။ branch ခွဲထွက်တာမရှိတဲ. short-live branch မှာ ဒီနည်းသုံးတယ်။ linear histories

Command:  
git switch main  
git merge feature

Visual : 
```
Before:  main:    A---B
                       \
         feature:       C---D

After:   main:    A---B---C---D
```

#### 1.2 Three-way merge (git merge commit feature)

* main မှာ တခြား commit ကိုရွေ.နေတာကြောင်. ွbranches diverged ဖြစ်နေတယ်။ုဒ်ေကြင်.merge commit  လုပ်ဖို.လိုတယ်။ branches နှစ်ခုလုံးက commitအသစ်တွေကို merge လုပ်တဲ.အခါမှာ feature branch ရဲ.histories တွေပါ ပေါင်းထည်.ပေးတယ်။ Normal collaborationမှာသုံးတယ်။ 

Command:  
git switch main  
git add .  
git commit -m "E: hotfix on main"  
git merge feature -m "M: merge feature into main"  

```
Before:  main:    A---B---E
                       \
         feature:       C---D

After:   A---B---E-------M
              \         /
               C-------D
```

#### 1.3 Squash merge (git merge --squash feature)
* features commit များတာ တွေကို parent ဆီ မပေါင်း၀ူး ၊ target branch ဆီ commitအသစ်ကိုလုပ်တယ်။  history Clean ဖြစ်တယ် main history တိတိကျကျ သိမ်းထားတယ်,  

Command:  
git switch main  
git merge --squash feature  
git commit -m 'S: add feature (squashed C + D)'  

```
Before:  main:    A---B---E
                       \
         feature:       C---D

After:   main:    A---B---E---S      (S = C + D combined, no link to feature)
```

#### 1.4 Rebase then fast-forward
* latest main ရဲ. အပေါမှာ feature ကို ထားတယ်။ linear commit historyရှိတယ်။

Command:  
git switch feature  
git rebase main  
git switch main  
git merge --ff-only feature  

## 2. Rebase and merge Cheatsheet

* Rebase က feature branch ရဲ.  changes တွေမတိုင်ခင် အခြေနေမှာ main ဘက်ကအသစ်ရှိလာတဲ. commit တွေကို အရင်ထည်.ပီးမှ feature က ရှေ.ဆက် commit အသစ် ဆက်ဆောက်တယ်။ commit ID ပြောင်းသွားတယ်။

* Merge က နှစ်ဖက်လုံးရဲ. branches တွေကိုပေါင်း commit လုပ်ပီး History နှစ်ခုလုံးကိုဆက်သိမ်းထားတယ်။ commit IDs တွေ တူတူဖြစ်နေအုံးမယ်။

## 3. Git Branching Cheatsheet

```bash
Command                          # Action (အလုပ်)
git branch                       # list local branches ရှိတဲ. စာရင်းပြ
git branch -a                    # list local + remote branches
git branch feature/login         # Create branch
git switch -c feature/login      # create branch and switch
git switch main                  # switch branch
git branch -m new-name           # Rename current branch
git branch -d feature/login      # Delete merged branch
git branch -D feature/login      # Force-delete local branch
git push -u origin feature/login # Push new branch
git push origin --delete feature/login  # Delete remote branch
git diff main..feature/login     # Compare branches
git log --oneline --graph --all --decorate  # view branch history
```
## 4. Git time travel Cheatsheet
```bash
Action                           # Purpose                                     Change history
git log --oneline	             # View previous commits	                   # No 
git show abc123	                 # Inspect an old commit	                   # No 
git switch --detach abc123	     # Visit a previous commit	                   # No 
git restore app.cs	             # Discard unstaged file changes	           # No 
git restore --source=abc123 app.cs	# Restore a file from an old commit	       # No
git reset --soft HEAD~1	         # Undo commit, keep changes staged	           # Yes
git reset --mixed HEAD~1	     # Undo commit, keep changes unstaged	       # Yes
git reset --hard HEAD~1	         # Undo commit and discard working changes	   # Yes
git revert abc123	             # Reverse a commit with a new commit	       # No, appends history
git reflog	                     # Find previous HEAD positions                # No
git cherry-pick abc123	         #Apply an existing commit's changes to the current branch	    # Appends commit
```
