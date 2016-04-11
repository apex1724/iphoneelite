The /Applications folder is in the system partition. Unfortunately, the system partition only has 300MBs of space. After installing quite a few applications, Installer will start complaining that you're running out of space. The fix is simple: move the applications folder to a bigger disk (/dev/disk0s2) and point a symlink to it.

# Instructions #

  1. Connect to your iPhone with SSH.
  1. Type `cp -rf /Applications /private/var/root`
  1. Type `rm -rf /Applications`
  1. Type `ln -s /private/var/root/Applications /Applications`
  1. Reboot your phone.