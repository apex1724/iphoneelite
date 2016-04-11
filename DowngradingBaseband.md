Normally, the baseband does not allow for downgrading. For each firmware flash, a check is present that ensures that it is an upgraded version or the same version. Luckily there is a way around this for 3.9 bootloader phones.

# Prerequisites #

  1. The FLS and EEP file of the firmware you want to flash
  1. The secpack of your current firmware (or a newer one)
  1. The `bbupdater` and `ieraser` tools (don't forget to set the permissions to 755!)
  1. MobileTerminal (you lose Wi-Fi access during this process)

Upload all of these to your phone in the /usr/bin folder.

# Instructions #

  1. Launch MobileTerminal.
  1. Unload CommCenter by typing `launchctl unload /System/Library/LaunchDaemons/com.apple.CommCenter.plist`.
  1. Change folders to the /usr/bin folder by typing `cd /usr/bin`.
  1. Erase your baseband by typing `ieraser`.
  1. Now reflash the new firmware with bbupdater (this can be any firmware version). Type `bbupdater -f firmware.fls -e firmware.eep`. Replace firmware.fls with the FLS file of the firmware you have - and firmware.eep with the EEP file of the firmware you have.
  1. Reboot your phone.

# Unlocking #

If you used anySIM, your iPhone will be relocked and you will need to run anySIM again. If you used IPSF, then your phone should still be unlocked.