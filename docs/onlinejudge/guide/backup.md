# Data backup and recovery

## How to backup

To ensure data security, please back up regularly.

The `data` folder in the OnlineJudgeDeploy directory contains all the data of the system, including logs, databases, test cases, uploaded files, etc. The data that needs to be backed up are `backend/public` and `backend/test_case` two directories.

**For databases, please do not use the method of copying database data files**. In the latest OnlineJudgeDeploy, the `backup` directory provides a database export sql file backup script. Please check the size and content of the generated sql file after each backup to ensure that the backup is successful.

Please do not put the backup data and OnlineJudge system on the same machine, so the risk of data loss is still high.

## Restore backup

If you just want to migrate and deploy between different machines, `docker stop $(docker ps -aq)` then copy the `OnlineJudgeDeploy` folder to the new machine and re-`docker-compose up -d`.

If you want to restore data, you must first ensure that a set of OnlineJudge has been newly deployed, and then you need to restore the data and test case files.

The test cases are stored in the `data/backend/test_case` folder and can be overwritten.

Perform the following operations on the new machine to restore the database

 -`docker cp db_backup_xxxxxxx.sql oj-postgres:/root`
 -`docker exec -it oj-postgres bash`
 -`psql -U postgres` and run `drop database onlinejudge;` (**Please pay attention!!! See which machine you are on**)
 -`\q` exit, then `psql -f /root/db_backup_xxxxxxx.sql -U postgres`
