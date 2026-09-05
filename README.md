
WHAT IT DOES
------------
Blocks malware, advertising and tracking domains at the DNS
level for the active Windows network adapter. It runs a local
DNS server on 127.0.0.1 and forwards allowed queries securely
through DNS-over-HTTPS.

STARTING THE APP
----------------
1. Double-click rust_dns_firewall.exe.
2. Approve the Windows UAC prompt. Administrator permission is
   required to bind port 53 and change the adapter DNS settings.
3. The compact control panel opens after the blocklist loads.

No command window is required. This is a standalone executable;
there is no need to run cargo, open a terminal, or use a .bat
launcher.

USING THE COMPACT PANEL
-----------------------
The compact panel is the quick day-to-day control surface:

- Use the Protection switch to turn blocking on or off.
- The switch shows "Starting..." or "Stopping..." while the
  operation is being completed, so it does not appear to move
  back and forth during the DNS change.
- Restore DNS returns the adapter to its saved DNS settings or
  automatic DHCP DNS when no backup is available.
- Mute alerts disables the Windows blocked-domain notifications.
- Clear stats resets the current blocked-request counter.
- "Blocked today" gives a quick reference count and chart.
- The blocking mode defaults to "Malware + ads/tracking".
- The X in the top-right quits the firewall after confirmation.
  It turns protection off, restores DNS and exits the process.
- Closing the panel window only hides it; protection continues
  running in the background.

The compact panel intentionally does not show Recent blocks or
Tools. Open the full dashboard for those features.

FULL DASHBOARD
--------------
While the app is running, open:

  http://127.0.0.1:7878

The dashboard provides detailed health information, warnings,
daily statistics, recent blocked requests, allowlist management,
domain checking and the full set of controls.

BLOCKING MODES
--------------
- Malware + ads/tracking (default): uses the full StevenBlack
  list, containing approximately 80,000 domains.
- Malware only: uses the safer malware-focused feed and avoids
  blocking most advertising and tracking domains.

Changing mode downloads the selected blocklist before applying
it. The selected mode is saved for the next launch.

CUSTOM ICON
-----------
This single-file package contains the custom application icon
and panel logo that were present when it was built. No assets
folder is required.

LOGS AND DATA
-------------
The app stores these files beside the executable:

- dns_blocks.log     Timestamped blocked-domain log
- dns_stats.json     Daily statistics for the last 30 days
- dns_allowlist.txt  Domains exempt from blocking
- blocking_mode.txt  The selected blocking mode
- dns_settings_backup.json  Temporary adapter DNS backup while protection is on

TROUBLESHOOTING
---------------
There is no console window. Check the compact panel or the full
dashboard first. They show protection state, adapter details,
port 53 status, DNS health, blocklist size and live warnings.

If websites stop loading:

1. Click Restore DNS in the compact panel.
2. If the app is no longer running and Windows still shows
   127.0.0.1 as DNS, start the EXE again. Startup recovery will
   restore the saved DNS or reset the adapter to automatic DHCP
   DNS when no usable backup exists.
3. As a last resort, run this from an Administrator PowerShell,
   replacing WiFi with your actual adapter name:

   Set-DnsClientServerAddress -InterfaceAlias "WiFi" -ResetServerAddresses

If the app cannot start, verify that:

- You approved the UAC prompt.
- Another DNS program is not already using UDP port 53.
- Windows Application Control has not blocked the executable or
  one of its Rust/WebView2 components.
- Microsoft Edge WebView2 Runtime is installed for the native
  compact panel. The DNS service can still be checked through
  the browser dashboard if the native panel cannot open.

The app actively checks upstream DNS health in the background and
shows warnings in both the compact panel and the dashboard when
an upstream resolver or blocklist download is unavailable.

START WITH WINDOWS
------------------
Create a shortcut to rust_dns_firewall.exe and place it in the
Windows Startup folder:

  Win+R -> shell:startup -> paste the shortcut

Administrator approval may still be requested when Windows starts
the app.
