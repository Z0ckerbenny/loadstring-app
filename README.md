# 🌍 Ordner 2: Öffentliches GitHub-Repo `loadstring-app`

**Inhalt dieses Ordners = der komplette Inhalt deines ÖFFENTLICHEN Repos.**
Hier liegen nur Versionsinfos + fertige Exes — keine Quellen, keine Keys, kein Bot-Token.

## Struktur (so muss es auf GitHub aussehen)

```
loadstring-app (PUBLIC!)
├── version.json            <- Loadstring Cracker: Version + Download-Links + Changelog
├── version-spoofer.json    <- Roblox Spoofer: Version + Download-Links + Changelog
├── README.md               <- diese Datei (optional auf GitHub)
└── builds/
    ├── installer-LoadstringCracker.exe  <- Installer Cracker  (Server: /download)
    ├── LoadstringCracker.exe        <- Haupt-App Cracker  (Auto-Update lädt diese)
    ├── uninstaller-LoadstringCracker.exe                <- Deinstaller Cracker
    ├── installer-RobloxSpoofer.exe      <- Installer Spoofer  (Server: /download-spoofer)
    ├── RobloxSpoofer.exe            <- Haupt-App Spoofer  (Auto-Update lädt diese)
    └── uninstaller-RobloxSpoofer.exe        <- Deinstaller Spoofer
```

Namensregel: **Jede App hat immer 3 Exes: `installer-<Appname>.exe`, `<Appname>.exe`, `uninstaller-<Appname>.exe`** — damit man sie nie verwechselt.

## Einmalig einrichten

1. GitHub-Repo **`loadstring-app`** erstellen → Sichtbarkeit: ⚠️ **Public!** (Private = 404 beim Download)
2. `version.json` + `version-spoofer.json` hochladen (liegen bei, URLs sind schon eingetragen).
3. Ordner `builds/` erstellen → die **6 kompilierten Exes** aus Ordner 3 hochladen (exakte Namen, siehe `builds/PUT_EXES_HERE.txt`).
4. Platzhalter-Datei `PUT_EXES_HERE.txt` auf GitHub wieder löschen.
5. Test: `installer-LoadstringCracker.exe` runterladen → installieren → App startet → Version stimmt ✅ → in Windows „Installierte Apps" nachsehen ✅

## Bei jedem Update (neue Version rausbringen)

1. Im lokalen BlackSystems-Hauptordner `python tools/release.py --app LoadstringCracker --bump patch --message "Änderung"` ausführen (für Spoofer `--app RobloxSpoofer`, für beide `--app all`). Umfang passend als Patch/Minor/Major wählen.
2. Alle drei Programme der betroffenen App neu kompilieren, mit den vorgeschriebenen EXE-Namen.
3. Die EXEs in `builds/` ersetzen.
4. **Danach** die bereits synchronisierte `version.json` bzw. `version-spoofer.json` veröffentlichen.
5. Den Update-Ablauf und die Icons unter Windows prüfen.

Installer laden weiterhin die neueste Haupt-App. Geänderte Installer-/Uninstaller-Binärdateien müssen ebenfalls veröffentlicht werden. Die aktuelle Version 2.1.27 / 1.1.0 ist schon eingetragen; zum Bauen dieser Korrektur nicht nochmals erhöhen.

## Versionsnummern

Format `Haupt.Neben.Patch` (z. B. `2.1.0`). Die Apps vergleichen jede Stelle einzeln: `2.10.0` > `2.9.0` ✅. Gleich oder kleiner → kein Update.

## Fehlerbehebung

| Problem | Lösung |
|---|---|
| Installer: „Download failed" / 0–14 Bytes | 404! Exe liegt nicht (richtig) auf GitHub — Dateiname + Großschreibung prüfen |
| „Invalid version info" | `version.json`-URL prüfen, JSON-Syntax prüfen (Kommas!) |
| Update kommt nicht | `#APP_VERSION` vs. JSON vergleichen; GitHub braucht ~1–5 Min (Cache) |
| App nicht in „Installierte Apps" | `uninstaller-LoadstringCracker.exe` fehlt in `builds/` oder `uninstallUrl` fehlt in JSON |


## Verbindliche Versionierung und Icon-Korrektur

Bei jeder Änderung die Version der betroffenen App passend als Patch/Minor/Major erhöhen, synchron in allen drei PB-Quellen und der Versions-JSON. Dafür `python tools/release.py --app all --bump patch --message "Änderung"` im BlackSystems-Hauptordner verwenden; `--check` prüft die Übereinstimmung. Die aktuelle Korrektur ist bereits als **2.1.27 / 1.1.0** eingetragen. Alle sechs EXEs neu kompilieren, EXEs zuerst hochladen, JSON zuletzt. Details und Icon-Anleitung: `purebasic-quellen/README.md`.
