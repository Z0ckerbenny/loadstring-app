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

**Loadstring Cracker:**
1. In `LoadstringCracker_Licensed.pb`: `#APP_VERSION` erhöhen (z. B. `2.1.25` → `2.1.26`)
2. Neu kompilieren → `LoadstringCracker.exe`
3. Auf GitHub in `builds/` die alte Exe **ersetzen** (gleicher Name!) — `uninstaller-LoadstringCracker.exe` nur ersetzen, wenn geändert
4. `version.json`: `version` + `changelog` anpassen → committen
5. Fertig — alle installierten Apps updaten sich beim nächsten Start automatisch 🎉

**Roblox Spoofer:** genauso, mit `#APP_VERSION` in `RobloxSpoofer.pb` → `RobloxSpoofer.exe` → `version-spoofer.json`.

> 💡 Die **Installer müssen fast nie neu kompiliert** werden — nur wenn sich die GitHub-URLs ändern. Sie laden immer die neueste Version.

## Versionsnummern

Format `Haupt.Neben.Patch` (z. B. `2.1.0`). Die Apps vergleichen jede Stelle einzeln: `2.10.0` > `2.9.0` ✅. Gleich oder kleiner → kein Update.

## Fehlerbehebung

| Problem | Lösung |
|---|---|
| Installer: „Download failed" / 0–14 Bytes | 404! Exe liegt nicht (richtig) auf GitHub — Dateiname + Großschreibung prüfen |
| „Invalid version info" | `version.json`-URL prüfen, JSON-Syntax prüfen (Kommas!) |
| Update kommt nicht | `#APP_VERSION` vs. JSON vergleichen; GitHub braucht ~1–5 Min (Cache) |
| App nicht in „Installierte Apps" | `uninstaller-LoadstringCracker.exe` fehlt in `builds/` oder `uninstallUrl` fehlt in JSON |
