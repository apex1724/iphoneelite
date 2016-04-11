After upgrading to 1.1.2 and replacing lockdownd with a patched version to get signal and activation, you might end up having everything perfect, except for when you try to dial or send an SMS, the application crashes. This page explains why it crashes and how to fix it.

# Why It Crashes #

When you dial a number or send an SMS and enter its number, both applications (MobilePhone.app and MobileSMS.app) crash in the same manner, at the same locations, in the code. The reason for this is because it tries to format a dial string to look nice, and crashes because there is no format string found.

Here's an interpretation of what Apple's code does:

  1. A SIM is inserted.
  1. The MNC/MCC are read out.
  1. Symlinks are created: `/private/var/root/Library/Preferences/com.apple.carrier.plist` and `/private/var/root/Library/Preferences/com.apple.operator.plist` will point to `/System/Library/Frameworks/CoreTelephony.framework/Support/*CarrierName*.plist`, or, if the carrier is not found, `/System/Library/Frameworks/CoreTelephony.framework/Support/UnknownCarrier.plist`.
  1. There is also a symlink from the MNC/MCC codes to the carrier in the same folder. `/System/Library/Frameworks/CoreTelephony.framework/Support/*<MNC><MCC>*` also points to `/System/Library/Frameworks/CoreTelephony.framework/Support/*CarrierName*.plist`.
  1. Now, if you dial, the number formatting string is being pulled out by looking up the MCC/MNC in `/System/Library/Frameworks/UIKit.framework/PhoneFormats/UIMobileCountryCodes.plist` to find the country code. Then the country code is being looked up in `/System/Library/Frameworks/UIKit.framework/PhoneFormats/UIPhoneFormats.plist` for the string formatting rules.

When `UnknownCarrier.plist` is being used because of an unknown MNC/MCC, the lookup in `UIMobileCountryCodes.plist` seems to fail and no country is found, and then the lookup in `UIPhoneFormats.plist` returns `NULL`, which then makes the formatting routine crash.

The fix is to create your own `*<MCC><MNC>*.plist` file. See [this page on Wikipedia](http://en.wikipedia.org/wiki/Mobile_Network_Code#International) for known MNC/MCC values).

So, for example, for a Swisscom Mobile file, you create `SwisscomMobile.plist`. Edit it to include the right APNs, etc, and then create a symlink `22801` to point to `SwisscomMobile.plist`. All of this should be done in `/System/Library/Frameworks/CoreTelephony.framework/Support`. Then swap the SIM card with another SIM (your unused AT&T SIM will work fine) and back. This will recreate the symlinks in `/private/var/root/Library/Preferences`. Now the phone's mapping should properly work.