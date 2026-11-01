# iMac 2009 — Monterey UltraLite (experimental no-USB workflow)

Hardware: iMac10,1, Core 2 Duo, 4 GB RAM, ATI Radeon HD 4670, mechanical HDD. The existing macOS Sierra 10.12.6 must remain available for the working Target Display Mode.

**Status — 2026-11-01:** Monterey installer downloaded to Sierra; OCLP installed; Sierra shrunk to 700 GB; Recovery partition preserved; approximately 299 GB of unallocated GPT space. Partition-v5 has been staged, NOT yet run successfully in Recovery. No-USB OpenCore installer boot and Monterey installation are **unverified on this exact machine**.

## Open from Recovery

1. Boot local Recovery with Command-R. Connect Wi-Fi via menu if available, or Ethernet.
2. Open Utilities > Terminal.
3. Test with: curl -I https://raw.githubusercontent.com
4. Download and read the short instructions:

    curl -fL --retry 2 -o /tmp/START-HERE.txt https://raw.githubusercontent.com/Ricardot14/ricardot14-github.io/master/imac-2009-monterey/RECOVERY-START-HERE.txt
    cat /tmp/START-HERE.txt

**Do not pipe remote download directly to Bash for disk-modifying scripts.** Download first, inspect, and execute deliberately.

## Files

- [RECOVERY-START-HERE.txt](RECOVERY-START-HERE.txt) — plain-text Recovery instructions and precautions.
- [Monterey-UltraLite-Partition-v5.txt](Monterey-UltraLite-Partition-v5.txt) — existing experimental Recovery-only GPT partition script.
- [Monterey-UltraLite-OFFLINE-v6.txt](Monterey-UltraLite-OFFLINE-v6.txt) — existing staged multi-phase controller.
- [Monterey-UltraLite-OFFLINE-GUIDE-v6.txt](Monterey-UltraLite-OFFLINE-GUIDE-v6.txt) — long offline runbook.

These are existing project artifacts shared as **experimental/reference material**; syntax checks do not prove real-hardware success. The OCLP and Apple installation stages require manual boot and destination selection.

## Critical safety boundaries

- **No independent backup exists**. GPT writes on the only internal HDD can destroy or make Sierra unbootable.
- EFI is disk0s1, Sierra disk0s2 (700 GB), Recovery currently BSD disk0s4 (GPT index 3). Recheck the GPT table before any write.
- Never use diskutil eraseDisk disk0, gpt force (-f), forced unmount, or the old v4.1 provision command.
- If any disk operation or geometry check fails, STOP; do not automatically retry.
- Shared EFI/bootloader changes may affect Sierra. Native Target Display Mode on Monterey is not yet restored or validated.
- AirPlay 2 receiver, Handoff, Universal Control, Codex CLI and UxPlay are experiments, not installed or guaranteed by these scripts.
- Recovery Wi-Fi and HTTPS certificate trust can fail on old hardware; do not use curl -k to bypass trust.

## Existing offline fallback

From Recovery, Sierra's shared v5 file can be copied after mounting disk0s2: /Volumes/Macintosh HD/Users/Shared/Monterey-UltraLite-Partition-v5.txt. The v6 file exists at /Users/Shared/Monterey-UltraLite-Offline-v6/Monterey-UltraLite-OFFLINE-v6.txt only if the earlier prepare command succeeded.

No personal logs, serials, Wi-Fi networks or credentials are published.
