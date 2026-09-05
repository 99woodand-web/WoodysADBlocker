# WoodysADBlocker
My homemade AD blocker 


WHAT IT DOES
------------
Blocks malware, ad and tracking domains at the DNS level for
the active Windows network adapter. Runs as a local DNS
server (127.0.0.1) and forwards allowed queries securely via
DNS-over-HTTPS, so it works even on networks that interfere
with normal DNS.

STARTING IT
-----------
1. Double-click start.bat
2. Click "Yes" on the Windows UAC prompt (admin is required
   to bind port 53 and change your DNS settings)
3. The control window opens within a second or two

   - Toggle "Protection ON" to start blocking
   - The window can be closed; protection keeps running
   - Double-click start.bat again later to re-open the window
   - To stop the app completely, click "Quit firewall" in the
     panel (or "Quit" on the dashboard) - it turns protection
     off, restores your DNS and exits. Closing the window only
     hides it; it does NOT stop the firewall

NOTE: this app never shows a console/command window - it runs
as a normal Windows program with only the small control panel
window (fixed 400x620 size). Starting it from a terminal
(cargo run, the exe path, etc.) also produces no text output.
If you want to see the live block stream, watch dns_blocks.log
or the dashboard instead (see below).

USING THE CONTROL PANEL
-----------------------
The small window has everything day-to-day: the on/off
toggle, Restore DNS, Mute alerts, Clear stats, blocking
mode, blocked-today stats and recent blocks. For the full
dashboard (7-day chart, allowlist, domain checker) open
http://127.0.0.1:7878 in a browser while the app is running.

KEY DETAILS
-----------
- Blocking modes: "Malware only" (safer default) and
  "Malware + ads/tracking" (full list, ~80k domains)
- Allowlist: domains you allow are never blocked, even if
  a list catches them (use the dashboard's Allowlist tools)
- Your custom icon: replace assets/app-icon.png with your
  own image (PNG recommended, transparency supported) and
  rebuild. The panel logo also updates live - just refresh.
- Blocked requests are logged to dns_blocks.log
- Daily stats are kept in dns_stats.json (last 30 days)
- Crash recovery: if a previous run left the adapter DNS pointed at
  127.0.0.1, the app repairs it automatically at startup - saved DNS is
  restored if a backup exists, otherwise it resets to automatic (DHCP) DNS

IF SOMETHING GOES WRONG
-----------------------
There is no console output anymore, so check these in order:

1. The panel window and the dashboard at
   http://127.0.0.1:7878 show live health: protection state,
   adapter, port 53 listener, DNS health, blocklist size
   and blocked counts. Open them first.
2. Blocked domains are logged to dns_blocks.log (with
timestamps) - open it in Notepad to see recent activity.
3. Daily stats live in dns_stats.json (last 30 days).
4. Click "Restore DNS" in the window to put your adapter
   DNS back to normal.
5. From an admin PowerShell you can also run:
     Set-DnsClientServerAddress -InterfaceAlias "WiFi" -ResetServerAddresses
6. If you can't start the app at all, check that nothing
   else is already using port 53 (other DNS tools) and that
   you approved the UAC prompt.

TIP: to make the app start with Windows, create a shortcut
to start.bat and drop it in the Startup folder:
  Win+R -> shell:startup -> paste the shortcut
