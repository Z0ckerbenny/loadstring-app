# 🚀 Release-Anleitung – Installer + Auto-Update

So verteilst du deine App: **Setup.exe an Freunde geben** → Setup lädt immer die neueste Version von GitHub → installierte App sucht bei jedem Start automatisch nach Updates. Die App erscheint in **Windows-Einstellungen → Apps → Installierte Apps** (mit Deinstallation).

## Wie es funktioniert

```
GitHub-Repo "loadstring-app" (PUBLIC!)
├── version.json                    <- Version + Download-Links + Changelog
└── builds/
    ├── LoadstringCracker.exe       <- die aktuelle App (einfach ersetzen)
    └── Uninstall.exe               <- der aktuelle Uninstaller (einfach ersetzen)
```

- **Setup.exe** (`LoadstringCracker_Installer.pb` kompiliert): Options-Seite (Desktop-/Startmenü-Häkchen, Autostart) → lädt neueste Version nach `%LOCALAPPDATA%\LoadstringCracker\` → erstellt Verknüpfungen → trägt die App in „Installierte Apps“ ein → startet die App.
- **App** (`LoadstringCracker_Licensed.pb`): prüft bei jedem Start schon im Ladebildschirm `version.json`. Neue Version → wird sofort automatisch geladen + installiert (App + Uninstaller) → automatischer Neustart. Kein Klick nötig.
- **Uninstall.exe** (`LoadstringCracker_Uninstaller.pb` kompiliert): löscht App-Dateien, Key, Verknüpfungen und den Registry-Eintrag.

## Einmalig einrichten

1. **GitHub-Repo** `loadstring-app` → ⚠️ **Public!** (Private geht nicht, sonst 404 beim Download.)
2. **`version.json` hochladen** (Vorlage liegt bei, deine URLs sind schon eingetragen):
   - `version` muss zu `#APP_VERSION` in der `.pb`-Datei passen (Start: `2.0.0`)
3. **3× kompilieren** (alle `.pb` in `purebasic-projekte/`):
   - `LoadstringCracker_Licensed.pb` → als `LoadstringCracker.exe`
   - `LoadstringCracker_Installer.pb` → als `Setup.exe`
   - `LoadstringCracker_Uninstaller.pb` → als `Uninstall.exe`
4. **Beide Exes hochladen:** Im Repo Ordner `builds/` erstellen → `LoadstringCracker.exe` + `Uninstall.exe` per Web-Upload hochladen (exakte Namen, Groß-/Kleinschreibung beachten!).
5. **Test:** `Setup.exe` starten → Installation startet von selbst (Optionen vorher ändern oder abbrechen möglich) → App startet → Versionsnummer prüfen = passt ✅ → in Windows-Einstellungen → Apps → Installierte Apps nach „Loadstring Cracker“ schauen ✅
6. **`Setup.exe` an Freunde geben** – mehr brauchen sie nie. Das Setup lädt immer die neueste Version.

## Bei jedem Update (neue Version rausbringen)

1. In `LoadstringCracker_Licensed.pb` die Version erhöhen:
   ```purebasic
   #APP_VERSION = "2.0.1"   ; z. B. 2.0.0 -> 2.0.1
   ```
2. Neu kompilieren → `LoadstringCracker.exe` (Uninstaller nur neu kompilieren, wenn du ihn geändert hast)
3. Auf GitHub: alte Exe(s) in `builds/` **ersetzen** (Datei anklicken → „Replace“ / oder neu hochladen, gleicher Name!)
4. `version.json` anpassen: `version` + `changelog`-Text → committen
5. Fertig – alle installierten Apps updaten sich beim nächsten Start automatisch im Ladebildschirm. 🎉

> 💡 **Setup.exe muss fast nie neu kompiliert werden** – nur wenn sich die `#VERSION_URL` ändert. Es lädt ja immer die neueste App-Version.

## Versionsnummern

Format `Haupt.Neben.Patch`, z. B. `2.1.0`. Die App vergleicht jede Stelle einzeln:
- `2.0.1` > `2.0.0` ✅ Update
- `2.10.0` > `2.9.0` ✅ (kein dummer Textvergleich)
- Gleich oder kleiner → kein Update

## Sicherheit (eingebaut)

- Download wird geprüft: Mindestgröße + `MZ`-Header (echte Windows-Exe, keine Fehlerseite)
- Update ersetzt die Exe erst nach erfolgreichem Download (per Helper-Skript + Neustart)
- Lizenz-Key bleibt bei Updates erhalten (liegt separat als `license.key`)
- Deinstallation löscht alles: Dateien, Key, Verknüpfungen, Registry-Eintrag

## Fehlerbehebung

| Problem | Lösung |
|---|---|
| Setup: „Download failed (received 14 bytes)“ | **404!** Exe liegt nicht (richtig) auf GitHub – Pfad + Dateiname + Großschreibung prüfen (so wie dein `_`-Fall) |
| Setup: „Invalid version info“ | `version.json`-URL prüfen, JSON-Syntax prüfen (Kommas!) |
| „Download failed (0 bytes)“ | Kein Internet oder GitHub down |
| Update-Dialog kommt nicht | `#APP_VERSION` vs. `version` in JSON vergleichen; GitHub braucht ~1–5 Min (Cache) |
| „Could not write the app file“ | App läuft noch → schließen → Retry |
| App nicht in „Installierte Apps“ | `Uninstall.exe` fehlt in `builds/` oder `uninstallUrl` fehlt in `version.json` → Setup fragt dann nichts ein |
