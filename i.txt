#!/bin/sh
# Monterey installer network and recovery checks — READ ONLY
# No partition, erase, installer, network-setting, or EFI changes.
echo '=== Monterey installer / network diagnostic ==='
date
echo ''
echo '=== Connected interfaces (ethernet, other) ==='
ifconfig en0 2>&1 | grep -E '^(en0:|[[:space:]]*status:|[[:space:]]*inet )' || true
ifconfig en1 2>&1 | grep -E '^(en1:|[[:space:]]*status:|[[:space:]]*inet )' || true
echo ''
echo '=== Default route ==='
route -n get default 2>&1 | grep -E 'gateway:|interface:|not in table|Network is unreachable' || true
echo ''
echo '=== DNS servers ==='
scutil --dns 2>&1 | grep 'nameserver\[' | head -3 || true
echo ''
echo '=== HTTPS checks (HTTP 200/301/403/404 all mean server responded) ==='
for URL in https://www.apple.com/ https://osrecovery.apple.com/ https://mesu.apple.com/ https://raw.githubusercontent.com/Ricardot14/ricardot14.github.io/master/i.txt
do
  echo "Checking $URL"
  curl -L -sS --connect-timeout 7 --max-time 17 -o /dev/null -w 'HTTP %{http_code}\n' "$URL" 2>&1 || echo "curl failed (see message above)"
done
echo ''
echo '=== Installer volume (read only) ==='
diskutil info '/Volumes/MLU-Monterey' 2>&1 | grep -E 'Volume Name:|Mounted:|Mount Point:|File System Personality:|Disk Size:|Volume Total Space:' || true
echo ''
echo 'NO DISK OR NETWORK SETTINGS CHANGED.'
