# Overview #

The `lockdownd` binary is responsible for several tasks, including activation, unlocking FairPlay DRM certificates, delegating communications to other services, and ensuring that Apple's security logic is implemented, to prevent people from using their iPhones in an unintended manner. This binary is the one that is either fooled or modified to allow activation, on an iPhone, that is not considered proper by Apple.

# Activation States #

The iPhone has been designed so that it must be properly activated before it may be used. The `lockdownd` binary is responsible for maintaining and verifying the activation state. The 1.1.2 `lockdownd` binary has the following activation states:

**Unactivated**: This state prevents the GUI on the iPhone from being accessed. In addition, it causes the modem to have no signal and only allows iTunes to attempt activation.

**Activated**: This state allows full access to the GUI and the modem. iTunes also allows full functionality.

**FactoryActivated**: This state allows full access to the GUI and the modem. It is made for quality control personnel to inspect the functionality of the phone.

**SoftActivation**: This state is used by AppleCare, and does not specify an ICCID. It allows full access to the GUI and full access from iTunes. It may be used with any SIM card.

**MissingSIM**: This state causes iTunes to display a message box informing the user that the iPhone may not be used with iTunes until a SIM card is installed. Full access to the GUI is allowed.

**MismatchedICCID**: This state is entered when the current SIM card's ICCID does not match the activation token's ICCID. It allows full access to the GUI, but the modem is disabled.

**MismatchedIMEI**: This state is never entered. It is intended for when the current IMEI does not match the one in the activation token.

Certain versions of iPHUC allow the user to retrieve the current activation state. In order to do this, connect an iPhone to your computer, in standard mode, and issue the command `readvalue ActivationState`. Versions compiled for Windows generally attempt to use CFShow to display the activation state. Unfortunately, this function is not documented to work on the Windows OS. However, the version of iPhoneInterface that accompanies iBrickr will correctly display the activation state.

# Activation Verification #

The following process outlines the steps that are used to authenticate an account token.

  1. An activation token is received by the device, or the current activation state is queried.
  1. The activation token's cryptographic signature is checked.
  1. The unique device ID is retrieved from the data ark.
  1. The activation state is retrieved from the data ark. If the activation state is not successfully retrieved, then the new activation state is set to Unactivated, bEnableBrickState is set to true, aPreviousActivationState is set to false, and it jumps to step 22.
  1. The account token is retrieved from the data given to the function. If the account token is not successfully retrieved, then set the new activation state to Unactivated, register no unlock code, and it jumps to step 21.
  1. Attempt to create a property list from the XML data. If this fails, then set the new activation state to Unactivated, register no unlock code, and jump to step 21.
  1. Attempt to get the unique device ID from the account token. If this fails, then set the new activation state to Unactivated, set the unlock code to no unlock code, and jump to step 21.
  1. Check to see if the unique device IDs match. If they do not match, jump to step 9.
  1. Check to see if lockdownd is running on the iPhone. If it isn't, then set the new activation state to Activated and jump to step 21.
  1. Check to see if the activation token has a FactoryActivation key set to true or if the account token was validated by a factory certificate. If either is true, then set the new activation state to FactoryActivated, set the unlock code to none, and jump to step 21.
  1. Get the IntegratedCircuitCardIdentity value from the data ark. If this fails, then set the new activation state to MissingSIM and jump to step 14.
  1. Get the IntegratedCircuitCardIdentity value from the activation token. If this fails, then set the new activation state to SoftActivation and jump to step 14.
  1. Compare the new IntegratedCircuitCardIdentity values from steps 11 and 12. If they match, then set the new activation state to Activated. If they do not, then set the new activation state to MismatchedICCID.
  1. Look up the InternationalMobileSubscriberIdentity from the data ark. If this fails, then go to step 17.
  1. Look up the InternationalMobileSubscriberIdentity from the account token. If this fails, then go to step 17.
  1. Compare the InternationalMobileSubscriberIdentity from steps 14 and 15. If they do not match, then set the new activation state to MismatchedICCID.
  1. Check to see if the account token contains an unlock code. If it does, then register the unlock code and set the new activation state to Unlocked.
  1. Retrieve the InternationalMobileEquipmentIdentity from the data ark. On failure, jump to step 21.
  1. Retrieve the InternationalMobileEquipmentIdentity from the account token. On failure, jump to step 21.
  1. Compare the InternationalMobileEquipmentIdentity from steps 18 and 19. If they do not match, then set the new activation state to Unactivated.
  1. If the new activation state is Unactivated or MismatchedICCID, then set bEnableBrickState to true. Otherwise, set it to false.
  1. Check to see if lockdownd is running on the iPhone. If it isn't, then jump to step 25.
  1. Get the current brick state from the data ark.
  1. If the current brick state is different from the bEnableBrickState value, then set the new brick state to bEnableBrickState from the data ark.
  1. Check to see if aPreviousActivationState is true. If not, then jump to step 30.
  1. Check to see if the new activation state is Unlocked. If not, then jump to step 28.
  1. Attempt to unlock the modem with the supplied lock code and set the new activation state to Activated.
  1. Set the activation state to the new activation state in the data ark and in the lockdown cache.
  1. If bEnableBrickState is true, then enable the brick state in the CoreTelephonyServer.
  1. Finished.

# Apple Security Logic #

Another task the `lockdownd` binary is concerned with is ensuring that the iPhone is in a state that Apple deems proper. One of these checks ensures that the modem's firmware version is consistent with the version that is distributed alongside the iPhone software. If these versions differ, then the modem is put into brick mode. Brick mode causes the modem to not register to any carrier and it will display no signal within the GUI.

Additionally, there are some references inside the 1.1.2 `lockdownd` binary to voiding the warranty. It appears that iTunes may send messages to the iPhone informing it that its warranty is invalid. It is an assumption that this is used because it would be hard for Apple to determine if an iPhone is outside of its warranty when it has its software completely reloaded.

# Prior Work by Others #

DVD Jon created an activation method that worked with older versions of the iPhone firmware. DVD Jon's method patched iTunes to use HTTP instead of HTTPS for the activation server, and redirected the activation requests to the Phone Activation Server. The Phone Activation Server would then send a valid account token to the iPhone. However, the account token was for a different IMEI, ICCID, and DeviceID. This method left the phone in the MismatchedICCID state, but allowed access to the user interface.

An activation method called iASign was released by the iPhone Dev Team. This activation method would change the certificates on the iPhone and would send an activation token that was signed by the new certificates. This allowed fully compliant tokens, however, it required a lengthy process to activate the iPhone.

Several patches have been distributed that modify the `lockdownd` binary directly. These patches generally modify the routine that checks the activation state of the iPhone and cause it to always believe it has been factory activated. The way this is done is to change step 4 to set the new activation state to FactoryActivated, set bEnableBrickState to false, and set aPreviousActivationState to false, and then jump to step 22.

# Improvements Upon Prior Work #

DVD Jon's activation method allows use of the iPhone GUI, but it does not allow use of the phone. iASign allows use of both the GUI and the phone, but it does not allow changing the SIM card without generating a new account token. Neither of the aforementioned activation methods will allow use of the iPhone with iTunes if a SIM card is not installed, and they leave the security logic intact.

The current patches to the `lockdownd` binary circumvent the previous issues. However, they do not bypass Apple's security logic. They will also not allow a person who has installed these patches, to unlock their phones via an Apple-supplied unlock code without reinstalling `lockdownd`. Finally, they are incompatible with alternate or additional unlock methods such as iASign.

# Current Work #

The current patches have been developed to modify the verification process so that brick mode is never enabled, and all references to Unactivated, MissingSIM, MismatchedICCID, and MismatchedIMEI have been changed to FactoryActivated. This allows people who have valid account tokens, to use the iPhone in the state that they have activated, allowing users who receive unlock codes from Apple to apply them without reinstalling `lockdownd`, as well allowing users to use iASign in addition to this patching method.

In addition, parts of Apple's security logic have also been circumvented. All parts of `lockdownd` that attempt to void the iPhone's warranty (on the 1.1.2 `lockdownd`) have been removed. Also, `lockdownd` has been modified to allow all baseband versions without enabling brick mode on the phone. This allows experimentation with different modem firmwares without having to modify `lockdownd`.

# `lockdownd` Patches #

The following shows the location and purpose of the modifications made to the stock `lockdownd` binary.

## 1.0.0 ##

| **Offset** | **Original Byte** | **New Byte** | **Reason** |
|:-----------|:------------------|:-------------|:-----------|
| 0x8CF8     | 0x01              | 0x00         | Change enable brick mode to disable |
| 0x90A4     | 0x01              | 0x00         | Change enable brick mode to disable |
| 0x90A8     | 0x3C              | 0x68         | Change Unactivated to FactoryActivated |
| 0x9178     | 0x84              | 0x98         | Change MismatchedIMEI to FactoryActivated |
| 0x91B8     | 0x01              | 0x00         | Change enable brick mode to disable |
| 0x91D8     | 0x2C              | 0x38         | Change MismatchedICCID to FactoryActivated |
| 0x91E0     | 0x28              | 0x30         | Change MissingSIM to FactoryActivated |
| 0x9258     | 0x01              | 0x00         | Change enable brick mode to disable |

## 1.0.1 ##

| **Offset** | **Original Byte** | **New Byte** | **Reason** |
|:-----------|:------------------|:-------------|:-----------|
| 0x9158     | 0x01              | 0x00         | Change enable brick mode to disable |
| 0x94C4     | 0x01              | 0x00         | Change enable brick mode to disable |
| 0x94C8     | 0x3C              | 0x68         | Change Unactivated to FactoryActivated |
| 0x9598     | 0x84              | 0x98         | Change MismatchedIMEI to FactoryActivated |
| 0x95D8     | 0x01              | 0x00         | Change enable brick mode to disable |
| 0x95F8     | 0x2C              | 0x38         | Change MismatchedICCID to FactoryActivated |
| 0x9600     | 0x28              | 0x30         | Change MissingSIM to FactoryActivated |
| 0x9678     | 0x01              | 0x00         | Change enable brick mode to disable |

## 1.0.2 ##

| **Offset** | **Original Byte** | **New Byte** | **Reason** |
|:-----------|:------------------|:-------------|:-----------|
| 0x9184     | 0x01              | 0x00         | Change enable brick mode to disable |
| 0x94F0     | 0x01              | 0x00         | Change enable brick mode to disable |
| 0x94F4     | 0x3C              | 0x68         | Change Unactivated to FactoryActivated |
| 0x95C4     | 0x84              | 0x98         | Change MismatchedIMEI to FactoryActivated |
| 0x9604     | 0x01              | 0x00         | Change enable brick mode to disable |
| 0x9624     | 0x2C              | 0x38         | Change MismatchedICCID to FactoryActivated |
| 0x962C     | 0x28              | 0x30         | Change MissingSIM to FactoryActivated |
| 0x96A4     | 0x01              | 0x00         | Change enable brick mode to disable |

## 1.1.1 ##

| **Offset** | **Original Byte(s)** | **New Byte(s)** | **Reason** |
|:-----------|:---------------------|:----------------|:-----------|
| 0x482F     | 0x1A                 | 0xEA            | Ignore baseband version |
| 0xAF5C     | 0x01                 | 0x00            | Change enable brick mode to disable |
| 0xB814     | 0x24                 | 0x54            | Change Unactivated to FactoryActivated |
| 0xB818     | 0x01                 | 0x00            | Change enable brick mode to disable |
| 0xB838     | 0x00                 | 0x30            | Change Unactivated to FactoryActivated |
| 0xB858     | 0xE0 0x14            | 0x10 0x15       | Change Unactivated to FactoryActivated |
| 0xB884     | 0xB4                 | 0xE4            | Change Unactivated to FactoryActivated |
| 0xB958     | 0x00                 | 0x10            | Change MismatchedICCID to FactoryActivated |
| 0xB970     | 0xEC                 | 0xF8            | Change MissingSIM to FactoryActivated |
| 0xB9E0     | 0x58                 | 0x88            | Change Unactivated to FactoryActivated |
| 0xBA58     | 0x01                 | 0x00            | Change enable brick mode to disable |

## 1.1.2 ##

| **Offset** | **Original Byte(s)** | **New Byte(s)** | **Reason** |
|:-----------|:---------------------|:----------------|:-----------|
| 0x4B3B     | 0x1A                 | 0xEA            | Ignore baseband version |
| 0x79FC     | 0xD7 0xFF            | 0x00 0x00       | Disallows enabling of voided warranty |
| 0x79FE     | 0xFF 0x1A            | 0xA0 0xE1       | Part of patch at 0x79FC |
| 0x7E0B     | 0x0A                 | 0xEA            | Disallows enabling of voided warranty |
| 0xAC73     | 0x0A                 | 0xEA            | Disallows enabling of voided warranty |
| 0xBC40     | 0x01                 | 0x00            | Change enable brick mode to disable |
| 0xC5CC     | 0x01                 | 0x00            | Change enable brick mode to disable |
| 0xC5D4     | 0x88                 | 0xEC            | Change Unactivated to FactoryActivated |
| 0xC614     | 0x48                 | 0xAC            | Change Unactivated to FactoryActivated |
| 0xC640     | 0x1C                 | 0x80            | Change Unactivated to FactoryActivated |
| 0xC6F0     | 0x90                 | 0xD0            | Change MissingSIM to FactoryActivated |
| 0xC74C     | 0x44                 | 0x74            | Change MismatchedICCID to FactoryActivated |
| 0xC7DC     | 0xB4                 | 0xE4            | Change MismatchedICCID to FactoryActivated |
| 0xC8AC     | 0xB0 0x33            | 0x14 0x34       | Change Unactivated to FactoryActivated |
| 0xC904     | 0x01                 | 0x00            | Change enable brick mode to disable |

# Issues with the Current Work #

If brick mode has been enabled on the iPhone, usually `lockdownd` stores a record that it has been enabled. However, it has been observed that a different `lockdownd` can enable brick mode without storing this record. When this happens, the current `lockdownd` is unaware of the brick mode state and won't disable brick mode. GUI access will still be allowed to the iPhone, but the phone will appear to get no signal. This is a limitation in the original design of the software. A piece of software is available, called `bricktool`, that will allow a user to enable or disable brick mode outside of `lockdownd` and fixes this issue.