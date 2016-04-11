So you've just finished hacking your iPhone and installed every application you could find, but what the heck happened to your battery life? In the process of installing various things, you've likely installed OpenSSH, the tool which allows remote logins to your iPhone from a computer. The problem is, OpenSSH requires that a listener called SSHd constantly runs, waiting for a remote login attempt. This, in turn, drains your battery. The solution is to disable SSHd when you don't need it.

For most seasoned iPhone hackers, disabling SSHd is a piece of cake. Unfortunately, the usual way is not very easy or convenient. Fortunately, there is a simple tool that'll do this.

# Instructions #

  1. In Installer, install the Community Sources (if you haven't already).
  1. In the Utilities category, install Services.
  1. Go back to the home screen and wait for the iPhone to reload the SpringBoard. Slide to unlock and launch Services.
  1. From Services, toggle SSH off.

That's it! Your battery life should now return to its original state. If you ever need to use SSH (including SFTP) again, simply use Services to turn SSH back on.

**Credits to iPhone Alley for this article**