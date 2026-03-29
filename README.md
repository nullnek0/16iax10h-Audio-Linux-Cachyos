DOWNLOAD THE REPO, READ THE GUIDE.MD FOR INSTRUCTIONS FOR BUILDING THE KERNEL AND ADDITIONAL STEPS TO GET AUDIO WORKING ON CACHYOS!

CREDITS TO CACHYOS DEV 1Naim FOR IMPLEMENTING A PATCH INTO CACHYOS KERNEL 7RC 4-2 AND NEWER - FOLLOW BELOW STEPS FOR IF YOU WANT TO FIX AUDIO ON CACHYOS.

1. DOWNLOAD REPO AND FIND aw88399_acf.bin IN THE FIRMWARE FOLDER AND COPY IT TO /lib/firmware/aw88399_acf.bin
2. COPY THE FILES FROM fix/ucm2/ AND PLACE IT INTO /usr/share/alsa/ucm2/HDA/ OVERRIDING THE ORIGINAL FILES
3. INSTALL THE KERNEL VIA THE CACHY KERNEL MANAGER LINUX 7 RC5 OR LATER
4. INCLUDE snd_intel_dspcfg.dsp_driver=3 WITHIN YOUR KERNEL BOOT PARAMETERS
5. REBOOT INTO YOUR NEW KERNEL
6. FIND YOUR LAPTOP VIA alsaucm listcards IN TERNMINAL
7. RUN COMMAND BUT YOU MAY NEED TO CHANGE THE NUMBER 0 ON ALL LINES TO MATCH THE HW ID OF YOUR LAPTOP
alsaucm -c hw:0 reset
alsaucm -c hw:0 reload
systemctl --user restart pipewire pipewire-pulse wireplumber
amixer sset -c 0 Master 100%
amixer sset -c 0 Headphone 100%
amixer sset -c 0 Speaker 100%
8. HAVE A DANCE PARTY USING YOUR LEGIONS SPEAKERS!!!

## ## Disclaimer

I, nullnek0, attest that all components of the fix provided here have been tested and work without any apparent harmful effects. The fix components are provided in good faith. However, I (as well as the main fix authors) disclaim all responsibility for any use of this fix and guide:

```
THE PROGRAM IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU. SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL NECESSARY SERVICING, REPAIR OR CORRECTION.
```

## Credits

I (nullnek0) modified the patch file to be non version specfic and adjusted the kernel patch to play nicely with newer kernel versions 19.6 onwards as the previous patch files done by Nadim Kobeissi are specific to kernel versions and are outdated.

Fixing this issue required weeks of intensive work from multiple people.

Approximately 95% of the engineering work was done by [Lyapsus](https://github.com/Lyapsus). Lyapsus improved an incomplete kernel driver, wrote new kernel codecs and side-codecs, and contributed much more. I want to emphasize his incredible kindness and dedication to solving this issue. He is the primary force behind this fix, and without him, it would never have been possible.

([Nadim Kobeissi](https://nadim.computer)) conducted the initial investigation that identified the missing components needed for audio to work on the 16IAX10H on Linux. Building on what I learned from Lyapsus's work, I helped debug and clean up his kernel code, tested it, and made minor improvements. I also contributed the solution to the volume control issue documented in Step 8, and wrote this guide.

Gergo K. showed me how to extract the AW88399 firmware from the Windows driver package and install it on Linux, as documented in Step 1.

[Richard Garber](https://github.com/rgarber11) graciously contributed [the fix](https://github.com/nadimkobeissi/16iax10h-linux-sound-saga/issues/19#issuecomment-3594367397) for making the internal microphone work.

Sincere thanks to everyone who [pledged](PLEDGE.md) a reward for solving this problem. The reward goes to Lyapsus.

I, nullnek0, attest that all components of the fix provided here have been tested and work without any apparent harmful effects. The fix components are provided in good faith. However, I (as well as the main fix authors) disclaim all responsibility for any use of this fix and guide:

```
THE PROGRAM IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU. SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL NECESSARY SERVICING, REPAIR OR CORRECTION.
```

## Credits

I (nullnek0) modified the patch file to be non version specfic and adjusted the kernel patch to play nicely with newer kernel versions 19.6 onwards as the previous patch files done by Nadim Kobeissi are specific to kernel versions and are outdated.

Fixing this issue required weeks of intensive work from multiple people.

Approximately 95% of the engineering work was done by [Lyapsus](https://github.com/Lyapsus). Lyapsus improved an incomplete kernel driver, wrote new kernel codecs and side-codecs, and contributed much more. I want to emphasize his incredible kindness and dedication to solving this issue. He is the primary force behind this fix, and without him, it would never have been possible.

([Nadim Kobeissi](https://nadim.computer)) conducted the initial investigation that identified the missing components needed for audio to work on the 16IAX10H on Linux. Building on what I learned from Lyapsus's work, I helped debug and clean up his kernel code, tested it, and made minor improvements. I also contributed the solution to the volume control issue documented in Step 8, and wrote this guide.

Gergo K. showed me how to extract the AW88399 firmware from the Windows driver package and install it on Linux, as documented in Step 1.

[Richard Garber](https://github.com/rgarber11) graciously contributed [the fix](https://github.com/nadimkobeissi/16iax10h-linux-sound-saga/issues/19#issuecomment-3594367397) for making the internal microphone work.

Sincere thanks to everyone who [pledged](PLEDGE.md) a reward for solving this problem. The reward goes to Lyapsus.
