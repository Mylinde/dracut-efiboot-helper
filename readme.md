# Kernel Postinstall Script

**Overview**
------------

This script is executed after installing a new kernel to perform the necessary steps for setting up the initramfs and UEFI boot entries.

**Functionality**
-----------------

*   Create initramfs with `dracut` for the installed kernel
*   Generate UEFI executable files for the kernel and rescue mode
*   Register UEFI executable files as boot entries with `efibootmgr`

**Prerequisites**
-----------------

*   `dracut` must be installed and executable (`/usr/bin/dracut`)
*   `efibootmgr` must be installed
*   The kernel must be configured to use an initramfs (`INITRD` ≠ 'No')

**Execution Conditions**
-----------------------

*   The script will exit if:
	+ No kernel version is passed as a parameter
	+ The kernel does not require an initramfs (`INITRD` = 'No')
	+ `DEB_MAINT_PARAMS` is set and the first parameter is not "configure"

**Files and Directories**
-------------------------

*   **`/boot`**: Root directory for creating initramfs and UEFI files
*   **`/boot/efi/EFI/Linux`**: Target directory for UEFI executable files
*   **`/lib/modules/<Kernel-Version>`**: Directory for kernel module files

**Target Environment**
---------------------

*   **No Boot Manager**: This script is explicitly designed for systems that do **not use a boot manager** (e.g., GRUB, Syslinux)
*   **Direct UEFI Boot Configuration**: The script directly configures UEFI boot entries to load the kernel and rescue mode

**Notes**
----------

*   The script does not sign the executables for secureboot.
*   Ensure all required packages (especially `dracut` and `efibootmgr`) are installed and up-to-date.
*   Using this script in combination with a boot manager may lead to unexpected behavior. In such cases, prefer boot manager-specific configuration mechanisms.
*   This script is designed for use in Debian-based systems but can be ported to other distributions with minor adjustments.


# Kernel Postremoval Script

**Overview**
------------

This script is executed after removing a kernel package to perform the necessary cleanup steps for the initramfs and UEFI boot entries.

**Functionality**
-----------------

*   Remove the initramfs for the specified kernel version
*   Unregister the UEFI executable as a boot entry for the kernel and rescue mode
*   Delete the UEFI executable files for the kernel and rescue mode

**Files and Directories**
-------------------------

*   **`/boot`**: Root directory for initramfs files (default)
*   **`/boot/efi/EFI/Linux`**: Directory for UEFI executable files (default)
*   **`initrd.img-<Kernel-Version>`**: Initramfs file to be removed
*   **`linux-<Kernel-Version>.efi`** and **`linux-<Kernel-Version>-rescue.efi`**: UEFI executable files to be removed

**Target Environment**
---------------------

*   **Post-Removal Cleanup**: This script is designed to clean up after a kernel package removal.
*   **Kernel Package Management**: This script is intended for use with kernel package management systems (e.g., Debian's `dpkg`).

**Notes**
----------

*   This script assumes the UEFI boot entries were created by the corresponding Postinst script.
*   This script is designed for use in Debian-based systems but can be adapted for other distributions with minor adjustments.
