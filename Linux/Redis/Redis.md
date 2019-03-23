# Redis

## Preparation
- OS (CentOS 7)
- [redis-5.0.4.tar.gz](http://download.redis.io/releases/redis-5.0.4.tar.gz)
- gcc  
  > yum install gcc -y

## Installation
- Unzip
  > tar -zxvf redis-5.0.4.tar.gz
- Build
  > make MALLOC=libc

## Start & Shutdown

- Common start
  > $REDIS_HOME/src/redis-server

  The terminal should not be closed, and use the command `CTRL` + `C` to shutdown.

- Start in the background  
  Modify `redis.conf` file, replace `daemonize no` to `daemonize yes`
  > $REDIS_HOME/src/redis-server $REDIS_HOME/redis.conf

  Find the pid and kill it to shutdown.
  > ps -ef | grep redis  
  kill $PID

- Start after booting

## Connect
> $REDIS_HOME/src/redis-cli

---
TODO
- [ ] cluster
