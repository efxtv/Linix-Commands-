# Linux & Windows Browsers - Complete Reference Guide

> Comprehensive list of all browsers supported on Linux and Windows with their configuration paths

---

## Table of Contents
1. [Linux - Chromium-Based Browsers](#linux---chromium-based-browsers)
2. [Linux - Firefox-Based Browsers](#linux---firefox-based-browsers)
3. [Linux - Native Browsers](#linux---native-browsers)
4. [Windows - Chromium-Based Browsers](#windows---chromium-based-browsers)
5. [Windows - Firefox-Based Browsers](#windows---firefox-based-browsers)
6. [Windows - Microsoft Edge & Legacy](#windows---microsoft-edge--legacy)
7. [Windows - Other Browsers](#windows---other-browsers)
8. [Android - Chromium-Based Browsers](#android---chromium-based-browsers)
9. [Android - Session Files](#android---session-files-inside-apk-data)
10. [Android - ADB Commands](#android---extracting-browser-data-adb-commands)
11. [Android - SQLite Queries](#android---sqlite-database-queries)
12. [Android - Package Names](#android---complete-package-name-reference)
13. [Quick Reference Commands](#quick-reference-commands)
14. [Session File Locations](#session-file-locations)
15. [Cross-Platform Reference](#cross-platform-reference)

---

# LINUX BROWSERS

---

## Linux - Chromium-Based Browsers

| Browser | Config Path | Session Files |
|---------|-------------|---------------|
| **Google Chrome** | `~/.config/google-chrome/` | Sessions/, History, Cookies |
| **Chromium** | `~/.config/chromium/` | Sessions/, History, Cookies |
| **Brave** | `~/.config/BraveSoftware/Brave-Browser/` | Sessions/, History, Cookies |
| **Vivaldi** | `~/.config/vivaldi/` | Sessions/, History, Cookies |
| **Microsoft Edge** | `~/.config/microsoft-edge/` | Sessions/, History, Cookies |
| **Opera** | `~/.config/opera/` | Sessions/, History, Cookies |
| **Opera GX** | `~/.config/opera-gx/` | Sessions/, History, Cookies |
| **Maxthon** | `~/.config/maxthon/` | Sessions/, History, Cookies |
| **SlimJet** | `~/.config/slimjet/` | Sessions/, History, Cookies |
| **Comodo Dragon** | `~/.config/comodo-dragon/` | Sessions/, History, Cookies |
| **UC Browser** | `~/.config/ucbrowser/` | Sessions/, History, Cookies |
| **CentBrowser** | `~/.config/centbrowser/` | Sessions/, History, Cookies |
| **Torch** | `~/.config/torch/` | Sessions/, History, Cookies |
| **Avast Browser** | `~/.config/avast/browser/` | Sessions/, History, Cookies |
| **Epic Browser** | `~/.config/epic/EpicBrowser/Chrome/` | Sessions/, History, Cookies |
| **Avira Browser** | `~/.config/avira/` | Sessions/, History, Cookies |
| **Samsung Browser** | `~/.config/samsung/` | Sessions/, History, Cookies |
| **Iron Browser** | `~/.config/iron/` | Sessions/, History, Cookies |
| **SuperBird** | `~/.config/superbird/` | Sessions/, History, Cookies |
| **Beaker Browser** | `~/.config/beaker/` | Sessions/, History, Cookies |

---

## Linux - Firefox-Based Browsers

| Browser | Config Path | Session Files |
|---------|-------------|---------------|
| **Firefox** | `~/.mozilla/firefox/` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Firefox ESR** | `~/.mozilla/firefox/` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Firefox Dev Edition** | `~/.mozilla/firefox/` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Firefox Nightly** | `~/.mozilla/firefox/` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Firefox Beta** | `~/.mozilla/firefox/` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Firefox 32-bit** | `~/.mozilla/firefox/` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Waterfox** | `~/.waterfox/` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Waterfox G4** | `~/.waterfox/` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Pale Moon** | `~/.moonchild productions/pale moon/` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Tor Browser** | `~/.torbrowser/Data/profile/` | `*.jsonlz4`, `recovery.jsonlz4` |
| **SeaMonkey** | `~/.mozilla/seamonkey/` | `sessionstore.jsonlz4` |
| **IceCat** | `~/.gnuweb/icecat/` | `sessionstore.jsonlz4` |
| **Basilisk** | `~/.basilisk/` | `sessionstore.jsonlz4` |
| **Cyberfox** | `~/.cyberfox/` | `sessionstore.jsonlz4` |
| **K-Meleon** | `~/.k-meleon/` | `sessionstore.jsonlz4` |
| **Comodo IceDragon** | `~/.icedragon/` | `sessionstore.jsonlz4` |
| **GNU IceCat** | `~/.gnuweb/icecat/` | `sessionstore.jsonlz4` |
| **Pale Moon (Goanna)** | `~/.moonchild productions/pale moon/` | `sessionstore.jsonlz4` |

---

## Linux - Native Browsers

| Browser | Config Path | Session/History Files |
|---------|-------------|----------------------|
| **Falkon** | `~/.config/falkon/` | profiles/, sessions/ |
| **QupZilla** | `~/.config/qupzilla/` | profiles/, sessions/ |
| **GNOME Web (Epiphany)** | `~/.config/epiphany/` | ephy-history.db, webapp.db |
| **Konqueror** | `~/.config/konquerorrc` | konq_shellrc |
| **Midori** | `~/.config/midori/` | databases/, history.db |
| **Otter Browser** | `~/.config/otter/` | sessions/, cookies.db |
| **Luakit** | `~/.config/luakit/` | bookmarks.lua, history.db |
| **Surf** | `~/.surf/` | cookies.txt, history.txt |
| **NetSurf** | `~/.netsurf/` | cookies, history |
| **Nyxt** | `~/.config/nyxt/` | history.db, bookmarks.db |
| **Dooble** | `~/.config/dooble/` | history.db, cookies.db |
| **Links2** | `~/.links2/` | bookmarks.txt |
| **Lynx** | `~/.lynx/` | lynx_bookmarks.html |
| **W3M** | `~/.w3m/` | bookmark.html |
| **QuteBrowser** | `~/.config/qutebrowser/` | history.sqlite, bookmarks.db |
| **Oddly** | `~/.config/oddly/` | history.db |
| **Hermes** | `~/.config/hermes/` | history.db |
| **Dooble** | `~/.config/dooble/` | history.db, cookies.db |
| **Mercury Browser** | `~/.config/mercury/` | history.db |
| **KDE Dolpin** | `~/.local/share/dolphin/` | session files |

---

# WINDOWS BROWSERS

---

## Windows - Chromium-Based Browsers

| Browser | Config Path | Session Files |
|---------|-------------|---------------|
| **Google Chrome** | `%LOCALAPPDATA%\Google\Chrome\User Data\` | Sessions/, History, Cookies |
| **Chrome Beta** | `%LOCALAPPDATA%\Google\Chrome Beta\User Data\` | Sessions/, History, Cookies |
| **Chrome Dev** | `%LOCALAPPDATA%\Google\Chrome Dev\User Data\` | Sessions/, History, Cookies |
| **Chrome Canary** | `%LOCALAPPDATA%\Google\Chrome SxS\User Data\` | Sessions/, History, Cookies |
| **Chromium** | `%LOCALAPPDATA%\Chromium\User Data\` | Sessions/, History, Cookies |
| **Brave** | `%LOCALAPPDATA%\BraveSoftware\Brave-Browser\User Data\` | Sessions/, History, Cookies |
| **Vivaldi** | `%LOCALAPPDATA%\Vivaldi\User Data\` | Sessions/, History, Cookies |
| **Opera** | `%APPDATA%\Opera Software\Opera Stable\` | Sessions/, History, Cookies |
| **Opera GX** | `%APPDATA%\Opera Software\Opera GX Stable\` | Sessions/, History, Cookies |
| **Opera Beta** | `%APPDATA%\Opera Software\Opera Beta\` | Sessions/, History, Cookies |
| **Opera Developer** | `%APPDATA%\Opera Software\Opera Developer\` | Sessions/, History, Cookies |
| **Maxthon** | `%LOCALAPPDATA%\Maxthon\Application\User Data\` | Sessions/, History, Cookies |
| **UC Browser** | `%LOCALAPPDATA%\UCBrowser\` | Sessions/, History, Cookies |
| **UC Browser Mini** | `%LOCALAPPDATA%\UCBrowserMini\` | Sessions/, History, Cookies |
| **Comodo Dragon** | `%LOCALAPPDATA%\Comodo\Dragon\` | Sessions/, History, Cookies |
| **Comodo IceDragon** | `%LOCALAPPDATA%\Comodo\IceDragon\` | Sessions/, History, Cookies |
| **Torch Browser** | `%LOCALAPPDATA%\Torch\` | Sessions/, History, Cookies |
| **CentBrowser** | `%LOCALAPPDATA%\CentBrowser\` | Sessions/, History, Cookies |
| **Slimjet** | `%LOCALAPPDATA%\Slimjet\` | Sessions/, History, Cookies |
| **SRWare Iron** | `%LOCALAPPDATA%\SRWare Iron\` | Sessions/, History, Cookies |
| **SuperBird** | `%LOCALAPPDATA%\SuperBird\` | Sessions/, History, Cookies |
| **Browzar** | `%LOCALAPPDATA%\Browzar\` | Sessions/, History, Cookies |
| **CoolNovo** | `%LOCALAPPDATA%\MapleStudio\ChromePlus\` | Sessions/, History, Cookies |
| **Rockmelts** | `%LOCALAPPDATA%\RockMelt\` | Sessions/, History, Cookies |
| **Flock** | `%LOCALAPPDATA%\Flock\` | Sessions/, History, Cookies |
| **Titan Browser** | `%LOCALAPPDATA%\Titan\` | Sessions/, History, Cookies |
| **Epic Browser** | `%LOCALAPPDATA%\Epic Privacy Browser\` | Sessions/, History, Cookies |
| **Avast Secure Browser** | `%LOCALAPPDATA%\Avast Software\Avast Secure Browser\` | Sessions/, History, Cookies |
| **AVG Secure Browser** | `%LOCALAPPDATA%\AVG\AVG Secure Browser\` | Sessions/, History, Cookies |
| **Kaspersky Secure Browser** | `%LOCALAPPDATA%\KasperskyLab\` | Sessions/, History, Cookies |
| **CCleaner Browser** | `%LOCALAPPDATA%\Ccleaner\CCleaner Browser\` | Sessions/, History, Cookies |
| **Norton Safe Web** | `%LOCALAPPDATA%\Norton\Safeweb\` | Sessions/, History, Cookies |
| **360 Secure Browser** | `%LOCALAPPDATA%\360Safe\` | Sessions/, History, Cookies |
| **QQ Browser** | `%LOCALAPPDATA%\Tencent\QQBrowser\` | Sessions/, History, Cookies |
| **Sogou Browser** | `%LOCALAPPDATA%\SogouExplorer\` | Sessions/, History, Cookies |
| **Baidu Browser** | `%LOCALAPPDATA%\Baidu\BaiduBrowser\` | Sessions/, History, Cookies |
| **Baidu Spark** | `%LOCALAPPDATA%\baidu\Spark` | Sessions/, History, Cookies |
| **猎豹浏览器** | `%LOCALAPPDATA%\liebao` | Sessions/, History, Cookies |
| **2345 Browser** | `%LOCALAPPDATA%\2345Explorer` | Sessions/, History, Cookies |
| **太平洋浏览器** | `%LOCALAPPDATA%\PCTools\PCTools Browser` | Sessions/, History, Cookies |

---

## Windows - Firefox-Based Browsers

| Browser | Config Path | Session Files |
|---------|-------------|---------------|
| **Firefox** | `%APPDATA%\Mozilla\Firefox\Profiles\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Firefox ESR** | `%APPDATA%\Mozilla\Firefox\Profiles\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Firefox Dev Edition** | `%APPDATA%\Mozilla\Firefox\Profiles\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Firefox Nightly** | `%APPDATA%\Mozilla\Firefox\Profiles\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Firefox Beta** | `%APPDATA%\Mozilla\Firefox\Profiles\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Firefox 32-bit** | `%APPDATA%\Mozilla\Firefox\Profiles\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Waterfox** | `%APPDATA%\Waterfox\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Waterfox G4** | `%APPDATA%\Waterfox\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Pale Moon** | `%APPDATA%\Moonchild Productions\Pale Moon\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Pale Moon (64-bit)** | `%LOCALAPPDATA%\Moonchild Productions\Pale Moon\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **SeaMonkey** | `%APPDATA%\Mozilla\SeaMonkey\Profiles\` | `sessionstore.jsonlz4` |
| **Basilisk** | `%APPDATA%\Basilisk\Profiles\` | `sessionstore.jsonlz4` |
| **CyberFox** | `%APPDATA\8pecxstudios\CyberFox\` | `sessionstore.jsonlz4` |
| **K-Meleon** | `%APPDATA%\K-Meleon\` | `sessionstore.jsonlz4` |
| **Comodo IceDragon** | `%LOCALAPPDATA%\Comodo\IceDragon\` | `sessionstore.jsonlz4` |
| **Tor Browser** | `%LOCALAPPDATA%\TorBrowser-Data\Browser\` | `*.jsonlz4`, `recovery.jsonlz4` |
| **Epic Browser** | `%LOCALAPPDATA%\Epic Privacy Browser\` | `sessionstore.jsonlz4` |
| **GNU IceCat** | `%APPDATA%\GNU IceCat\` | `sessionstore.jsonlz4` |

---

## Windows - Microsoft Edge & Legacy

| Browser | Config Path | Session Files |
|---------|-------------|---------------|
| **Microsoft Edge (Chromium)** | `%LOCALAPPDATA%\Microsoft\Edge\User Data\` | Sessions/, History, Cookies |
| **Microsoft Edge Beta** | `%LOCALAPPDATA%\Microsoft\Edge Beta\User Data\` | Sessions/, History, Cookies |
| **Microsoft Edge Dev** | `%LOCALAPPDATA%\Microsoft\Edge Dev\User Data\` | Sessions/, History, Cookies |
| **Microsoft Edge Canary** | `%LOCALAPPDATA%\Microsoft\Edge SxS\User Data\` | Sessions/, History, Cookies |
| **Internet Explorer** | `%APPDATA%\Microsoft\Windows\Cookies\` | Cookie files |
| **IE Favorites** | `%USERPROFILE%\Favorites\` | Bookmark files |
| **IE Cache** | `%LOCALAPPDATA%\Microsoft\Windows\INetCache\` | Temporary files |
| **IE Session** | `%APPDATA%\Microsoft\Windows\` | Session state |

---

## Windows - Other Browsers

| Browser | Config Path | Session Files |
|---------|-------------|---------------|
| **Safari (Windows)** | `%APPDATA%\Apple Computer\Safari\` | Bookmarks, History |
| **Avant Browser** | `%APPDATA%\AvantBrowser\` | Sessions, Bookmarks |
| **TheWorld Browser** | `%APPDATA%\TheWorld\` | Sessions, Bookmarks |
| **Maxthon (old)** | `%APPDATA%\Maxthon\` | Sessions, Bookmarks |
| **Sleipnir** | `%APPDATA%\Fenrir Inc.\Sleipnir\` | Sessions, Bookmarks |
| **Sleipnir (Chromium)** | `%LOCALAPPDATA%\Fenrir Inc\Sleipnir\` | Sessions, Bookmarks |
| **Lunascape** | `%APPDATA%\Lunascape\` | Sessions, Bookmarks |
| **Dooble** | `%APPDATA%\Dooble\` | history.db, cookies.db |
| **QuteBrowser** | `%APPDATA%\qutebrowser\` | history.sqlite, bookmarks.db |
| **Otter Browser** | `%APPDATA%\otter-browser\` | sessions/, cookies.db |
| **Falkon** | `%APPDATA%\Falkon\` | profiles/, sessions/ |
| **360 Secure Browser** | `%LOCALAPPDATA%\360Safe\safeguard\` | Sessions, History |
| **Liebao Browser** | `%LOCALAPPDATA%\liebao\UserData\` | Sessions, History |
| **Redcore Browser** | `%LOCALAPPDATA%\Redcore\` | Sessions, History |
| **Sogou Explorer** | `%APPDATA%\SogouExplorer\` | Sessions, History |

---

## Windows - OneDrive & Roaming Profiles

| Path Type | Environment Variable |
|-----------|----------------------|
| Local AppData | `%LOCALAPPDATA%` → `C:\Users\<user>\AppData\Local` |
| Roaming AppData | `%APPDATA%` → `C:\Users\<user>\AppData\Roaming` |
| User Profile | `%USERPROFILE%` → `C:\Users\<user>` |
| Home | `%HOME%` → `C:\Users\<user>` |

---

# QUICK REFERENCE COMMANDS

---

## Linux Commands

### Find All Browser Binaries
```bash
which firefox chrome chromium brave vivaldi opera edge falkon epiphany midori konqueror waterfox palemoon seamonkey icecat nyxt dooble qutebrowser links2 lynx w3m
```

### Find All Browser Config Directories
```bash
ls -la ~/.config/ | grep -iE "(chrome|firefox|brave|vivaldi|opera|edge|falkon|epiphany|midori|qupzilla|nyxt|dooble)"
```

### Find Firefox Profiles
```bash
find ~/.mozilla/firefox -maxdepth 1 -type d -name "*.default*" -o -name "*.default-esr*" 2>/dev/null
```

### Find Chromium Profiles
```bash
find ~/.config -maxdepth 3 -type d \( -name "Default" -o -name "Profile*" \) 2>/dev/null | grep -iE "(chrome|chromium|brave|vivaldi|opera)"
```

### One-Liner: Find All Browser Paths
```bash
find ~/.config ~/.mozilla ~/.cache ~/.local/share ~/.var -maxdepth 5 2>/dev/null | grep -iE "(firefox|chrome|chromium|brave|vivaldi|opera|edge|falkon|epiphany|midori|konqueror|waterfox|palemoon|torbrowser|seamonkey|nyxt|dooble)" | grep -iE "(default|profile|default-release|default-esr|sessions)" | sort -u
```

---

## Windows Commands (PowerShell)

### Find All Browser Paths
```powershell
Get-ChildItem "$env:LOCALAPPDATA" -Directory | Where-Object { $_.Name -match "Chrome|Firefox|Brave|Vivaldi|Opera|Edge|Edge Beta" }
```

### Find Firefox Profiles
```powershell
Get-ChildItem "$env:APPDATA\Mozilla\Firefox\Profiles" -Directory
```

### Find Chromium Profiles
```powershell
Get-ChildItem "$env:LOCALAPPDATA\*\User Data" -Directory -ErrorAction SilentlyContinue
```

### Find Edge Profiles
```powershell
Get-ChildItem "$env:LOCALAPPDATA\Microsoft\Edge\User Data" -Directory
```

### One-Liner: Find All Browsers
```powershell
@("$env:LOCALAPPDATA\Google\Chrome\User Data","$env:LOCALAPPDATA\Microsoft\Edge\User Data","$env:APPDATA\Mozilla\Firefox\Profiles","$env:LOCALAPPDATA\BraveSoftware\Brave-Browser\User Data","$env:LOCALAPPDATA\Vivaldi\User Data","$env:APPDATA\Opera Software\Opera Stable") | Where-Object { Test-Path $_ }
```

---

# SESSION FILE LOCATIONS

---

## Firefox Session Files

### Linux
```
~/.mozilla/firefox/<profile>/sessionstore-backups/recovery.jsonlz4
~/.mozilla/firefox/<profile>/sessionstore-backups/previous.jsonlz4
~/.mozilla/firefox/<profile>/sessionstore.jsonlz4
~/.mozilla/firefox/<profile>/sessionstore.json
~/.cache/mozilla/firefox/<profile>/
```

### Windows
```
%APPDATA%\Mozilla\Firefox\Profiles\<profile>\sessionstore-backups\recovery.jsonlz4
%APPDATA%\Mozilla\Firefox\Profiles\<profile>\sessionstore-backups\previous.jsonlz4
%APPDATA%\Mozilla\Firefox\Profiles\<profile>\sessionstore.jsonlz4
%APPDATA%\Mozilla\Firefox\Profiles\<profile>\sessionstore.json
```

---

## Chromium Session Files

### Linux
```
~/.config/<browser>/Default/Sessions/
~/.config/<browser>/Default/History
~/.config/<browser>/Default/Cookies
~/.config/<browser>/Default/Network/Cookies
~/.config/<browser>/Default/History-journal
```

### Windows
```
%LOCALAPPDATA%\<browser>\User Data\Default\Sessions\
%LOCALAPPDATA%\<browser>\User Data\Default\History
%LOCALAPPDATA%\<browser>\User Data\Default\Cookies
%LOCALAPPDATA%\<browser>\User Data\Default\Network\Cookies
%LOCALAPPDATA%\<browser>\User Data\Default\History-journal
```

---

## Session File Formats

| Format | Browser | Decompression |
|--------|---------|---------------|
| `.jsonlz4` | Firefox | `mozLz40` header, LZ4 |
| `.baklz4` | Firefox | `mozLz40` header, LZ4 |
| `.json` | Various | Plain JSON |
| `.db` | Chromium | SQLite database |
| Binary | Chromium | Custom format |
| `.sqlite` | Various | SQLite database |

---

## Firefox (jsonlz4) Format
```
Header: 8 bytes "mozLz40\0"
Size:   4 bytes (little-endian)
Data:   LZ4 compressed JSON
```

---

## Chromium History (SQLite)

```sql
-- URLs Table
SELECT url, title, last_visit_time FROM urls;

-- Cookies Table
SELECT host, name, value FROM cookies WHERE name LIKE '%session%';

-- Downloads Table
SELECT tab_url, target_path FROM downloads;

-- Keywords Table
SELECT url, keyword FROM keywords;
```

---

# CROSS-PLATFORM REFERENCE

---

## Profile Paths Quick Reference

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                                    LINUX                                        │
├────────────────────────────────────────────────────────────────────────────────┤
│ Firefox          →  ~/.mozilla/firefox/<profile>/                              │
│ Firefox ESR      →  ~/.mozilla/firefox/<profile>.esr/                          │
│ Waterfox         →  ~/.waterfox/<profile>/                                      │
│ Pale Moon        →  ~/.moonchild productions/pale moon/<profile>/              │
│ Tor Browser      →  ~/.torbrowser/Data/profile/                                │
│ SeaMonkey        →  ~/.mozilla/seamonkey/<profile>/                              │
│ IceCat           →  ~/.gnuweb/icecat/<profile>/                                 │
│ Chrome           →  ~/.config/google-chrome/Default/                             │
│ Chromium         →  ~/.config/chromium/Default/                                 │
│ Brave            →  ~/.config/BraveSoftware/Brave-Browser/Default/             │
│ Vivaldi          →  ~/.config/vivaldi/Default/                                  │
│ Edge             →  ~/.config/microsoft-edge/Default/                           │
│ Opera            →  ~/.config/opera/Default/                                    │
│ Opera GX         →  ~/.config/opera-gx/Default/                                 │
│ Falkon           →  ~/.config/falkon/<profile>/                                 │
│ QupZilla         →  ~/.config/qupzilla/<profile>/                               │
│ GNOME Web        →  ~/.config/epiphany/<profile>/                               │
│ Midori           →  ~/.config/midori/                                            │
│ Nyxt             →  ~/.config/nyxt/<profile>/                                    │
│ QuteBrowser      →  ~/.config/qutebrowser/<profile>/                            │
├────────────────────────────────────────────────────────────────────────────────┤
│                                   WINDOWS                                       │
├────────────────────────────────────────────────────────────────────────────────┤
│ Firefox          →  %APPDATA%\Mozilla\Firefox\Profiles\<profile>\               │
│ Firefox ESR      →  %APPDATA%\Mozilla\Firefox\Profiles\<profile>.esr\            │
│ Waterfox         →  %APPDATA%\Waterfox\<profile>\                               │
│ Pale Moon        →  %APPDATA%\Moonchild Productions\Pale Moon\<profile>\         │
│ Tor Browser      →  %LOCALAPPDATA%\TorBrowser-Data\Browser\<profile>\            │
│ SeaMonkey        →  %APPDATA%\Mozilla\SeaMonkey\Profiles\<profile>\              │
│ Chrome           →  %LOCALAPPDATA%\Google\Chrome\User Data\Default\             │
│ Chrome Beta      →  %LOCALAPPDATA%\Google\Chrome Beta\User Data\Default\         │
│ Chromium         →  %LOCALAPPDATA%\Chromium\User Data\Default\                  │
│ Brave            →  %LOCALAPPDATA%\BraveSoftware\Brave-Browser\User Data\       │
│ Vivaldi          →  %LOCALAPPDATA%\Vivaldi\User Data\Default\                    │
│ Edge             →  %LOCALAPPDATA%\Microsoft\Edge\User Data\Default\             │
│ Edge Beta        →  %LOCALAPPDATA%\Microsoft\Edge Beta\User Data\Default\       │
│ Opera            →  %APPDATA%\Opera Software\Opera Stable\                      │
│ Opera GX         →  %APPDATA%\Opera Software\Opera GX Stable\                  │
│ Maxthon          →  %LOCALAPPDATA%\Maxthon\Application\User Data\                │
│ UC Browser       →  %LOCALAPPDATA%\UCBrowser\User Data\Default\                 │
│ Avast Browser    →  %LOCALAPPDATA%\Avast Software\Avast Secure Browser\         │
│ AVG Browser      →  %LOCALAPPDATA%\AVG\AVG Secure Browser\                       │
│ QQ Browser       →  %LOCALAPPDATA%\Tencent\QQBrowser\User Data\Default\        │
│ Sogou Browser    →  %LOCALAPPDATA%\SogouExplorer\User Data\Default\            │
│ Falkon           →  %APPDATA%\Falkon\<profile>\                                 │
│ Dooble           →  %APPDATA%\Dooble\<profile>\                                 │
└────────────────────────────────────────────────────────────────────────────────┘
```

---

## Supported Session Keywords

The parser extracts data containing these keywords:

```
user_session, session, session_id, sessionid, auth, token, csrf, 
xsrf, nonce, key, secret, bearer, access_token, refresh_token, 
api_key, apikey, sessionkey, _session, sid, sess, sessionData, 
sessionToken, remember_me, login_token, auth_token, csrf_token
```

---

## Windows Registry Browser Paths

```reg
; Find installed browsers
HKLM\SOFTWARE\Clients\StartMenuInternet
HKLM\SOFTWARE\WOW6432Node\Clients\StartMenuInternet
HKCU\SOFTWARE\Clients\StartMenuInternet
```

### PowerShell - List Installed Browsers
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Clients\StartMenuInternet\*" | Select-Object -Property "(default)", "(System.CurrentControlSet)\Services"
Get-ItemProperty "HKCU:\SOFTWARE\Microsoft\Windows\Shell\Associations\UrlAssociations\http\UserChoice" | Select-Object -Property ProgId
```

---

## Notes

- Browser profiles are stored in `profiles.ini` (Firefox) or as numbered directories (Chromium)
- Session files may be in `sessionstore-backups/` subdirectory
- Some browsers store history/cookies in SQLite `.db` files
- Tor Browser uses additional encryption layers
- Windows browsers may have multiple profiles
- `%LOCALAPPDATA%` = `C:\Users\<user>\AppData\Local`
- `%APPDATA%` = `C:\Users\<user>\AppData\Roaming`

---

*Generated for Browser Forensics & Session Analysis - Linux, Windows & Android*

---

# ANDROID BROWSERS

---

## Android Browser Storage Overview

On Android, browser data is stored in different locations depending on the browser type:

| Storage Type | Path | Description |
|--------------|------|-------------|
| **App Private** | `/data/data/<package>/` | Private app data (requires root) |
| **App External** | `/sdcard/Android/data/<package>/` | External storage |
| **User Data** | `/data/user/<uid>/<package>/` | Android 6+ multi-user |

### Key Paths
- **Rooted Devices**: `/data/data/com.android.chrome/`
- **App Data (Unrooted)**: `/sdcard/Android/data/<package>/`
- **ChromeADB**: Can use `adb backup` or `adb shell dumpsys`

---

## Android - Chromium-Based Browsers

| Browser | Package Name | Private Data Path | External Data Path |
|---------|-------------|-------------------|-------------------|
| **Chrome** | `com.android.chrome` | `/data/data/com.android.chrome/` | `/sdcard/Android/data/com.android.chrome/` |
| **Chrome Beta** | `com.chrome.beta` | `/data/data/com.chrome.beta/` | `/sdcard/Android/data/com.chrome.beta/` |
| **Chrome Dev** | `com.chrome.dev` | `/data/data/com.chrome.dev/` | `/sdcard/Android/data/com.chrome.dev/` |
| **Chrome Canary** | `com.chrome.canary` | `/data/data/com.chrome.canary/` | `/sdcard/Android/data/com.chrome.canary/` |
| **Chromium** | `org.chromium.chromeworld` | `/data/data/org.chromium.chromeworld/` | `/sdcard/Android/data/org.chromium.chromeworld/` |
| **Chromium (Official)** | `org.chromium.base` | `/data/data/org.chromium.base/` | `/sdcard/Android/data/org.chromium.base/` |
| **Brave** | `com.brave.browser` | `/data/data/com.brave.browser/` | `/sdcard/Android/data/com.brave.browser/` |
| **Brave Beta** | `com.brave.browser_beta` | `/data/data/com.brave.browser_beta/` | `/sdcard/Android/data/com.brave.browser_beta/` |
| **Brave Nightly** | `com.brave.browser_nightly` | `/data/data/com.brave.browser_nightly/` | `/sdcard/Android/data/com.brave.browser_nightly/` |
| **Vivaldi** | `com.vivaldi.browser` | `/data/data/com.vivaldi.browser/` | `/sdcard/Android/data/com.vivaldi.browser/` |
| **Vivaldi Beta** | `com.vivaldi.browser.beta` | `/data/data/com.vivaldi.browser.beta/` | `/sdcard/Android/data/com.vivaldi.browser.beta/` |
| **Opera** | `com.opera.browser` | `/data/data/com.opera.browser/` | `/sdcard/Android/data/com.opera.browser/` |
| **Opera Beta** | `com.opera.browser.beta` | `/data/data/com.opera.browser.beta/` | `/sdcard/Android/data/com.opera.browser.beta/` |
| **Opera GX** | `com.opera.browser.gx` | `/data/data/com.opera.browser.gx/` | `/sdcard/Android/data/com.opera.browser.gx/` |
| **Opera Touch** | `com.opera.browser.touch` | `/data/data/com.opera.browser.touch/` | `/sdcard/Android/data/com.opera.browser.touch/` |
| **Opera Mini** | `com.opera.mini.android` | `/data/data/com.opera.mini.android/` | `/sdcard/Android/data/com.opera.mini.android/` |
| **Microsoft Edge** | `com.microsoft.emmx` | `/data/data/com.microsoft.emmx/` | `/sdcard/Android/data/com.microsoft.emmx/` |
| **Microsoft Edge Beta** | `com.microsoft.emmx.beta` | `/data/data/com.microsoft.emmx.beta/` | `/sdcard/Android/data/com.microsoft.emmx.beta/` |
| **Microsoft Edge Dev** | `com.microsoft.emmx.dev` | `/data/data/com.microsoft.emmx.dev/` | `/sdcard/Android/data/com.microsoft.emmx.dev/` |
| **Firefox** | `org.mozilla.firefox` | `/data/data/org.mozilla.firefox/` | `/sdcard/Android/data/org.mozilla.firefox/` |
| **Firefox Beta** | `org.mozilla.firefox_beta` | `/data/data/org.mozilla.firefox_beta/` | `/sdcard/Android/data/org.mozilla.firefox_beta/` |
| **Firefox Nightly** | `org.mozilla.fenix` | `/data/data/org.mozilla.fenix/` | `/sdcard/Android/data/org.mozilla.fenix/` |
| **Firefox Focus** | `org.mozilla.focus` | `/data/data/org.mozilla.focus/` | `/sdcard/Android/data/org.mozilla.focus/` |
| **Firefox Klar** | `org.mozilla.klar` | `/data/data/org.mozilla.klar/` | `/sdcard/Android/data/org.mozilla.klar/` |
| **Firefox TV** | `org.mozilla.tv.firefox` | `/data/data/org.mozilla.tv.firefox/` | `/sdcard/Android/data/org.mozilla.tv.firefox/` |
| **Firefox Lite** | `org.mozilla.rocket` | `/data/data/org.mozilla.rocket/` | `/sdcard/Android/data/org.mozilla.rocket/` |
| **DuckDuckGo** | `com.duckduckgo.mobile.android` | `/data/data/com.duckduckgo.mobile.android/` | `/sdcard/Android/data/com.duckduckgo.mobile.android/` |
| **Bromite** | `org.bromite.cromite` | `/data/data/org.bromite.cromite/` | `/sdcard/Android/data/org.bromite.cromite/` |
| **Bromite SystemWebView** | `org.bromite.systemwebview` | `/data/data/org.bromite.systemwebview/` | `/sdcard/Android/data/org.bromite.systemwebview/` |
| **Kiwi Browser** | `com.kiwibrowser.browser` | `/data/data/com.kiwibrowser.browser/` | `/sdcard/Android/data/com.kiwibrowser.browser/` |
| **Kiwi Lite** | `com.kiwibrowser.lite` | `/data/data/com.kiwibrowser.lite/` | `/sdcard/Android/data/com.kiwibrowser.lite/` |
| **Yandex Browser** | `com.yandex.browser` | `/data/data/com.yandex.browser/` | `/sdcard/Android/data/com.yandex.browser/` |
| **Yandex Browser Beta** | `com.yandex.browser.beta` | `/data/data/com.yandex.browser.beta/` | `/sdcard/Android/data/com.yandex.browser.beta/` |
| **Yandex Browser Lite** | `com.yandex.browser.light` | `/data/data/com.yandex.browser.light/` | `/sdcard/Android/data/com.yandex.browser.light/` |
| **Samsung Internet** | `com.sec.android.app.sbrowser` | `/data/data/com.sec.android.app.sbrowser/` | `/sdcard/Android/data/com.sec.android.app.sbrowser/` |
| **Samsung Internet Beta** | `com.sec.android.app.sbrowser.beta` | `/data/data/com.sec.android.app.sbrowser.beta/` | `/sdcard/Android/data/com.sec.android.app.sbrowser.beta/` |
| **UC Browser** | `com.UCMobile.intl` | `/data/data/com.UCMobile.intl/` | `/sdcard/Android/data/com.UCMobile.intl/` |
| **UC Browser Mini** | `com.ucmobile.browser` | `/data/data/com.ucmobile.browser/` | `/sdcard/Android/data/com.ucmobile.browser/` |
| **UC Browser Turbo** | `com.translate.uploader` | `/data/data/com.translate.uploader/` | `/sdcard/Android/data/com.translate.uploader/` |
| **QQ Browser** | `com.tencent.mtt` | `/data/data/com.tencent.mtt/` | `/sdcard/Android/data/com.tencent.mtt/` |
| **QQ Browser Lite** | `com.tencent.qbnet` | `/data/data/com.tencent.qbnet/` | `/sdcard/Android/data/com.tencent.qbnet/` |
| **Sogou Browser** | `com.sogou.msa.mrsd` | `/data/data/com.sogou.msa.mrsd/` | `/sdcard/Android/data/com.sogou.msa.mrsd/` |
| **Baidu Browser** | `com.baidu.browser.core` | `/data/data/com.baidu.browser.core/` | `/sdcard/Android/data/com.baidu.browser.core/` |
| **Baidu Browser Lite** | `com.hicloud.android.swift` | `/data/data/com.hicloud.android.swift/` | `/sdcard/Android/data/com.hicloud.android.swift/` |
| **猎豹浏览器** | `com.ijinshan.browser` | `/data/data/com.ijinshan.browser/` | `/sdcard/Android/data/com.ijinshan.browser/` |
| **360 Browser** | `com.qihoo.browser` | `/data/data/com.qihoo.browser/` | `/sdcard/Android/data/com.qihoo.browser/` |
| **360 Security Browser** | `com.huawei.browser` | `/data/data/com.huawei.browser/` | `/sdcard/Android/data/com.huawei.browser/` |
| **Maxthon** | `com.maxthon.browser` | `/data/data/com.maxthon.browser/` | `/sdcard/Android/data/com.maxthon.browser/` |
| **AVG Browser** | `com.antivirus` | `/data/data/com.antivirus/` | `/sdcard/Android/data/com.antivirus/` |
| **Avast Browser** | `com.avast.android.browser` | `/data/data/com.avast.android.browser/` | `/sdcard/Android/data/com.avast.android.browser/` |
| **CM Browser** | `com.ksmobile.cleandroid` | `/data/data/com.ksmobile.cleandroid/` | `/sdcard/Android/data/com.ksmobile.cleandroid/` |
| **Nox Browser** | `com.bignox.browser` | `/data/data/com.bignox.browser/` | `/sdcard/Android/data/com.bignox.browser/` |
| **MEmu Browser** | `com.microvirt.browser` | `/data/data/com.microvirt.browser/` | `/sdcard/Android/data/com.microvirt.browser/` |
| **BlueStacks Browser** | `com.bluestacks.browser` | `/data/data/com.bluestacks.browser/` | `/sdcard/Android/data/com.bluestacks.browser/` |
| **ALOE Browser** | `com.alohamobile.allinone` | `/data/data/com.alohamobile.allinone/` | `/sdcard/Android/data/com.alohamobile.allinone/` |
| **Beaker Browser** | `com.beaker.browser` | `/data/data/com.beaker.browser/` | `/sdcard/Android/data/com.beaker.browser/` |
| **FOSS Browser** | `me.d潇.com` | `/data/data/me.d潇.com/` | `/sdcard/Android/data/me.d潇.com/` |
| **IceCatMobile** | `org.gnu.icecat` | `/data/data/org.gnu.icecat/` | `/sdcard/Android/data/org.gnu.icecat/` |
| **Ghostery Browser** | `com.ghostery.android.dolphin` | `/data/data/com.ghostery.android.dolphin/` | `/sdcard/Android/data/com.ghostery.android.dolphin/` |
| **Orbot (Tor)** | `org.torproject.android` | `/data/data/org.torproject.android/` | `/sdcard/Android/data/org.torproject.android/` |
| **Onion Browser** | `com.guskastl.g浏览器` | `/data/data/com.guskastl.g浏览器/` | `/sdcard/Android/data/com.guskastl.g浏览器/` |
| **Browsec VPN** | `com.browsec.vpn` | `/data/data/com.browsec.vpn/` | `/sdcard/Android/data/com.browsec.vpn/` |

---

## Android - Session Files Inside APK Data

### Chrome/Chromium Data Structure
```
/data/data/com.android.chrome/
├── app_chrome/
│   ├── Default/
│   │   ├── History
│   │   ├── History-journal
│   │   ├── Cookies
│   │   ├── Cookies-journal
│   │   ├── Bookmarks
│   │   ├── Login Data
│   │   ├── Login Data-journal
│   │   ├── Web Data
│   │   ├── Web Data-journal
│   │   ├── Sessions/
│   │   │   ├── <session_files>
│   │   │   ├── _CACHE_001_/
│   │   │   └── _CACHE_002_/
│   │   └── Local Storage/
│   ├── Local State
│   └── preferences
├── app_webview/
│   └── WebData/
└── shared_prefs/
    └── chrome_default_client_preferences.xml
```

### Firefox Data Structure
```
/data/data/org.mozilla.firefox/
├── files/
│   └── profiles/
│       └── <profile_id>/
│           ├── places.sqlite (history)
│           ├── cookies.sqlite
│           ├── webappsstore.sqlite
│           ├── sessionstore.jsonlz4
│           ├── sessionstore-backups/
│           │   ├── recovery.jsonlz4
│           │   └── previous.jsonlz4
│           └── cache2/
│               └── entries/
├── cache/
├── shared_prefs/
└── lib/
```

### Samsung Internet Data Structure
```
/data/data/com.sec.android.app.sbrowser/
├── files/
│   └── pml/
│       └── Sessions/
├── databases/
│   ├── sbrowser.db
│   └── sbrowser_cache.db
├── shared_prefs/
│   └── com.sec.android.app.sbrowser_preferences.xml
└── lib/
```

---

## Android - Extracting Browser Data (ADB Commands)

### Prerequisites
```bash
# Enable USB debugging on Android device
# Connect device via USB
adb devices
```

### Pull Browser Data (Root Required)
```bash
# Chrome
adb pull /data/data/com.android.chrome/ ./chrome_data/

# Firefox
adb pull /data/data/org.mozilla.firefox/ ./firefox_data/

# Samsung Internet
adb pull /data/data/com.sec.android.app.sbrowser/ ./samsung_data/
```

### Pull External Data (No Root Required)
```bash
# Chrome external data
adb pull /sdcard/Android/data/com.android.chrome/ ./chrome_external/

# Firefox external data
adb pull /sdcard/Android/data/org.mozilla.firefox/ ./firefox_external/
```

### Use App Data Extractor (No Root)
```bash
# Install APK via ADB
adb install app_data_extractor.apk

# Or use Titanium Backup (requires root)
# Or use Swift Backup (no root, requires Shizuku)
```

### ADB Backup Method (No Root)
```bash
# Backup Chrome (encrypted)
adb backup com.android.chrome -f chrome_backup.ab

# Extract backup
dd if=chrome_backup.ab bs=1 skip=24 | openssl zlib -d > chrome_backup.tar

# Or use Android Backup Extractor
java -jar abe.jar unpack chrome_backup.ab chrome_backup.tar
```

### Dump Browser Data with Shizuku
```bash
# Grant Shizuku permissions via ADB
adb shell sh /storage/emulated/0/Android/data/.../shizuku.sh

# Then use apps with Shizuku support
```

---

## Android - SQLite Database Queries

### Chrome/Chromium History
```sql
-- Get browsing history
SELECT url, title, visit_count, last_visit_time 
FROM urls 
ORDER BY last_visit_time DESC;

-- Get downloads
SELECT tab_url, target_path, start_time, end_time 
FROM downloads;

-- Get cookies
SELECT host, name, value, path, expires_utc 
FROM cookies;

-- Get bookmarks
SELECT url, title FROM bookmarks;

-- Get saved passwords
SELECT origin_url, username_value, password_value 
FROM logins;
```

### Firefox Places (History)
```sql
-- Get history
SELECT url, title, visit_count, last_visit_date 
FROM moz_places 
ORDER BY last_visit_date DESC;

-- Get cookies
SELECT host, name, value, path, expiry 
FROM moz_cookies;

-- Get bookmarks
SELECT url, title, parent FROM moz_bookmarks;
```

### Samsung Internet
```sql
-- Get history
SELECT url, title, date FROM pml_history;

-- Get bookmarks
SELECT url, title FROM pml_bookmark;

-- Get saved passwords
SELECT url, username, password FROM pml_passwords;
```

---

## Android - Key Session Files by Browser

| Browser | Session File | Format |
|---------|-------------|--------|
| **Chrome** | `Sessions/_CACHE_001_/`, `Sessions/_CACHE_002_/` | Binary (Custom) |
| **Firefox** | `sessionstore.jsonlz4` | LZ4 compressed JSON |
| **Firefox Focus** | `sessionstore.jsonlz4` | LZ4 compressed JSON |
| **Samsung Internet** | `files/pml/Sessions/` | JSON |
| **Brave** | `Sessions/` | Binary (Custom) |
| **Vivaldi** | `Sessions/` | Binary (Custom) |
| **Opera** | `sessions/` | Binary |
| **UC Browser** | `UCCache4/` | Binary |
| **Kiwi Browser** | `Sessions/` | Binary |
| **Yandex** | `Sessions/` | Binary |

---

## Android - Backup File Locations

| Browser | Backup Path |
|---------|------------|
| **Chrome Bookmarks** | `/data/data/com.android.chrome/app_chrome/Default/Bookmarks` |
| **Chrome Preferences** | `/data/data/com.android.chrome/shared_prefs/` |
| **Firefox Profiles** | `/data/data/org.mozilla.firefox/files/profiles/` |
| **Firefox Cache** | `/data/data/org.mozilla.firefox/cache/` |
| **Samsung Preferences** | `/data/data/com.sec.android.app.sbrowser/shared_prefs/` |
| **UC Browser Data** | `/sdcard/Android/data/com.UCMobile.intl/` |
| **QQ Browser Data** | `/sdcard/Android/data/com.tencent.mtt/` |

---

## Android - Additional Paths

### Android Data Partition (Root Required)
```
/data/data/                    # All app private data
/data/dalvik-cache/            # Optimized DEX files
/data/app/                     # Installed APKs
/data/system/users/0/         # User-specific data
/data/user/0/                 # User 0 app data
/data/user/10/                # User 10 app data (work profile)
```

### Android External Storage
```
/sdcard/Android/data/          # Apps external data
/sdcard/Android/obb/           # OBB expansion files
/sdcard/Download/              # Downloads
/sdcard/Documents/              # Documents
```

### Chrome Profile Paths
```
/data/data/com.android.chrome/app_chrome/Default/
/data/data/com.android.chrome/app_chrome/Profile 1/
/data/data/com.android.chrome/app_chrome/Profile 2/
/data/data/com.android.chrome/app_chrome/Profile 3/
/data/data/com.android.chrome/app_chrome/Profile 4/
```

---

## Android - Complete Package Name Reference

| Browser | Package Name |
|---------|-------------|
| Chrome | `com.android.chrome` |
| Chrome Beta | `com.chrome.beta` |
| Chrome Dev | `com.chrome.dev` |
| Chrome Canary | `com.chrome.canary` |
| Firefox | `org.mozilla.firefox` |
| Firefox Nightly/Fenix | `org.mozilla.fenix` |
| Firefox Beta | `org.mozilla.firefox_beta` |
| Firefox Focus | `org.mozilla.focus` |
| Firefox Klar | `org.mozilla.klar` |
| Firefox Lite | `org.mozilla.rocket` |
| Firefox TV | `org.mozilla.tv.firefox` |
| Brave | `com.brave.browser` |
| Vivaldi | `com.vivaldi.browser` |
| Edge | `com.microsoft.emmx` |
| Opera | `com.opera.browser` |
| Opera Mini | `com.opera.mini.android` |
| Opera GX | `com.opera.browser.gx` |
| Opera Touch | `com.opera.browser.touch` |
| Samsung Internet | `com.sec.android.app.sbrowser` |
| DuckDuckGo | `com.duckduckgo.mobile.android` |
| UC Browser | `com.UCMobile.intl` |
| UC Browser Mini | `com.ucmobile.browser` |
| Bromite | `org.bromite.cromite` |
| Kiwi Browser | `com.kiwibrowser.browser` |
| Yandex | `com.yandex.browser` |
| QQ Browser | `com.tencent.mtt` |
| Sogou Browser | `com.sogou.msa.mrsd` |
| Baidu Browser | `com.baidu.browser.core` |
| Maxthon | `com.maxthon.browser` |
| Avast | `com.avast.android.browser` |
| AVG | `com.antivirus` |
| CM Browser | `com.ksmobile.cleandroid` |
| Ghostery | `com.ghostery.android.dolphin` |
| Dolphin | `com.dolphin.browser` |
| Lightning | `acr.browser.lightning` |
| Mint Browser | `com.mint.browser` |
| Kiwi Lite | `com.kiwibrowser.lite` |
| 360 Browser | `com.qihoo.browser` |
|猎豹 Browser | `com.ijinshan.browser` |
| CM Security | `com.cleanmaster.security` |
| BitBrowser | `com.bitbrowser.android` |
| Aloha Browser | `com.alohamobile.browser` |
| ALook Browser | `com.alookbrowser.android` |
| Via Browser | `mark.via.gp` |
| XBrowser | `com.wKee.browsers` |
| FOSS Browser | `me.d潇.com` |
| IceCat | `org.gnu.icecat` |
| Orbot | `org.torproject.android` |

---

## Android Forensics Quick Reference

### List All Installed Browsers
```bash
adb shell pm list packages | grep -E "(chrome|firefox|brave|vivaldi|opera|edge|browser|web)"
```

### Get Package Info
```bash
adb shell dumpsys package com.android.chrome | grep -E "version|dataDir|codePath"
```

### Pull All Browser Data (Root)
```bash
for pkg in com.android.chrome org.mozilla.firefox com.brave.browser com.microsoft.emmx com.vivaldi.browser com.sec.android.app.sbrowser; do
    adb pull /data/data/$pkg/ ./$pkg/ 2>/dev/null
done
```

### List Browser Sessions (If Accessible)
```bash
adb shell find /data/data -name "session*" -o -name "Session*" 2>/dev/null | grep -v lib
```

### Extract Chrome Session (Root)
```bash
adb pull /data/data/com.android.chrome/app_chrome/Default/Sessions/ ./sessions/
```

### Decode Chrome Session Binary
```python
import struct
# Chrome session files are Protocol Buffer based
# Use chrome-session-parser tools or browser-history library
```

---

## Cross-Platform Summary

```
┌────────────────────────────────────────────────────────────────────────────────────┐
│                                    ANDROID                                          │
├────────────────────────────────────────────────────────────────────────────────────┤
│ Chrome           →  /data/data/com.android.chrome/                                  │
│ Firefox          →  /data/data/org.mozilla.firefox/                                │
│ Firefox Nightly  →  /data/data/org.mozilla.fenix/                                   │
│ Firefox Focus   →  /data/data/org.mozilla.focus/                                    │
│ Brave            →  /data/data/com.brave.browser/                                  │
│ Vivaldi          →  /data/data/com.vivaldi.browser/                                 │
│ Edge             →  /data/data/com.microsoft.emmx/                                 │
│ Opera            →  /data/data/com.opera.browser/                                   │
│ Opera Mini       →  /data/data/com.opera.mini.android/                             │
│ Samsung Internet →  /data/data/com.sec.android.app.sbrowser/                       │
│ DuckDuckGo       →  /data/data/com.duckduckgo.mobile.android/                      │
│ UC Browser       →  /data/data/com.UCMobile.intl/                                  │
│ QQ Browser       →  /data/data/com.tencent.mtt/                                    │
│ Bromite          →  /data/data/org.bromite.cromite/                                │
│ Kiwi Browser     →  /data/data/com.kiwibrowser.browser/                            │
│ Yandex           →  /data/data/com.yandex.browser/                                  │
│ Maxthon          →  /data/data/com.maxthon.browser/                                 │
│ Dolphin          →  /data/data/com.dolphin.browser/                                │
│ Lightning        →  /data/data/acr.browser.lightning/                               │
├────────────────────────────────────────────────────────────────────────────────────┤
│ EXTERNAL STORAGE (No Root)                                                         │
├────────────────────────────────────────────────────────────────────────────────────┤
│ Chrome           →  /sdcard/Android/data/com.android.chrome/                      │
│ Firefox          →  /sdcard/Android/data/org.mozilla.firefox/                      │
│ Brave            →  /sdcard/Android/data/com.brave.browser/                        │
│ Vivaldi          →  /sdcard/Android/data/com.vivaldi.browser/                      │
│ UC Browser       →  /sdcard/Android/data/com.UCMobile.intl/                        │
│ QQ Browser       →  /sdcard/Android/data/com.tencent.mtt/                          │
└────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Notes

- **Root Required**: Most `/data/data/` paths require root access
- **No Root Alternative**: Use ADB backup, Shizuku, or Swift Backup
- **Chrome Data**: Often encrypted on modern Android (Android 10+)
- **Firefox**: Session files may be in profile directory under `files/profiles/`
- **Samsung**: Stores session data in `files/pml/Sessions/`
- **Multi-User**: Android 4.4+ supports multiple users under `/data/user/`
- **Work Profile**: Browser data may be in `/data/user/10/` (work profile)

---

*Generated for Browser Forensics & Session Analysis - Linux, Windows & Android*
