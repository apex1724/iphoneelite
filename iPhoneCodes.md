CommCenter supports feature codes that enable you to change certain things from within the phone (by simply dialing numbers).

# Codes #

```
Any command can be preceded by *#, ** or ##

*# is INTERROGATION
** is SETUP
## is CANCEL / DELETE

*#5005*78283#           This dumps a baseband log in /Library/Logs/Baseband/ - see Note 1
**5005*78283#           This dumps a baseband log in /Library/Logs/Baseband/ - see Note 1
##5005*78283#           This dumps a baseband log in /Library/Logs/Baseband/ - see Note 1
*#5005*62#              "Error performing request - No Network Service" - see Note 2
*#5005*62255#           "Error performing request - No Network Service" - see Note 2
*#5005*87223#           "Error performing request - No Network Service" - see Note 2
*#5005*86#              Displays the voicemail dial-in number
**5005*86*VOICEMAIL#    Sets the voicemail number to VOICEMAIL (international format)
##5005*86#              Erases the voicemail number from phone
*#5005*7672#            Display the SMSC setting
**5005*7672*SMSCNUMBER# Sets the SMSC to SMSCNUMBER (international format)
##5005*7672#            Erases the SMSC number from phone
*#5005*22#              "Error performing request - No Network Service" - see Note 2
**5005*22#              Unknown. Displays "Please wait"; returns to dial pad - see Note 2
##5005*22#              Unknown. Displays "Please wait"; returns to dial pad - see Note 2
*#5005*5264#            This reads LANG and tells you the "actual language" (English)
*#5005#                 "Error performing request - No Network Service" - Note 2
##5005*22*12345678#     Function unknown, displays "please wait.."; power off to exit
```

**Note 1**: It may be necessary to change the prefix from the set of `*#`, `**`, and `##`, to initiate a new log dump. The logs are not tailable, since this is a single dump, with no additional data written. So far, these log files appear to always remain the same size. Log files are stamped with the current date.

**Note 2**: These tests were performed with a stock phone on the AT&T network. It is possible that these errors represent debugging features that are only available on a special debug cellular base-station (CONJECTURE only).