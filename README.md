# Guide: Linux Audio on the Lenovo Legion Pro 7i Gen 10 (16IAX10H)

This guide explains how to get audio working correctly on the Lenovo Legion Pro 7i Gen 10 (**16IAX10H**). Since this solution is still very new, it will take some time for all components to be properly integrated into the Linux kernel. Until that happens, you can follow the steps below, which have been rigorously tested and are confirmed to work. This guide will be updated for future kernel versions as they are released, until the fix is fully integrated into the kernel.

2.cd linux-cachyos-bore

3.copy aw88399-legion.patch into this path

4.add patch code into PKGBUILD , like:

existing----->        local src
        for src in "${source[@]}"; do
            src="${src%%::*}"
            # Skip nvidia patches
            [[ "$src" == "${_patchsource}"/misc/nvidia/*.patch ]] && continue
            src="${src##*/}"
            src="${src%.zst}"
            [[ $src = *.patch ]] || continue
            
            # Skip our custom patch so we can apply it manually with fuzz
            [[ "$src" == "aw88399-legion.patch" ]] && continue

            echo "Applying patch $src..."
            patch -Np1 < "../$src"
        done

        # Apply the AW88399 Legion patch with fuzz allowed
        echo "Applying patch aw88399-legion.patch with fuzz..."
        patch -Np1 --fuzz=3 -i "$startdir/aw88399-legion.patch"

# LLVM makedepends
if _is_lto_kernel; then
5.add audio config to the end of config, like:


CONFIG_SND_HDA_SCODEC_AW88399=m                
CONFIG_SND_HDA_SCODEC_AW88399_I2C=m
CONFIG_SND_SOC_AW88399=m
CONFIG_SND_SOC_SOF_INTEL_TOPLEVEL=y
CONFIG_SND_SOC_SOF_INTEL_COMMON=m
CONFIG_SND_SOC_SOF_INTEL_MTL=m
CONFIG_SND_SOC_SOF_INTEL_LNL=m                    

6.you can skip checksum to build kernel:

makepkg --skipchecksums -sfi

or fix checksum than build:

makepkg -sfi

7.waiting the build finish, and will auto install new kernel

8.update boot config & cpoy some .conf to /usr/share/alsa/ucm2/HDA/

9.reboot

10. install 595 nvidia open dkms driver packages
    sudo pacman -U https://archive.cachyos.org/nvidia/595-beta/nvidia-open-dkms-595.45.04-1.1-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/nvidia-utils-595.45.04-1.1-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/opencl-nvidia-595.45.04-1.1-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/lib32-nvidia-utils-595.45.04-0-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/lib32-opencl-nvidia-595.45.04-0-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/libxnvctrl-595.45.04-1-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/nvidia-settings-595.45.04-1-x86_64.pkg.tar.zst

11.reset audio card

alsaucm -c hw:1 reset
alsaucm -c hw:1 reload
systemctl --user restart pipewire pipewire-pulse wireplumber
amixer sset -c 1 Master 100% unmute
amixer sset -c 1 Headphone 100% unmute
amixer sset -c 1 Speaker 100% unmute

## Disclaimer

I, nullnek0, attest that all components of the fix provided here have been tested and work without any apparent harmful effects. The fix components are provided in good faith. However, I (as well as the main fix authors) disclaim all responsibility for any use of this fix and guide:

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
