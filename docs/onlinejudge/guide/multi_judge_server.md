# Multiple evaluation machines

Multiple evaluation machines only need to ensure two points to operate normally:

 -JudgeServer Token is the same
 -Multi-machine synchronization of test cases

This OJ uses `rsync` to synchronize, the steps are as follows:

### Start the test case synchronization master service on the deployed machine

On the deployed server, modify the file `docker-compose.yml` in `OnlineJudgeDeploy`

Incorporate the following code (that is, add a service, pay attention to indentation), and run `docker-compose up -d`:

```yaml
  oj-rsync-master:
    image: registry.cn-hangzhou.aliyuncs.com/onlinejudge/oj_rsync
    container_name: oj-rsync-master
    volumes:
      -$PWD/data/backend/test_case:/test_case:ro
      -$PWD/data/rsync_master:/log
    environment:
      -RSYNC_MODE=master
      -RSYNC_USER=ojrsync
      -RSYNC_PASSWORD=CHANGE_THIS_PASSWORD
    ports:
      -"0.0.0.0:873:873"
```

!> Be sure to modify `RSYNC_PASSWORD`, otherwise it will cause the leakage of test cases

### Configure JudgeServer and test case synchronization slave service on the new machine

On the new machine, initialize the environment according to the `OnlineJudgeDeploy` project, modify `docker-compose.yml`, only need to keep `judge-server` as a service, and then add the following service to the file.

```yaml
  oj-rsync-slave:
    image: registry.cn-hangzhou.aliyuncs.com/onlinejudge/oj_rsync
    container_name: oj-rsync-slave
    volumes:
      -$PWD/data/backend/test_case:/test_case
      -$PWD/data/rsync_slave:/log
    environment:
      -RSYNC_MODE=slave
      -RSYNC_USER=ojrsync
      -RSYNC_PASSWORD=CHANGE_THIS_PASSWORD
      -RSYNC_MASTER_ADDR=YOUR_BACKEND_ADDR
```

!> Please modify `RSYNC_PASSWORD` synchronously, and change `RSYNC_MASTER_ADDR` to the address where the `oj-rsync-master` service is running, without port number, such as `example.com` or `192.168.1.10`.

Then add to JudgeServer

```
  ports:
    -"0.0.0.0:80:8080"
```

Port configuration, but also need to modify

 -`SERVICE_URL` is the address of the new machine
 -The domain name of `BACKEND_URL` is the address of the deployed host
 -`TOKEN` is consistent with the deployed host `TOKEN`.

Run `docker-compose up -d` to start a new JudgeServer, `tail -f data/rsync_slave/rsync_slave.log`, you can see the progress of test case synchronization, and you can see the new JudgeServer in the background of the deployed host Heartbeat status.
