MONTEREY ULTRALITE — RECOVERY START HERE
Version 2026-11-01. Public, read-only GitHub download instructions.
iMac10,1 / Sierra 10.12.6 / existing 1 TB disk / NO external backup.

IMPORTANT
---------
The existing Sierra system is the only proven Target Display Mode configuration.
The no-USB Monterey path and restoration of Target Display Mode on Monterey are
experimental. Public scripts are not an assurance of safe installation.
Keep the internal disk intact unless you accept the possibility of losing Sierra.

1. ENTER RECOVERY / GET NETWORK
--------------------------------
Hold Command-R while starting the iMac. Use Utilities > Terminal.
Connect Wi-Fi using the menu if present; Ethernet may be more reliable.

Test secure networking:
    curl -I https://raw.githubusercontent.com

If TLS/certificate verification or DNS fails, STOP network retrieval.
Do not bypass security with curl -k, and do not enter GitHub passwords/tokens.

2. DOWNLOAD PUBLISHED FILES TO TEMPORARY RECOVERY STORAGE
----------------------------------------------------------
Run these commands, one line at a time:

    BASE=https://raw.githubusercontent.com/Ricardot14/ricardot14-github.io/master/imac-2009-monterey
    curl -fL --retry 2 "$BASE/Monterey-UltraLite-Partition-v5.txt" -o /tmp/mlu-v5.txt
    curl -fL --retry 2 "$BASE/Monterey-UltraLite-OFFLINE-v6.txt" -o /tmp/mlu-v6.txt
    curl -fL --retry 2 "$BASE/Monterey-UltraLite-OFFLINE-GUIDE-v6.txt" -o /tmp/mlu-guide.txt
    ls -lh /tmp/mlu-v5.txt /tmp/mlu-v6.txt /tmp/mlu-guide.txt
    cat /tmp/mlu-guide.txt

Files in /tmp disappear when Recovery restarts; they are NOT saved to Sierra.

3. IF YOU HAVE NO INTERNET: USE PREVIOUSLY STAGED LOCAL FILES
--------------------------------------------------------------
Sierra partition was disk0s2 and is expected to be called Macintosh HD.

    diskutil mount disk0s2
    ls "/Volumes/Macintosh HD/Users/Shared/Monterey-UltraLite-Partition-v5.txt"
    cp "/Volumes/Macintosh HD/Users/Shared/Monterey-UltraLite-Partition-v5.txt" /tmp/mlu-v5.txt

If the volume path differs, inspect ls /Volumes and STOP until you know
the correct path. Never erase a disk to make it mount.

4. READ-ONLY PRECHECK (NO PARTITIONS CHANGED)
---------------------------------------------
    diskutil list disk0
    gpt -r show disk0
    bash /tmp/mlu-v5.txt check

This exact-geometry script expects:
- GPT index 1: EFI at sector 40, size 409600
- GPT index 2: Sierra at 409640, size 1367187496
- GPT index 3: Recovery at 1367597136, size 1269536
- Free GPT region begins at 1368866672, size 584658463 sectors.

If these do not match, STOP. Do not run a disk write.

5. OPTIONAL, HIGH-RISK PARTITION CREATION
-----------------------------------------
Only if the precheck passes, you understand the backup risk, and you
have explicitly decided to proceed:

    bash /tmp/mlu-v5.txt provision

The script asks for PROVISION. It attempts to unmount disk0 without force.
It then adds installer and Monterey partition entries in the free region.
If busy, error, partial creation, or an unexpected geometry: STOP.
NEVER use gpt -f, diskutil eraseDisk disk0, or forced unmount.

Only continue after BOTH new GPT entries have been verified.

6. REBOOT INTO SIERRA; CONTINUE WITH THE OFFLINE GUIDE
-------------------------------------------------------
Do not format or erase anything in Recovery. Restart normally into Sierra.
Follow the phase called "BACK IN SIERRA" in /tmp/mlu-guide.txt
(write/save the guide before restarting because /tmp is temporary).

The original local offline guide should also be on Sierra Desktop if
v6 prepare was previously successful. If v6 prepare was not completed,
STOP before the next disk-writing phase.

GitHub files:
https://github.com/Ricardot14/ricardot14-github.io/tree/master/imac-2009-monterey

Remember: the initial setup does NOT install UxPlay, Codex, AirPlay 2 receiver,
Handoff, Universal Control, or working Monterey Target Display Mode.
