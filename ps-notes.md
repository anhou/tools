## 杀死进程组
```
ps a -o pid,pgid,command
sudo pkill -g <PGID>
```

```
ps a -o pid,pgid,ppid,command
sudo pkill -g <PPID>
```
