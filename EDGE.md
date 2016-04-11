These tests were performed on TIM, an Italian carrier.

# Test Results #

Download (from network to phone - SFTP put from another host):

```
100% 3089KB 22KB/s 00:02:16
```

Upload (from phone to network - SFTP get from another host):

```
100% 3089KB 10KB/s 00:04:41
```

The **downstream** is about 176 kilobits per second, but there was also SSH and a VPN - so the actual speed is probably about 200 kilobits per second. The **upstream** is half, so it could be 128 kilobits (80 kilobits per second through SSH and VPN).