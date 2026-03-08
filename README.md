# Guide: Linux Audio on the Lenovo Legion Pro 7i Gen 10 (16IAX10H)

This guide explains how to get audio working correctly on the Lenovo Legion Pro 7i Gen 10 (**16IAX10H**). Since this solution is still very new, it will take some time for all components to be properly integrated into the Linux kernel. Until that happens, you can follow the steps below, which have been rigorously tested and are confirmed to work. This guide will be updated for future kernel versions as they are released, until the fix is fully integrated into the kernel.



## Disclaimer

I, Nadim Kobeissi, attest that all components of the fix provided here have been tested and work without any apparent harmful effects. The fix components are provided in good faith. However, I (as well as the main fix authors) disclaim all responsibility for any use of this fix and guide:

```
THE PROGRAM IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU. SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL NECESSARY SERVICING, REPAIR OR CORRECTION.
```

## Credits

Fixing this issue required weeks of intensive work from multiple people.

Approximately 95% of the engineering work was done by [Lyapsus](https://github.com/Lyapsus). Lyapsus improved an incomplete kernel driver, wrote new kernel codecs and side-codecs, and contributed much more. I want to emphasize his incredible kindness and dedication to solving this issue. He is the primary force behind this fix, and without him, it would never have been possible.

I ([Nadim Kobeissi](https://nadim.computer)) conducted the initial investigation that identified the missing components needed for audio to work on the 16IAX10H on Linux. Building on what I learned from Lyapsus's work, I helped debug and clean up his kernel code, tested it, and made minor improvements. I also contributed the solution to the volume control issue documented in Step 8, and wrote this guide.

Gergo K. showed me how to extract the AW88399 firmware from the Windows driver package and install it on Linux, as documented in Step 1.

[Richard Garber](https://github.com/rgarber11) graciously contributed [the fix](https://github.com/nadimkobeissi/16iax10h-linux-sound-saga/issues/19#issuecomment-3594367397) for making the internal microphone work.

Sincere thanks to everyone who [pledged](PLEDGE.md) a reward for solving this problem. The reward goes to Lyapsus.
