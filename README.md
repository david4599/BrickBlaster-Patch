# Brick Blaster Patch
Patch for Brick Blaster (Media Pocket French Version) to fix some crashes on Windows 7+.

<img src="img/BrickBlaster_logo.png" alt="Brick Blaster Logo">

## History and motivation
Brick Blaster (from Media Pocket) was a great part in my childhood and some years ago, I wanted to discover it again but each time I tried to launch the game on my former Windows 7 computer, it kept crashing and it doesn't work neither on Windows 10, even with the compatibility settings enabled and by using the [VDMSound Project](https://sourceforge.net/projects/vdmsound/).
Also, the game can't work in DosBox since it is a Win32 executable.

So the solutions at that time was to either have another machine on Windows XP or make a dualboot installation or create a Windows XP Virtual Machine to run the game.
It worked but I was not fully satisfied because I either needed to restart my computer to boot on Windows XP or start a VM that results in a loss of performance and a not great game experience (also the "Stretch" option of VMware to have the game in fullscreen mode was buggy).

But today, I came back to this project and I could finally find what was the crashing issues.

## Patches list
- BrickBlaster_patch_ev.1337: The first issue concerns the length of the environment variables. It seems that a limit of 2048 (0x800) characters is defined for the counting loop and the game exits if it doesn't find any null dword (end of the list) in that interval. On a fresh Windows install, this should not be a problem but when installing many programs, the environment variables list grows and the limit can be exceeded. The simple fix is to set an arbitrary higher number e.g. 32768 (0x8000).
- BrickBlaster_patch_io.1337: Another issue is about some I/O instructions that throw a Privileged Instruction Exception (0xc0000096). <del>Delete them solve the issue and it appears to have no negative effects on it.</del> It seems setting the "Windows 95" compatibility mode solve this issue without removing these instructions but try this patch if the mode doesn't work.

## Patching the game
/!\ Before doing any modifications, make sure to create a backup of the original "blaster.exe" file and check its SHA256 hash to avoid bad patches. It should be C0FC698A0C928127DB285138F3FDC4E2EDAFD9B6A053A92C0ECC44C5481ACF06 (French Version).

- Download and install x64dbg debugger and launch x32dbg.
- Load the "blaster.exe" file inside the debugger.
- In the View tab, click on "Patch file..." and import the file "BrickBlaster_patch_ev.1337" (and/or "BrickBlaster_patch_io.1337").
- The patches can now be saved to the "blaster.exe" file with the "Patch File" button.

For more convinience, patched versions are already available in the "Releases" section:
- "BrickBlaster_patch_ev.zip" for the environment variables issue (blaster.exe SHA256: A2E5ABCE58A7AF7E86E76AA43F87CD251BACF9FC6B76B52A9155C5D61167E9D3).
- "BrickBlaster_patch_ev_io.zip" for the environment variables and I/O instructions issues (blaster.exe SHA256: C9FC62B55159B333135A0A28EF6387C2066300BD167AE2633E94BB6D1AB35737).

When patching, the filename must remain the same i.e. "blaster.exe" otherwise the game cannot start.

## Notes
- The patches are tested and working on Windows 10 64-bit and Windows 7 32-bit. Other modern Windows versions should work as well.
- The patched game seems to crash inside a Windows 7+ Virtual Machine (VMware) probably due to a conflit with the generic VMware display driver.
- Compatibility settings (256 colors and 640 x 480 screen resolution options) and VDMSound may be required in certain circumstances, try using them if the game doesn't work well.
- The game was launched in 1999 and can't be found easily without having the original CD-ROM. I assume it can be considered as an abandonware today and can be distributed freely so I share it here though an [ISO file](https://www.dropbox.com/s/91b3xgbr1c1e86v/Brick%20Blaster.iso?dl=1).

#
david4599 - 2021