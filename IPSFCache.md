IPSF users can probably recover their original seczone token value before IPSF zeroed it out.

# Saving the Cache #

**IPSF users should do the following**: (This only works if you have not restored since you ran IPSF.)

  1. Make sure you have BSD Subsystem and SSH on your iPhone.
  1. SSH into your iPhone and type the following. (If you get an error like "missing destination file," then you either have no cache, or you typed something wrong.)

```
cp $(find /var/root/Library/Caches/bbsimfree -name "*.cache") /ipsf.cache
```

Copy that cache off of your iPhone and save it! It contains very valuable data.

# Using the Cache #

To recover your token manually, do the following:

  1. Using a hex editor, find the LTOKEN1.0 string in the cache and note its starting offset (call this value "a"). In our example, a = 0x1E7.
  1. Compute the offset of the encrypted seczone, which will be 0x810 bytes after the start of that string: b = a + 0x810. So, in our example, b = 0x9F7.
    1. See [HowIPSFWorks](HowIPSFWorks.md) for details about that 0x810 constant.
  1. Extract the 0x2000 bytes beginning at that offset into a file called "en"
  1. Run George Hotz's `deipsf` program to produce the "de" file. That is your original seczone.
    1. Note that `deipsf` only works on little-endian architecture, such as X86 or ARM (not PowerPC)
    1. Sanity check the "de" file. It should begin with 0x100 bytes of "FF," and then non-FF bytes. If you don't see that, then something went wrong - try again.
  1. Use the decrypted seczone in a [flow like this one](http://rdgaccess.com/iphone-elite/viewtopic.php?t=158).