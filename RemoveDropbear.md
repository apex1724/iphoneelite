# Instructions #
  1. Install BSD Subsystem using Installer.
  1. Install UIctl from Installer.
  1. Run UIctl (be careful with this application). Tap au.asn.ucc.matt.dropbear and choose "unload -w".
  1. Install OpenSSH with Installer.
  1. Connect to your iPhone using SCP or SFTP, with a user name of root, and a password of alpine (if your phone is running 1.1.x) or dottie (if your phone is running 1.0.x).
  1. Using SCP or SFTP, delete the following files and folders on the iPhone: /etc/dropbear (folder) /System/Library/LaunchDaemons/au.asn.ucc.matt.dropbear.plist /usr/bin/dropbear
  1. Restart the iPhone. In UIctl, verify that au.asn.ucc.matt.dropbear is gone. If so, dropbear-ssh is not loaded (which is what we want).

If you have dropbear-ssh loaded with com.apple.update.plist:

  1. Install BSD Subsystem using Installer.
  1. Install OpenSSH with Installer.
  1. Copy com.apple.update.plist.orig from your sshkit folder to /System/Library/LaunchDaemons/com.apple.update.plist (Delete com.apple.update.plist beforehand, and make sure comm.apple.update.plist.orig is named com.apple.update after copying.)
  1. Restart the iPhone.
  1. Connect to your iPhone using SCP, with a user name of root, and a password of alpine (if your phone is running 1.1.x) or dottie (if your phone is running 1.0.x).
  1. Using SCP or SFTP, delete the following files and folders on the iPhone: /etc/dropbear (folder) /etc/init.d (folder) /usr/bin/dropbear /etc/hackinit.sh
  1. Restart the iPhone again.