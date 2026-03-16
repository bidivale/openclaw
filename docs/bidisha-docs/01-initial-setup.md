# Initial Setup

## Forked the repository from github - https://github.com/openclaw/openclaw

## Cloned my forked version to local

## Added the original github openclaw project as upstream so that I can pull from it when needed
git remote add upstream https://github.com/openclaw/openclaw


## check the connections 
git remote -v


## When I need to pull/sync the changes from original project in future
git fetch upstream
git checkout main
git merge upstream/main -X ours


-X ours argument keeps our changes when in conflict in the same file same area.
