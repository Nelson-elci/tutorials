###Divergent Branches
Two branches are divergent when they share a common ancestor commit but have each made different commits afterward.

Diagram 1

A ______ B (main)
         \ 
           (feature) 
          
Diagram 2
          
A ______ B ______ C ______ (main)
         \
          D ______ E ______ (feature)


In other words:
They started at the same point, then each branch went its own way with separate changes as shown in diagram 2. Only the main branch has commit C, and only the feature branch has commits D and E 

In Diagram 1 above
at the point where feature branch was created, main
had only commits A and B and so feature will also 
have only commits A and B. 




#### 1. Fast-forward Merge
This happens when there are no new commits on the base branch (main) since the feature branch was created. The main pointer can simply be moved forward to the tip of the feature branch.


A ______ B  (main)
         \
          D ______ E ______ (feature)
                     |
                     (After `git merge feature` on main)
                     ˅
 
A ______ B ______ C ______ D ______ E ______ (main, feature)
main (after fast-forward)


Explanation:
The history is linear. Git "fast-forwards" the main pointer to match the commit E. No new merge commit is created. This is only possible if the branches have not diverged.


---


#### 2. Merge Commit
This is the standard way to merge when the branches have diverged (i.e., there are new commits on both main and feature). It creates a new "merge commit" that has two parents, tying the two histories together.

A ______ B ______ C ______ F (main)
         \                 /
          D ______ E ______ (feature)
          

Explanation:
A new commit F is created on the main branch. This commit F has two parents: commit C (from main) and commit E (from feature). The history graph now shows a clear fork and join, preserving the exact context of both development lines.


---


#### 3. Rebase
Rebase rewrites the history of the feature branch. It takes the commits from feature and replays them on top of the current tip of main. The result is a perfectly linear history.


Before Rebase:

A ______ B ______ C ______ (main)
         \
          D ______ E ______ (feature)
          
          
 
 After git rebase main while on feature:   
 
 A ______ B ______ C ______ (main)
                     \
                      D'______ E'______ (feature) 
                          
                      
 
 Final State after a Fast-forward merge:
 A ______ B ______ C ______ D'______ E'______ (main)
 
 
 
