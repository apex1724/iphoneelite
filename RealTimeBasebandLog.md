# How to Log the Baseband in Real-Time #

From SSH (or MobileTerminal), issue these commands:

```
rm /dev/console
mknod /dev/console c 4 0
kill -USR1 `ps aux|grep omm|grep ys|xargs|cut -d ' ' -f 1`
kill -HUP `ps aux|grep omm|grep ys|xargs|cut -d ' ' -f 1`
```

# How to Stop the Log #

From SSH (or MobileTerminal), issue these commands:

```
rm /dev/console
mknod /dev/console c 1 0
kill -USR1 `ps aux|grep omm|grep ys|xargs|cut -d ' ' -f 1`
```