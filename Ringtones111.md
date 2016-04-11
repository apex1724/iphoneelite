The 1.1.1 firmware does not recognize .M4A files as ringtones. The .M4R files are protected, so simply renaming .M4A to .M4R does not work. The following workaround will allow you to install and use your own .M4A ringtones. After patching the MeCCA, all of the .M4R files which have a space as the last character of the filename (right before the .M4R part) will be treated as regular .M4A files.

# Patches #

| **Offset** | **Original Byte** | **New Byte** |
|:-----------|:------------------|:-------------|
| 0x14458    | 0x05              | 0x06         |
| 0x1445C    | 0x6C              | 0x03         |
| 0x1445D    | 0x10              | 0x30         |
| 0x1445E    | 0x9F              | 0x43         |
| 0x1445F    | 0xE5              | 0xE0         |
| 0x14462    | 0x8F              | 0x50         |
| 0x14463    | 0xE0              | 0xE5         |
| 0x14464    | 0x6D              | 0x20         |
| 0x14465    | 0x28              | 0x00         |
| 0x14466    | 0x03              | 0x51         |
| 0x14467    | 0xEB              | 0xE3         |
| 0x14468    | 0x00              | 0x02         |
| 0x1446A    | 0x50              | 0x00         |
| 0x1446B    | 0xE3              | 0x0A         |
| 0x1446C    | 0x01              | 0x03         |
| 0x1446D    | 0x30              | 0x10         |
| 0x1446E    | 0xA0              | 0x90         |
| 0x1446F    | 0x03              | 0xE5         |
| 0x14470    | 0x00              | 0x72         |
| 0x14472    | 0x00              | 0x51         |
| 0x14473    | 0x0A              | 0xE3         |
| 0x14474    | 0x00              | 0x01         |
| 0x14477    | 0xE3              | 0x03         |

# Instructions #

  1. Using the patch table above, open a hex editor and patch `/System/Library/Frameworks/MeCCA.framework/MeCCA`.
  1. Reboot your iPhone. You only need to do this once after patching MeCCA.
  1. Put your .M4A ringtones into /Library/Ringtones.
  1. You need to rename your ringtone so it will end with ' .M4R'. Notice the space before the dot. It will indicate that no special .M4R treatment is needed for the file. So normal .M4R files will look like 'Tone1.m4r' or 'Tone2.m4r'. Custom .M4A files will look like 'Tone1 .m4r' or 'Tone2 .m4r'.
  1. Go to Settings, and then Sounds. Select your ringtone.