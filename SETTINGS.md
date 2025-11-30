# Game of Thrones RPG - System Settings

## Verfügbare Einstellungen

Das GoT-RPG System bietet verschiedene optionale Einstellungen, die im Foundry-Menü unter **Game Settings → Configure Settings → System Settings** angepasst werden können.

### 📊 Charaktererstellung

#### Start-Punkte
- **Standard**: 40
- **Beschreibung**: Anzahl der Charakterpunkte, die ein neuer Charakter zum Start erhält
- **Verwendung**: Wird beim Erstellen eines neuen Charakters automatisch in `system.punkte.verfuegbar` eingetragen

#### Standard-Lebenspunkte
- **Standard**: 100
- **Beschreibung**: Standard-Lebenspunkte für neue Charaktere
- **Verwendung**: Wird als `system.lebenspunkte.maximum` und `system.lebenspunkte.aktuell` gesetzt

#### Maximaler Attributwert
- **Standard**: 20
- **Bereich**: 10-30
- **Beschreibung**: Höchster Wert, den ein Attribut erreichen kann

#### Maximaler Fertigkeitswert
- **Standard**: 20
- **Bereich**: 10-30
- **Beschreibung**: Höchster Wert, den eine Fertigkeit erreichen kann

### ⚔️ Spielregeln

#### Fertigkeitsbonus-Regel aktivieren
- **Standard**: Aktiviert
- **Beschreibung**: Wenn aktiviert, erhalten ausgerüstete Fernkampfwaffen +1W6 Schaden, wenn **Schütze ≥5 UND Entfernungssinn ≥5**
- **Beispiel**: Ein Langbogen macht normalerweise 2W6 Schaden. Mit dieser Regel und den erforderlichen Fertigkeiten macht er 2W6+1W6 = 3W6 Schaden

#### Adelsstand-Charisma-Regel
- **Standard**: Aktiviert
- **Beschreibung**: Bestimmte Adelsstände setzen Charisma automatisch
- **Beispiele**:
  - Bürgerlicher, Leibeigener, Bastarde → Charisma 0
  - Verarmter Ritter → Charisma 1
  - Gesalbter Ritter, Wohlhabender Händler → Charisma 2
  - Hoher Offizier → Charisma 3
  - Lord kleines Territorium → Charisma 4

#### Traglast-Berechnung
- **Standard**: Aktiviert
- **Formel**: `((Stärke + Zähigkeit) / 2) × 20`
- **Beschreibung**: Berechnet automatisch die maximale Traglast des Charakters
- **Anzeige**: Im Inventar-Tab wird das aktuelle Gewicht vs. Traglast angezeigt

#### Rüstungsverteidigung-Berechnung
- **Standard**: Aktiviert
- **Formel**: `⌊Summe aller ausgerüsteten Rüstungs-Verteidigungswerte / 2⌋`
- **Beschreibung**: Berechnet die Gesamtverteidigung aus allen ausgerüsteten Rüstungsteilen
- **Beispiel**: Kettenhemd (Vert. 4) + Schild (Vert. 2) = 6/2 = 3 Rüstung

### 💰 Wirtschaft

#### Automatische Währungskonvertierung
- **Standard**: Aktiviert
- **Regeln**: 
  - 56 Kupferstücke (KS) = 1 Silberhirsch (SH)
  - 210 Silberhirsche (SH) = 1 Golddrache (GD)
  - 11.760 KS = 1 GD
- **Verhalten**: Bei Änderung der Währung wird automatisch umgerechnet

#### Automatischer Kaufpreis-Abzug
- **Standard**: Aktiviert
- **Beschreibung**: Beim Drag & Drop von Items aus einem Compendium wird der Preis automatisch vom Charaktergeld abgezogen
- **Verhalten**: 
  - Wenn nicht genug Geld vorhanden ist, erscheint eine Warnung
  - Bei Bestätigung wird das Item hinzugefügt und der Preis abgezogen

## 🏰 Neue Charakterbogen-Features

### Zustände / Statuseffekte
Im Allgemein-Tab gibt es jetzt eine Zustands-Sektion mit folgenden Statuseffekten:
- 🩹 Verwundet
- 💤 Erschöpft
- ☠️ Vergiftet
- 🍺 Betrunken
- 😵 Bewusstlos
- 🩸 Blutend
- 🦯 Verkrüppelt
- 🔥 Verbrannt
- ❄️ Erfroren
- 🍞 Verhungert
- 💧 Verdurstet
- 🤒 Krank
- 🤪 Wahnsinnig
- 🌑 Verflucht

Diese werden als Checkboxen angezeigt und können einfach aktiviert/deaktiviert werden.

### Haus-Tab
Ein neuer Tab für Haus-/Fraktionsinformationen:

#### Grunddaten
- Hausname
- Wappen (Bildpfad)
- Motto
- Sitz (Burg/Festung)
- Region
- Oberhaus / Lehnsherr
- Gründungsjahr

#### Hausstatus (0-10 Skala)
- **Einfluss**: Politische Macht und Verbindungen
- **Reichtum**: Finanzielle Ressourcen
- **Ansehen**: Reputation und Ehre

#### Ressourcen & Militär
- Ressourcen (Silberminen, Holzwälder, Fischereirechte, etc.)
- Militär (Anzahl Soldaten, Ritter, etc.)

#### Weitere Informationen
- Vasallen (Liste der Vasallenhäuser)
- Geschichte & Chronik (wichtige Ereignisse, berühmte Ahnen, Schlachten)

## 🔧 Technische Details

### Zugriff auf Settings im Code
```javascript
// Beispiel: Start-Punkte abrufen
const startPunkte = game.settings.get("got-rpg", "startPunkte");

// Beispiel: Prüfen ob Regel aktiv ist
const bonusAktiv = game.settings.get("got-rpg", "fertigkeitsBonusAktiv");
if (bonusAktiv && schuetze >= 5 && entfernungssinn >= 5) {
  // Bonus anwenden
}
```

### Template.json Erweiterungen
Die Character-Vorlage wurde erweitert um:

```json
"zustaende": {
  "verwundet": false,
  "erschoepft": false,
  // ... weitere Zustände
}
```

```json
"haus": {
  "name": "",
  "wappen": "",
  "motto": "",
  "sitz": "",
  "region": "",
  "oberhaus": "",
  "gruendung": "",
  "ressourcen": "",
  "militaer": "",
  "einfluss": 0,
  "reichtum": 0,
  "ansehen": 0,
  "geschichte": "",
  "vasallen": ""
}
```

## 📝 Hinweise für Spielleiter

### Empfohlene Einstellungen für verschiedene Kampagnentypen

#### Gritty Low-Fantasy
- Start-Punkte: 30
- Standard-LP: 80
- Fertigkeitsbonus: Deaktiviert
- Traglast-Regel: Aktiviert

#### Heroische Kampagne
- Start-Punkte: 50
- Standard-LP: 120
- Fertigkeitsbonus: Aktiviert
- Maximale Werte: 25

#### Haus-zentrierte Kampagne
Nutzen Sie den Haus-Tab ausgiebig für:
- Hausressourcen-Management
- Dynastien-Tracking
- Vasallen-Verwaltung
- Politische Intrigen

### Best Practices

1. **Settings vor Kampagnenstart festlegen**: Änderungen während der Kampagne können bestehende Charaktere beeinflussen
2. **Zustände konsistent nutzen**: Definieren Sie klare Regeln, was jeder Zustand mechanisch bedeutet
3. **Haus-Tab für Adelsfamilien**: Besonders wichtig für Charaktere mit hohem Adelsstand
4. **Kaufpreis-Abzug**: Bei Verwendung von Compendiums den Abzug im Auge behalten

## 🐛 Bekannte Einschränkungen

- Settings ändern nicht retroaktiv bestehende Charaktere (außer Regelberechungen)
- Haus-Daten sind aktuell rein informativ (keine automatischen Berechnungen)
- Zustände haben noch keine automatischen mechanischen Effekte (müssen vom SL gehandhabt werden)

## 🚀 Zukünftige Erweiterungen

Geplante Features:
- Automatische Effekte für Zustände (z.B. Verwundet → -2 auf alle Würfe)
- Haus-Ressourcen-System mit Würfen
- Dynastien-Tracker
- Vasallen-Management-Dialog
- Import/Export von Haus-Daten
