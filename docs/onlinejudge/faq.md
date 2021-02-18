## Is there a mirror of docker hub?

https://hub.docker.com/u/qduoj/

## smtp configuration is unsuccessful?

 - OnlineJudge does not support ssl, please use tls, for example, the qq mailbox is smtp.qq.com / tls / port 25
 - Some email smtp passwords are not login passwords, but separate authorization passwords

## Forgot user password

 - Retrieve password on webpage
 - User management reset
 - If you forget the password of the super administrator, you can use the following command

```
docker exec -it oj-backend /bin/sh
python3 manage.py inituser --username USERNAME --password NEW_PASSWORD --action=reset
```

## CentOS Problems encountered on deployment

 - Check if the docker version is too old
 - Turn off SELinux
  
## View Docker container running status

Run `docker ps -a`, you can see the following output.

```bash
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
645070877c6c registry.cn-hangzhou.aliyuncs.com/onlinejudge/oj_backend "/bin/sh -c'sh /app…" About an hour ago Up About an hour (healthy) 0.0.0.0:443->1443/tcp, 0.0 .0.0:80->8000/tcp oj-backend
b6fc725b2417 registry.docker-cn.com/library/redis:4.0-alpine "docker-entrypoint.s..." About an hour ago Up About an hour 6379/tcp oj-redis
3402b59b96d3 registry.docker-cn.com/library/postgres:10-alpine "docker-entrypoint.s..." About an hour ago Up About an hour 5432/tcp oj-postgres
7c399af69344 registry.cn-hangzhou.aliyuncs.com/onlinejudge/judge_server "/bin/sh -c'/bin/ba…" About an hour ago Up About an hour (healthy) 8080/tcp judge-server
```

`NAMES` is the name of the container, which will be used frequently later. `STATUS` is the current running status of the container, `Up xxx (healthy)` is the normal running status, and `unhealthy` or `Exited (x) xxx` is the exit status.

Note that where `{CONTAINER_NAME}` is used below, it is replaced with the corresponding name, and the braces need to be removed.

## Enter the running container

Then run `docker exec -it {CONTAINER_NAME} /bin/sh`, such as `docker exec -it oj-backend /bin/sh`.

## The container exits abnormally

The container `STATUS` is displayed as `Exited(x) xxx`, run `docker logs {CONTAINER_NAME}` to view the error information.

## docker-compose reports an error when starting'module' object has on attribute'connection'

Try to run `pip install --upgrade pip && pip install -U urllib3`, and then try again.

## Invalid token

Please check whether `JUDGE_SERVER_TOKEN` and `TOKEN` in `docker-compose.yml` are consistent

## Port 80 or 443 is occupied and docker cannot be started

Error message `bind 0.0.0.0:80 failed, port is already allocated`

Modify the configuration related to `ports` in docker-compose. For example, `0.0.0.0:80:8080` can be changed to `0.0.0.0:8020:8080`, and the port number after the colon will not conflict. Please do not change it.

## My browser does not display data or displays abnormally

Please use Chrome or Firefox to use this OJ, if you can’t solve it, please feedback the problem.

## How to solve the problem of running error on oj but local success

 - 90% of the possibility is a code bug, which is not triggered locally, especially if there is no complete test data and the same operating environment locally.
 - If it is `Runtime Error`, it may be caused by a crash during code running. If it prompts `signal=31`, it may be that a prohibited system call was triggered. You can see the    system call number through `dmesg`.
 - If you really want to see the code running result, you can modify `docker-compose.yml`, remove the comment of `judger_debug=1`, and then `docker-compose up -d`. After resubmitting, `docker exec -it judge-server bash` `cd /judger/run` can see many folders, and you can find your own code and running results. After debugging, please comment this line and repeat `up -d`, otherwise the results of each run will be retained.

## How to adjust database parameters

In the following cases, you need to consider adjusting database parameters

  - The machine configuration is relatively high, such as CPU more than 4 cores or memory more than 4G
  - The backend reports an error that the number of database connections is not enough `too many clients already`

  Please refer to https://pgtune.leopard.in.ua to select DB Version 10, Number of Connections is 20 times the number of CPU cores, and other parameters are filled in according to actual conditions.

  Then update the parameters on the right to the `OnlineJudgeDeploy/OnlineJudgeDeploy/data/postgres/postgresql.conf` file, note that the original configuration part is commented out, you need to delete the beginning `#`, `docker-compose restart oj-postgres `OK.
