# Commands #

| **AT+IPR=XXXX** | Sets the terminal speed |
|:----------------|:------------------------|
| **AT+XGENDATA** | Prints the baseband version, EEP version, and revision |
| **AT+XTRANSPORTMODE** | Leaves AT mode and enters TRANSPORT mode (binary) |
| **AT+XLOG=0**   | Prints the firmware version |
| **AT+XPINCNT**  | Prints the SIM PIN counter (the number of attempts left) |
| **AT+CCID**     | Prints the SIM ICCID    |
| **AT+XL1SET="command"** | Various commands - see note 1 |
| **AT+XRRSET="command"** | Various commands - see note 2 |
| **AT+XGCNTSET=???** | Various commands - see note 3 |
| **AT+XGCNTRD=???** | Various commands - see note 3 |

# Notes #

  1. "HELP," "IROFF," "IRON," "IR\_OPT\_OFF," "IR\_OPT\_ON," "IROFF"
  1. "HELP," "PSVOFF," "PSVON," "SEV," "MON:," "MS\_ERROR\_LOG," "HO\_FAIL\_ON," "HO\_FAIL\_OFF," "SCELL," "BALIST," "PDCH\_FILTER"
  1. Unknown parameters. GRPS-related.

# To be Inspected #

## System Commands ##

```
AT+XDIAG
AT+XGENDATA
AT+XMER
AT+XCEER
AT+XCIND
AT+XCONFIG
AT+XCTMDR
AT+XCTMS
AT+XSLN
AT+XALS
AT+XHANDSFREE
AT+XSELFRXSTAT
AT+CSQ
AT+CTZU
AT+CTZR
AT+CALM
AT+CRSL
AT+CLVL
AT+CMUT
AT+XALSBLOCK
AT+XLIN
AT+XHOMEZR
AT+XCIPH
AT+CGED
AT+XREL
AT+XCSPAGING
AT+TRACE
AT+XSERVICE
AT+XTDEV
AT+XTFILTER
AT+XTRACEIP
AT+CMEC
AT+XMAGETKEY
AT+XMAGETBLOCK
AT+XPOW
AT+XTRACESYSTIME
```

## Test Commands ##

```
AT+XDEVICE
AT+XMULTISLOT
AT+XRLCSET
AT+XRRSET
AT+XSIMLOOPBACK
AT+XLOOPBACK
AT+XPPP
```