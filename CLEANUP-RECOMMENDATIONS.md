# GoT RPG System - Cleanup Empfehlungen

## Übersicht

Nach dem umfassenden Debugging wurden folgende optionale Cleanup-Maßnahmen identifiziert. Das System ist **voll funktionsfähig** - diese Empfehlungen sind reine Optimierungen zur Reduzierung ungenutzter Dateien.

---

## Status-Update (2025-11-30)

Die Cleanup-Maßnahmen wurden umgesetzt:

- 6 ungenutzte Dateien archiviert (`archive/…`)
- 4 RollTable-Pack-Einträge aus `system.json` entfernt
- `styles/rolltables.css` aus `system.json` entfernt und Datei gelöscht

Die nachfolgenden Schritte sind als Referenz für neue Kopien oder zukünftige Wartung dokumentiert.

## 1. Orphaned Files (Deaktiviertes Custom Rolltable-System)

### Betroffene Dateien
Diese Dateien gehören zum **deaktivierten** benutzerdefinierten Rolltabellen-System und werden nicht mehr verwendet:

```
module/logic/rolltables.js          (4.8 KB) - Tabellendaten & Funktionen
module/logic/macros.js              (3.2 KB) - SL-Makro Funktionen
module/logic/rolltables-init.js     (2.1 KB) - Initialisierungscode
module/init-rolltables.js           (1.3 KB) - Hook-Registrierung
styles/rolltables.css               (1.6 KB) - Pergament-Styling
ROLLTABLES-README.md                (4.5 KB) - Alte Dokumentation
```

**Gesamt: ~17.5 KB**

### Empfohlene Aktion

Status: Bereits durchgeführt (Dateien liegen in `archive/`).

**Option 1: Löschen (Empfohlen)**
```powershell
# PowerShell-Befehle zum sicheren Löschen
Remove-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\module\logic\rolltables.js"
Remove-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\module\logic\macros.js"
Remove-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\module\logic\rolltables-init.js"
Remove-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\module\init-rolltables.js"
Remove-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\styles\rolltables.css"
Remove-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\ROLLTABLES-README.md"
```

**Option 2: Archivieren**
```powershell
# Verschieben in Archive-Ordner (falls du sie später wiederherstellen willst)
New-Item -ItemType Directory -Path "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\archive" -Force
Move-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\module\logic\rolltables.js" "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\archive\"
Move-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\module\logic\macros.js" "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\archive\"
Move-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\module\logic\rolltables-init.js" "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\archive\"
Move-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\module\init-rolltables.js" "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\archive\"
Move-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\styles\rolltables.css" "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\archive\"
Move-Item "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\ROLLTABLES-README.md" "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg\archive\"
```

---

## 2. System.json - Ungenutzte Einträge

### Problem 1: Nicht existierende Pack-Dateien

`system.json` enthält RollTable-Pack-Einträge (Zeilen 69-92), die auf **nicht vorhandene** Dateien verweisen:

```json
{
  "name": "got-table-intrigen",
  "path": "packs/table-intrigen.db",
  "type": "RollTable"
}
```

**Status:** Diese `.db`-Dateien existieren nicht. Die RollTables wurden stattdessen als **World-Dokumente** erstellt.

### Problem 2: Ungenutztes CSS

`system.json` lädt `styles/rolltables.css` (Zeile 24), das zum deaktivierten Custom-System gehört.

### Empfohlene Änderungen

Status: Bereits umgesetzt. Die folgenden Beispiele dienen als Referenz für frische Installationen.

**Schritt 1: Entferne RollTable Packs**

Öffne `system.json` und **entferne die Zeilen 69-92**:

```json
// VOR (LÖSCHEN):
{
  "name": "got-table-intrigen",
  "label": "GoT Intrigen-Ereignisse",
  "path": "packs/table-intrigen.db",
  "type": "RollTable",
  "system": "got-rpg"
},
{
  "name": "got-table-reise",
  "label": "GoT Reise-Komplikationen",
  "path": "packs/table-reise.db",
  "type": "RollTable",
  "system": "got-rpg"
},
{
  "name": "got-table-geruechte",
  "label": "GoT Gerüchte",
  "path": "packs/table-geruechte.db",
  "type": "RollTable",
  "system": "got-rpg"
},
{
  "name": "got-table-haus",
  "label": "GoT Haus-Ereignisse",
  "path": "packs/table-haus.db",
  "type": "RollTable",
  "system": "got-rpg"
}

// NACH (nur andere Packs bleiben):
// ... (Weapons, Armor, Gear, etc. bleiben erhalten)
```

**Schritt 2: Entferne rolltables.css Referenz**

Ändere Zeile 22-24 in `system.json`:

```json
// VOR:
"styles": [
  "styles/got-rpg.css",
  "styles/rolltables.css"
]

// NACH:
"styles": [
  "styles/got-rpg.css"
]
```

---

## 3. Vollständiges Cleanup-Skript

Für maximale Effizienz, führe alle Cleanups auf einmal durch:

### PowerShell-Skript (Kopiere und führe in PowerShell aus)

Hinweis: Bereits ausgeführt. Nur nötig bei frischen Kopien oder wenn die Alt-Dateien erneut auftauchen.

```powershell
# GoT RPG System - Vollständiges Cleanup

$basePath = "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg"

# 1. Archiv erstellen (optional - entferne diese Zeile zum direkten Löschen)
New-Item -ItemType Directory -Path "$basePath\archive" -Force

# 2. Orphaned Files verschieben (oder löschen mit Remove-Item)
$filesToArchive = @(
    "module\logic\rolltables.js",
    "module\logic\macros.js",
    "module\logic\rolltables-init.js",
    "module\init-rolltables.js",
    "styles\rolltables.css",
    "ROLLTABLES-README.md"
)

foreach ($file in $filesToArchive) {
    $sourcePath = Join-Path $basePath $file
    if (Test-Path $sourcePath) {
        # Option A: Archivieren
        Move-Item $sourcePath "$basePath\archive\" -Force
        # Option B: Löschen (ersetze obere Zeile mit dieser):
        # Remove-Item $sourcePath -Force
    }
}

Write-Host "✓ Orphaned Files bereinigt" -ForegroundColor Green

# 3. system.json bereits bereinigt (Referenz)
Write-Host "`nHinweis: system.json ist bereits bereinigt. Die folgenden Schritte sind Referenz." -ForegroundColor Yellow
Write-Host "  - RollTable pack entries entfernt" -ForegroundColor Cyan
Write-Host "  - 'styles/rolltables.css' aus styles array entfernt" -ForegroundColor Cyan
Write-Host "Pfad: $basePath\system.json" -ForegroundColor Cyan
```

### Manuelle system.json Bearbeitung (Referenz)

Für frische Kopien: öffne `system.json` und entferne die 4 RollTable-Packs und den `styles/rolltables.css` Eintrag.

---

## 4. Verifikation

Nach dem Cleanup, überprüfe dass alles funktioniert:

### Checkliste

- [x] FoundryVTT neu laden
- [x] Charakterbogen öffnet ohne Fehler
- [x] Würfeln mit Modifiern funktioniert
- [x] RollTables sind über Sidebar zugänglich
- [x] Keine Console-Errors beim Laden
- [x] Equipment-Effekte funktionieren
- [x] Punkteallokation funktioniert

### Fehlerprüfung

```javascript
// In Browser-Konsole (F12):
game.tables.contents.map(t => t.name)
// Sollte ausgeben: ["Intrigen-Ereignisse", "Reise-Komplikationen", "Gerüchte", "Haus-Ereignisse"]
```

---

## 5. Rückgängig machen (Rollback)

Falls nach dem Cleanup Probleme auftreten:

### Option 1: Aus Archiv wiederherstellen

```powershell
$basePath = "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg"
Move-Item "$basePath\archive\*" "$basePath\" -Force
```

### Option 2: Git zurücksetzen

```powershell
cd "c:\Users\User\AppData\Local\FoundryVTT2\Data\systems\got-rpg"
git checkout HEAD -- system.json
git checkout HEAD -- module/logic/rolltables.js
# ... etc.
```

---

## Zusammenfassung

### Was wurde bereinigt?
- 6 ungenutzte Dateien (~17.5 KB) → archiviert
- 4 fehlerhafte Pack-Einträge in system.json → entfernt
- 1 ungenutzter CSS-Import → entfernt

### Risiko
**Minimal** - Alle zu löschenden Dateien sind deaktiviert und werden vom System nicht geladen.

### Empfehlung
**Archivieren statt löschen** für die ersten 30 Tage, dann endgültig löschen wenn keine Probleme auftreten.

### Nächste Schritte
1. Optional: Nach 30 Tagen Archiv-Ordner löschen
2. Bei neuen Kopien: Schritte aus diesem Dokument anwenden
