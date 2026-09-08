# 🚀 Release-Anleitung – Installer + Auto-Update

So verteilst du deine App: **Setup.exe an Freunde geben** → Setup lädt immer die neueste Version von GitHub → installierte App sucht bei jedem Start automatisch nach Updates.

## Wie es funktioniert

```
GitHub-Repo "loadstring-app" (PUBLIC!)
├── version.json                    <- aktuelle Versionsnummer + Download-Link
└── builds/
    └── LoadstringCracker.exe       <- die aktuelle App (einfach ersetzen)
```

- **Setup.exe** (`LoadstringCracker_Installer.pb` kompiliert): liest `version.json`, lädt die Exe nach `%LOCALAPPDATA%\LoadstringCracker\`, erstellt Desktop- + Startmenü-Verknüpfungen, startet die App.
- **App** (`LoadstringCracker_Licensed.pb`): prüft bei jedem Start `version.json`. Neue Version → Dialog → 1 Klick → Download → automatischer Neustart in die neue Version.

## Einmalig einrichten (ca. 10 Min)

1. **Neues GitHub-Repo erstellen**, z. B. `loadstring-app` → ⚠️ **Public!** (Private geht nicht, sonst schlägt der Download fehl.)
2. **`version.json` hochladen** (Vorlage liegt bei: `updater/version.json`):
   - `DEINNAME` durch deinen GitHub-Namen ersetzen
   - `version` muss zu `#APP_VERSION` in der `.pb`-Datei passen (Start: `2.0.0`)
3. **In BEIDEN `.pb`-Dateien** die Konstante setzen (muss exakt gleich sein!):
   ```purebasic
   #VERSION_URL = "https://raw.githubusercontent.com/DEINNAME/loadstring-app/main/version.json"
   ```
   - `LoadstringCracker_Licensed.pb` → kompilieren als `LoadstringCracker.exe`
   - `LoadstringCracker_Installer.pb` → kompilieren als `Setup.exe`
4. **Exe hochladen:** Im Repo Ordner `builds/` erstellen → `LoadstringCracker.exe` per Web-Upload („Add file → Upload files“) hochladen.
5. **Test:** `Setup.exe` starten → installiert die App → App starten → kein Update-Dialog = alles passt. ✅
6. **`Setup.exe` an Freunde geben** – mehr brauchen sie nie. Das Setup lädt immer die neueste Version.

## Bei jedem Update (neue Version rausbringen)

1. In `LoadstringCracker_Licensed.pb` die Version erhöhen:
   ```purebasic
   #APP_VERSION = "2.0.1"   ; z. B. 2.0.0 -> 2.0.1
   ```
2. Neu kompilieren → `LoadstringCracker.exe`
3. Auf GitHub: alte `builds/LoadstringCracker.exe` **ersetzen** (Datei anklicken → „Replace“ / oder neu hochladen, gleicher Name!)
4. `version.json` anpassen: `version` + `changelog`-Text → committen
5. Fertig – alle installierten Apps bieten das Update beim nächsten Start automatisch an. 🎉

> 💡 **Setup.exe muss fast nie neu kompiliert werden** – nur wenn sich die `#VERSION_URL` ändert (z. B. neues Repo). Es lädt ja immer die neueste App-Version.

## Versionsnummern

Format `Haupt.Neben.Patch`, z. B. `2.1.0`. Die App vergleicht jede Stelle einzeln:
- `2.0.1` > `2.0.0` ✅ Update
- `2.10.0` > `2.9.0` ✅ (kein dummer Textvergleich)
- Gleich oder kleiner → kein Update

## Sicherheit (eingebaut)

- Download wird geprüft: Mindestgröße + `MZ`-Header (echte Windows-Exe, keine Fehlerseite)
- Update ersetzt die Exe erst nach erfolgreichem Download (per Helper-Skript + Neustart)
- Lizenz-Key bleibt erhalten (liegt separat in `%LOCALAPPDATA%\LoadstringCracker\license.key`)

## Fehlerbehebung

| Problem | Lösung |
|---|---|
| Setup meldet „Invalid version info“ | `version.json`-URL prüfen (`#VERSION_URL`), JSON-Syntax prüfen (Kommas!) |
| „Download failed“ | Exe-URL in `version.json` im Browser testen – muss den Download starten |
| Update-Dialog kommt nicht | `#APP_VERSION` in der Exe vs. `version` in JSON vergleichen; GitHub braucht ~1–5 Min (Cache) |
| „Could not write the app file“ | App läuft noch → schließen → Retry |
