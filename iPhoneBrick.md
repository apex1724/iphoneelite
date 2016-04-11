# Want to Know Why Your Phones Got Bricked? #

anySIM and iUnlock were patched to make a routine exit of 0 (successful) to unlock the phone. The only problem was that the routine is not only called by NCK but rather by about 6 different routines total, because the baseband code is very well optimized. An analogy was made in the IRC channel that it was basically like patching memcpy. The other five didn't expect 00 to be there and were therefore spammed cross your baseband during the upgrade.

Here is the disassembly of where the patch was made:

```
ROM:A0237B38                 MOV     R0, R6
ROM:A0237B3C                 BL      IMPORTANT_ROUTINE
ROM:A0237B40                 CMP     R0, #0
ROM:A0237B44                 BNE     exit            ; NOP
ROM:A0237B48                 ADR     R0, a00000000   ; "00000000"
ROM:A0237B4C                 LDMIA   R0, {R0,R1}
ROM:A0237B50                 STR     R0, [R5,#0x28]
ROM:A0237B54                 STR     R1, [R5,#0x2C]
ROM:A0237B58                 B       loc_A0237B98
```

Here we see the actual patch:

```
ROM:A0235148                 ; PATCH: MOV R0, 0
ROM:A0235148                 MOV     R0, R4
```

They just changed **MOV [R0](https://code.google.com/p/iphoneelite/source/detail?r=0), [R4](https://code.google.com/p/iphoneelite/source/detail?r=4)** to **MOV [R0](https://code.google.com/p/iphoneelite/source/detail?r=0), 0**.

IMPORTANT\_ROUTINE is what would be memcpy in the analogy. The patch was made to cause IMPORTANT\_ROUTINE to exit 0 (successful) by putting MOV [R0](https://code.google.com/p/iphoneelite/source/detail?r=0), #0 at the end. The coder wanted that routine to go to the "000000." It is possible that attempts were made to NOP at the comment above, but it was unsuccessful. It is also possible other routines were NOP'd, but without any luck. So the coder put 0 inside the routine, and bam - it unlocked. But as was stated above, this is the wrong routine. So again, imagine you'd patched memcpy to behave this way.

[A method to virginize your phone](RevirginizingTool.md) is now available. This, in essence, makes your phone as if it was just purchased - virgin.