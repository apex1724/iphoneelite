# Instructions #

  1. Enter DFU mode by pressing the power and home buttons, and releasing the power button after 10 seconds. You may release the home button after iTunes detects your phone. Note that iTunes will say it has detected a phone in recovery mode - this is perfectly normal. (In DFU mode your screen should be white or black. If it is not, then you are in recovery mode. You want to be in DFU mode. See the troubleshooting section below.)
  1. In iTunes, restore your phone. If you're on a Mac, then hold the option key down and click Restore. If you're on Windows, then hold the Shift key down and click Restore. You will be asked to select your IPSW - select your 1.0.2 IPSW.
  1. Your phone will be restored and will end with error 1013 or 1015. This is normal, as the baseband cannot be downgraded (without a hack - see below). To boot into the phone, go into iPHUC and issue `cmd setenv auto-boot true`, `cmd saveenv`, and `cmd fsboot`. If you're on a Mac you can also launch iNdependence and it will boot your phone.
  1. You've just downgraded!

# Downgrading the Baseband #

Normally, the baseband cannot be downgraded. The `bbupdater` tool only allows an upgrade, not a downgrade. However, with a 3.9 bootloader it is possible to downgrade your baseband by first erasing the baseband (using `ieraser`). To do this, upload `ieraser`, `bbupdater`, the 03.14.08 baseband firmware, and the 04.02.13 secpack (in a file called 'secpack') to your phone. After setting the permissions to 755, run `ieraser`, and then run `bbupdater` to downgrade your baseband. These tools/files are commonly available.

# Updating to 1.1.2 #

From here, you can also update to 1.1.2 as long as you have prepared for the jailbreak. To prepare for the jailbreak, install BSD Subsystem and then OktoPrep. Or, you can choose to do this manually; SSH into your phone and type the following commands:

```
cd /var/root/Media
mknod disk c 14 1
chmod 666 disk
```

**Perform an UPDATE to 1.1.2 - NOT a restore.**

# Troubleshooting #

If you get error 1, that means you are not in DFU mode, but rather recovery mode. DFU and recovery mode are two different modes. The main difference between DFU mode and recovery mode is that you can downgrade firmware in DFU mode.

To fix this, you can either try the button trick again (hold the power and home buttons, release the power after 10 seconds, and release the home button after iTunes detects the phone), or you can use iPHUC to upload the WTF.s5l8900.RELEASE.dfu file (it's in the IPSWs) and then type `cmd go`.