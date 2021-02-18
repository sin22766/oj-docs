# Code upgrade instructions

Because the new version of the system is still in the rapid upgrade iteration, if you deploy or are using OnlineJudge, it is recommended to watch this project so that you can receive email reminders every time a new release is released.

# Upgrade steps

!> The following methods are only applicable to OJs built with the official [deployment script](https://github.com/QingdaoU/OnlineJudgeDeploy)

If you make changes to the deployment warehouse code, please back it up by yourself or `git stash`, and then run the following command in the OnlineJudgeDeploy directory to complete the upgrade:

```bash
git pull
docker-compose pull && docker-compose up -d
```

But generally speaking, the mirrors of `Redis` and `Postgresql` do not need to be updated, so you can pull the OJ-related mirrors separately, which can save upgrade time, which is the same as the effect achieved by the above command in most cases (unless the larger version upgrade):

```bash
git pull
docker pull registry.cn-hangzhou.aliyuncs.com/onlinejudge/judge_server
docker pull registry.cn-hangzhou.aliyuncs.com/onlinejudge/oj_backend
docker-compose up -d
```

If you still have any questions, please file an issue.
