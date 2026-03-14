# Guide: Linux Audio on the Lenovo Legion Pro 7i Gen 10 (16IAX10H) using Cachyos Kernel

This guide explains how to get audio working correctly on the Lenovo Legion Pro 7i Gen 10 (**16IAX10H**). Since this solution is still very new, it will take some time for all components to be properly integrated into the Linux kernel. Until that happens, you can follow the steps below, which have been rigorously tested and are confirmed to work. This guide will be updated for future kernel versions as they are released, until the fix is fully integrated into the kernel.

1. open konsole or your favorite terminal and run the command git clone https://github.com/CachyOS/linux-cachyos then close terminal

2. Go to your home directory and enter the linux-cachyos folder, there is now a few kernel options for you to choose, generally use the linux-cachyos-bore folder or if you want the latest kernel use the linux-cachyos-rc folder and enter the folder of your choosing.

3.copy aw88399-legion.patch into the folder of the kernel type you will be building.

4. find the PKGBUILD file and open in your favorite text editor e.g kate

5. Select your kernel settings for your build, Here is what i chose if you would like to replicate NOTE THAT I WOULD NOT RECOMEND CHANGING SETTINGS IF YOU DO NOT KNOW WHAT YOURE DOING.

(### BUILD OPTIONS
# Set these variables to ANYTHING that is not null or choose proper variable to enable them

### Selecting CachyOS config
: "${_cachy_config:=yes}"

### Selecting the CPU scheduler
# ATTENTION - only one of the following values can be selected:
# 'bore' - select 'Burst-Oriented Response Enhancer'
# 'bmq' - select 'BMQ Scheduler'
# 'hardened' - select 'BORE Scheduler hardened' ## kernel with hardened config and hardening patches with the bore scheduler
# 'cachyos' - select 'CachyOS Default Scheduler (EEVDF)'
# 'eevdf' - select 'EEVDF Scheduler'
# 'rt' - select EEVDF, but includes a series of realtime patches
# 'rt-bore' - select Burst-Oriented Response Enhancer, but includes a series of realtime patches
: "${_cpusched:=bore}"

### Tweak kernel options prior to a build via nconfig
: "${_makenconfig:=no}"

### Tweak kernel options prior to a build via xconfig
: "${_makexconfig:=no}"

# Compile ONLY used modules to VASTLYreduce the number of modules built
# and the build time.
#
# To keep track of which modules are needed for your specific system/hardware,
# give module_db script a try: https://aur.archlinux.org/packages/modprobed-db
# This PKGBUILD read the database kept if it exists
#
# More at this wiki page ---> https://wiki.archlinux.org/index.php/Modprobed-db
: "${_localmodcfg:=no}"

# Path to the list of used modules
: "${_localmodcfg_path:="$HOME/.config/modprobed.db"}"

# Use the current kernel's .config file
# Enabling this option will use the .config of the RUNNING kernel rather than
# the ARCH defaults. Useful when the package gets updated and you already went
# through the trouble of customizing your config options.  NOT recommended when
# a new kernel is released, but again, convenient for package bumps.
: "${_use_current:=no}"

### Enable KBUILD_CFLAGS -O3
: "${_cc_harder:=yes}"

### Set performance governor as default
: "${_per_gov:=no}"

### Enable TCP_CONG_BBR3
: "${_tcp_bbr3:=no}"

### Running with a 1000HZ, 750Hz, 600 Hz, 500Hz, 300Hz, 250Hz and 100Hz tick rate
: "${_HZ_ticks:=1000}"

## Choose between perodic, idle or full
### Full tickless can give higher performances in various cases but, depending on hardware, lower consistency.
: "${_tickrate:=full}"

## Choose between full(low-latency), lazy, voluntary or none
: "${_preempt:=full}"

### Transparent Hugepages
# ATTENTION - one of two predefined values should be selected!
# 'always' - always enable THP
# 'madvise' - madvise, prevent applications from allocating more memory resources than necessary
# More infos here:
# https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/performance_tuning_guide/sect-red_hat_enterprise_linux-performance_tuning_guide-configuring_transparent_huge_pages
: "${_hugepage:=always}"

# CPU compiler optimizations - Defaults to native if left empty
# - "native" (use compiler autodetection)
# - "zen4" (Use znver4 compiler optimizations)
# - "generic" (kernel's default - to share the package between machines with different CPU µarch as long as they are x86-64)
: "${_processor_opt:=generic_v4}"

# Clang LTO mode, only available with the "llvm" compiler - options are "none", "full" or "thin".
# ATTENTION - one of three predefined values should be selected!
# "full: uses 1 thread for Linking, slow and uses more memory, theoretically with the highest performance gains."
# "thin: uses multiple threads, faster and uses less memory, may have a lower runtime performance than Full."
# "thin-dist: Similar to thin, but uses a distributed model rather than in-process: https://discourse.llvm.org/t/rfc-distributed-thinlto-build-for-kernel/85934"
# "none: disable LTO
: "${_use_llvm_lto:=full}"

# Use suffix -lto only when requested by the user
# yes - enable -lto suffix
# no - disable -lto suffix
# https://github.com/CachyOS/linux-cachyos/issues/36
: "${_use_lto_suffix:=no}"

# Use suffix -gcc when requested by the user
# Enabled by default to show the difference between LTO kernels and GCC kernels
: "${_use_gcc_suffix:=yes}"

# KCFI is a proposed forward-edge control-flow integrity scheme for
# Clang, which is more suitable for kernel use than the existing CFI
# scheme used by CONFIG_CFI_CLANG. kCFI doesn't require LTO, doesn't
# alter function references to point to a jump table, and won't break
# function address equality.
: "${_use_kcfi:=no}"

# Build the zfs module in to the kernel
# WARNING: The ZFS module doesn't build with selected RT sched due to licensing issues.
# If you use ZFS, refrain from building the RT kernel
: "${_build_zfs:=no}"

# Builds the open nvidia module and package it into a own base
# This does replace the requirement of nvidia-open-dkms
# Use this only if you have Turing+ GPU
: "${_build_nvidia_open:=no}"

# Builds the r8125 module and package it into its own package
# Replaces requirement for r8125-dkms
: "${_build_r8125:=no}"

# Build a debug package with non-stripped vmlinux
: "${_build_debug:=no}"

# Enable AUTOFDO_CLANG for the first compilation to create a kernel, which can be used for profiling
# Workflow:
# https://cachyos.org/blog/2411-kernel-autofdo/
# 1. Compile Kernel with _autofdo=yes and _build_debug=yes
# 2. Boot the kernel in QEMU or on your system, see Workload
# 3. Profile the kernel and convert the profile, see Generating the Profile for AutoFDO
# 4. Put the profile into the sourcedir
# 5. Run kernel build again with the _autofdo_profile_name path to profile specified
: "${_autofdo:=no}"

# Name for the AutoFDO profile
: "${_autofdo_profile_name:=}"

# Propeller should be applied, after the kernel is optimized with AutoFDO
# Workflow:
# 1. Proceed with above AutoFDO Optimization, but enable at the final compilation also _propeller
# 2. Boot into the AutoFDO Kernel and profile it
# 3. Convert the profile into the propeller profile, example:
# create_llvm_prof --binary=/usr/src/debug/linux-cachyos-rc/vmlinux --profile=propeller.data --format=propeller --propeller_output_module_name --out=propeller_cc_profile.txt --propeller_symorder=propeller_ld_profile.txt
# 4. Place the propeller_cc_profile.txt and propeller_ld_profile.txt into the srcdir
# 5. Enable _propeller_prefix
: "${_propeller:=no}"

# Enable this after the profiles have been generated
: "${_propeller_profiles:=no}")
)

6. find using control+F _patchsource="https://raw.githubusercontent.com/cachyos/kernel-patches/master/${_major}"

7. after lines  

"https://github.com/CachyOS/linux/releases/download/${_srctag}/${_srctag}.tar.gz"
    "config")
    
    add a line below and then add 
source+=("aw88399-legion.patch")

it should look similar to this:

_patchsource="https://raw.githubusercontent.com/cachyos/kernel-patches/master/${_major}"
_nv_ver=595.45.04
_nv_pkg="NVIDIA-Linux-x86_64-${_nv_ver}"
_nv_open_pkg="NVIDIA-kernel-module-source-${_nv_ver}"
source=(
    "https://github.com/CachyOS/linux/releases/download/${_srctag}/${_srctag}.tar.gz"
    "config")

source+=("aw88399-legion.patch")

# LLVM makedepends

8. find using control+F # Skip nvidia patches

this should allow you to see a chunk of code like this:

local src
    for src in "${source[@]}"; do
        src="${src%%::*}"
        # Skip nvidia patches
        [[ "$src" == "${_patchsource}"/misc/nvidia/*.patch ]] && continue
        src="${src##*/}"
        src="${src%.zst}"
        [[ $src = *.patch ]] || continue
        echo "Applying patch $src..."
        patch -Np1 < "../$src"
    done
    
9. highlight from local src to done and paste in the below text:

local src
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
        
10. save the file and exists

11. find the config file and open it in your text editor of choice e.g kate

12. scroll to the very bottom of the config file and paste the following code on the line under the last line of text:

CONFIG_SND_HDA_SCODEC_AW88399=m
CONFIG_SND_HDA_SCODEC_AW88399_I2C=m 
CONFIG_SND_SOC_AW88399=m 
CONFIG_SND_SOC_SOF_INTEL_TOPLEVEL=y 
CONFIG_SND_SOC_SOF_INTEL_COMMON=m 
CONFIG_SND_SOC_SOF_INTEL_MTL=m 
CONFIG_SND_SOC_SOF_INTEL_LNL=m


13. copy the file aw88399_acf.bin into the path /lib/firmware/
you can use terminal or just find the folder in your file browser and paste it.

14. copy the 2 files in fix/ucm2/ into the path /usr/share/alsa/ucm2/HDA/
you can use terminal or just find the folder in your file browser and paste it.

15. open terminal in the folder that you have chosen for the kernel build e.g linux-cachyos-rc

16. run makepkg --skipchecksums -sfi then patiently wait because this may take a while but dont worry and trust the process.

17. at the end it will ask you to put your password in to install the kernel, if it timed out because youre busy and decided to let the terminal run in the background dont worry simply run sudo pacman -U and drag and drop the two kernel tarbals e.g nulls0ng-cachyos-rc-7.0.rc3-1-x86_64.pkg.tar.zst and nulls0ng-cachyos-rc-headers-7.0.rc3-1-x86_64.pkg.tar.zst into the terminal and then enter and type your password.

18. (optional) if your nvidia drivers are not working do not fret sometimes the open nvidia dkms module is a pain in the ass simply run terminal and paste this in and hit enter and type your password etc etc

install 595 nvidia open dkms driver packages
    sudo pacman -U https://archive.cachyos.org/nvidia/595-beta/nvidia-open-dkms-595.45.04-1.1-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/nvidia-utils-595.45.04-1.1-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/opencl-nvidia-595.45.04-1.1-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/lib32-nvidia-utils-595.45.04-0-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/lib32-opencl-nvidia-595.45.04-0-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/libxnvctrl-595.45.04-1-x86_64.pkg.tar.zst https://archive.cachyos.org/nvidia/595-beta/nvidia-settings-595.45.04-1-x86_64.pkg.tar.zst
    
    
note this is for the 595 nvidia open dkms driver packages, this may be outdated in future so just find a later version of your nvidia dkms drivers and install manually and bob is indeed your uncle.

19. reboot and select your new kernel in your bootloader e.g grub and enjoy :)

## Disclaimer

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
