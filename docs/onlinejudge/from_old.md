# Migrate from old version

## Migration instructions

Because the database structure of version 2.0 and the old version is very different, the new version will not be fully compatible with the data of the old version. If you cannot bear such changes, please adjust your mindset or continue to use the old version.

Only the three tables `problem`, `problemtag`, and `user` can be migrated. The data after migration is changed to:

### user table

+ The `ac_number` of the imported user, `submition_number` will be set to 0
+ The previous user permissions are still maintained, but the `problem permission` of admin is set to `own`
+ If the user's `email` is invalid (usually a temporarily generated contest user), it will not be imported
+ Those with the same name as an existing user will not be imported

### problemtag table

Will be all imported normally

### problem table

+ The statistical information of the imported problem is the initial state
+ The imported problem `languages` is set to `["C", "C++"]`
+ The `difficulty` of the imported problems are all set to `Mid`
+ The creation time of the problem, the creator and other information remain unchanged
+ The import process needs to enter a prefix, which is the prefix of the Display ID of the imported problem. For example, if you enter `old`, the Display ID of the imported problem will be `old1`, `old2`, `old2`...
+ Problems that duplicate the existing `Display ID` will not be imported

!> Please read the above migration situation carefully and make a data backup, make a data backup, make a data backup! ! !

## Migration process:

!> This tutorial is only for the OJ built using the official `OnlineJudgeDeploy`, if you build it in other ways, please migrate by yourself

The following tutorial requires that the new version and the old version can be used normally, and the new version is updated to the latest version, otherwise it may cause import failure

### Backup 2.0 database

The easiest way to back up the database is to back up the database file directly: In the `OnlineJudgeDeploy` directory of 2.0, run

```bash
cp -r data data_bak
```

This completes the backup. When there is a problem with the import, you can delete the `data` directory and copy the `data_bak` back.

### Prepare 1.0 data

+ Database data
    Run on a 1.0 machine
    ```bash
    docker exec -it oj_web_server python manage.py dumpdata problem account.user --indent 2> old_data.json
    ```

    Under normal circumstances, you will see a file named `old_data.json` in this directory. This file is required for the following steps.

+ test_cases

    The `/data/test_case` in the 1.0 `OnlineJudgeDeploy` directory stores all 1.0 test case data. You need to manually copy all test cases to the new version of `OnlineJudgeDeploy/data/backend/test_case`, and you can use it
    ```bash
    zip -r test_case.zip test_case
    ```
    Pack it into a zip for easy copying, make sure the move is completed before proceeding to the following steps

### Run the import script

Copy the above `old_data.json` to the 2.0 machine, make sure there is the file `old_data.json` in the current directory, and then run in turn:

```bash
docker cp old_data.json oj-backend:/app/utils/
docker exec -it oj-backend /bin/sh
cd utils
python3 migrate_data.py old_data.json
```

Then follow the script prompts to import to complete the import, after importing, you can enter the background to view the import situation

If you have any questions, please issue or ask for help in the discussion group
