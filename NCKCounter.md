The attempt counter is the seczone block at 0xA03FAC88. This block contains the NCK attempt counter and the lock status.

You cannot put unlocked bytes here without the hash of the lock code in the 0xA03FA400 zone.

# Counters #

These are the unencrypted counter zones in 3 different situations: virgin state, after one NCK attempt, after 2 NCK attempts, and after IPSF.

## Virgin ##

```
[0000]  [00]01[05]00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0010]   00 00 00 00 01 00 00 00   01 01 05 01 00 00 00 00   ........ ........
[0020]   09 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0030]   09 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0040]   05 05 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0050]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0060]   00 00 00 00 00 00 00 00   05 05 00 00 00 00 00 00   ........ ........
[0070]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0080]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0090]   05 05 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00a0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00b0]   00 00 00 00 00 00 00 00   05 05 00 00 00 00 00 00   ........ ........
[00c0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00d0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
```

## One NCK Attempt ##

```
[0000]  [01]01[05]00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0010]   00 00 00 00 01 00 00 00   01 01 05 01 00 00 00 00   ........ ........
[0020]   09 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0030]   09 00[01]00[01]00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0040]   05 05 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0050]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0060]   00 00 00 00 00 00 00 00   05 05 00 00 00 00 00 00   ........ ........
[0070]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0080]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0090]   05 05 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00a0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00b0]   00 00 00 00 00 00 00 00   05 05 00 00 00 00 00 00   ........ ........
[00c0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00d0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
```

## Two NCK Attempts ##

```
[0000]  [02]01[00]00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0010]   00 00 00 00 01 00 00 00   01 01 05 01 00 00 00 00   ........ ........
[0020]   09 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0030]   09 00[02]00[07]00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0040]   05 05 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0050]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0060]   00 00 00 00 00 00 00 00   05 05 00 00 00 00 00 00   ........ ........
[0070]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0080]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0090]   05 05 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00a0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00b0]   00 00 00 00 00 00 00 00   05 05 00 00 00 00 00 00   ........ ........
[00c0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00d0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
```

## After IPSF ##

```
[0000]  [02]01[01]04*00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0010]   00 00 00 00 01 00 00 00  *05*05 05 01*00 00 00 00   ........ ........
[0020]   09 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0030]   09 00[00]00[00]00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0040]   05 05 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0050]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0060]   00 00 00 00 00 00 00 00   05 05 00 00 00 00 00 00   ........ ........
[0070]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0080]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[0090]   05 05 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00a0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00b0]   00 00 00 00 00 00 00 00   05 05 00 00 00 00 00 00   ........ ........
[00c0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
[00d0]   00 00 00 00 00 00 00 00   00 00 00 00 00 00 00 00   ........ ........
```