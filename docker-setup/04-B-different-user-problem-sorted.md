# One issue sorted with sitecloner project 
My sitecloner project has an sqlLite with it. 
When I ran it from default dokcer engine to clone a site - it got an error that it got only the read only access for the database - so it cannot write. 

## Resolve 
Why this happened: Docker Desktop ran containers as your user (skmindlab), so it could write files you own. The native engine runs containers as a different internal user.

Container runs as uid=999 (cloner) but the files are owned by uid=1000 (you). 

 giving 777 (world-writable) is not ideal. The right fix is to give ownership to uid 999.

 I ran the following command to fix the ownership :

 sudo chown -R 999:999 ~/proj/others/sitecloner/data/
sudo chown -R 999:999 ~/proj/others/sitecloner/output/


And it fixed
