# Technische Modernisierung - GoT RPG System

## Übersicht der Änderungen (V13+ Kompatibilität)

### 1. Foundry V13+ Namespace Migration

#### Globale Klassen ersetzt:
- ❌ `ActorSheet` → ✅ `foundry.applications.sheets.ActorSheetV2` (mit Fallback)
- ❌ `Actors.registerSheet()` → ✅ `foundry.applications.sheets.register()` (mit Fallback)
- ❌ `new Roll()` → ✅ `foundry.dice.Roll` (mit Fallback)

#### Abwärtskompatibilität:
Alle Änderungen enthalten Fallbacks für V11-V12:
```javascript
const ActorSheetClass = foundry.applications?.sheets?.ActorSheetV2 || ActorSheet;
```

**Warum wichtig?**
Foundry V13 warnt bereits: "You are accessing the global 'ActorSheet' which is now namespaced..."
In V15/16 werden globale Klassen vollständig entfernt.

---

### 2. Modularisierung

#### Neue Dateistruktur:
```
module/
├── got-rpg.js (Hauptdatei, ~70% kleiner)
├── logic/
│   ├── helpers.js (DOM-Helpers, Handlebars-Helpers, FIELD_NAMES)
│   └── dice.js (Würfellogik, Kampfsystem)
└── sheets/ (bereit für zukünftige Aufteilung)
```

#### module/logic/helpers.js
**Zentrale Helper-Funktionen:**
- `DOMHelpers.getNumberField(html, name, defaultValue)` - Sicherer Zugriff auf Number-Inputs
- `DOMHelpers.getTextField(html, name, defaultValue)` - Sicherer Zugriff auf Text-Inputs
- `DOMHelpers.getCheckboxField(html, name, defaultValue)` - Sicherer Zugriff auf Checkboxen
- `DOMHelpers.getSelectField(html, name, defaultValue)` - Sicherer Zugriff auf Selects
- `DOMHelpers.setFieldValue(html, name, value)` - Setzen von Werten
- `DOMHelpers.fieldExists(html, name)` - Existenz-Prüfung

**Zentrale Feld-Namen (FIELD_NAMES):**
```javascript
import { FIELD_NAMES } from './logic/helpers.js';

// Statt:
html.find('input[name="system.waehrung.ks"]')

// Besser:
html.find(`input[name="${FIELD_NAMES.CURRENCY_KS}"]`)
```

**Vorteile:**
- Template-Änderungen brechen nicht mehr still
- Zentrale Fehlerbehandlung mit Fallbacks
- Bessere Wartbarkeit und Refactoring
- Klare Trennung von UI-Logik und Datenlogik

#### module/logic/dice.js
**Würfel- und Kampfsystem:**
- `DiceHelpers.rollAttack(actor, item)` - Angriffswurf mit automatischem Fernkampf-Bonus
- `DiceHelpers.rollDamage(actor, item)` - Schadenswurf
- `DiceHelpers.rollAttribute(actor, attributeName)` - Attributsprobe
- `DiceHelpers.rollSkill(actor, skillName)` - Fertigkeitsprobe
- `DiceHelpers.calculateDamageFormula(actor, item)` - Schadensberechnung für UI
- `DiceHelpers.isRangedWeapon(item)` - Fernkampf-Erkennung

**Vorteile:**
- Konsistente Würfellogik in gesamtem System
- Zentrale Stelle für Spielmechanik
- Einfacher zu testen und debuggen
- Settings-Integration (fertigkeitsBonusAktiv)

---

### 3. Handlebars Helpers Zentralisierung

**Alle Helpers in `helpers.js` registriert:**
```javascript
registerHandlebarsHelpers(); // Ein Aufruf statt 10+
```

**Neue Comparison Helpers:**
- `eq`, `gt`, `lt`, `gte`, `lte` - Vergleichsoperatoren für Templates

**Bestehende Helpers:**
- `multiply`, `divide`, `add` - Mathematik
- `carryCapacity` - GoT-spezifisch: Traglast
- `lpBarColor` - GoT-spezifisch: LP-Farbe basierend auf %

---

### 4. Best Practices

#### Fehlerrobuste DOM-Selektoren
```javascript
// ❌ Alt (bricht still bei Template-Änderung):
const value = Number(html.find('input[name="system.waehrung.ks"]').val());

// ✅ Neu (mit Fallback und Logging):
const value = DOMHelpers.getNumberField(html, FIELD_NAMES.CURRENCY_KS, 0);
```

#### Zentrale Konfiguration
```javascript
// Statt hardcoded strings überall:
"system.lebenspunkte.aktuell"

// Verwende FIELD_NAMES:
FIELD_NAMES.LP_CURRENT
```

#### Konsistente Würfellogik
```javascript
// ❌ Alt (duplizierte Logik):
const roll = await (new Roll(formula)).roll({ async: true });
roll.toMessage({ ... });

// ✅ Neu (zentrale Funktion):
await DiceHelpers.rollAttack(this.actor, item);
```

---

## Migration bestehender Code

### Beispiel: Währungskonvertierung

**Vorher:**
```javascript
html.find('.convert-currency').on('click', async ev => {
  const ks = Number(html.find('input[name="system.waehrung.ks"]').val());
  const sh = Number(html.find('input[name="system.waehrung.sh"]').val());
  // ...
});
```

**Nachher:**
```javascript
html.find('.convert-currency').on('click', async ev => {
  const ks = DOMHelpers.getNumberField(html, FIELD_NAMES.CURRENCY_KS, 0);
  const sh = DOMHelpers.getNumberField(html, FIELD_NAMES.CURRENCY_SH, 0);
  // ...
});
```

---

## Nächste Schritte (Optional)

### 1. Weitere Modularisierung
- `sheets/actor.js` - GotCharacterSheet-Klasse
- `sheets/items.js` - GotItemSheet-Klasse
- `logic/bonuses.js` - Item-Bonus-Berechnung
- `logic/migrations.js` - Datenmigration zwischen Versionen

### 2. ApplicationV2 Migration (Foundry V13+)
```javascript
class GotCharacterSheet extends foundry.applications.api.ApplicationV2 {
  // Neue V2 API mit besserer Performance
}
```

### 3. TypeScript Migration
- Type-Safety für bessere Fehlerprävention
- IntelliSense-Unterstützung
- Automatische Dokumentation

### 4. Automated Testing
- Unit-Tests für DiceHelpers
- Integration-Tests für Sheet-Funktionen
- Regression-Tests bei Updates

---

## Bekannte Limitierungen

1. **Legacy-Unterstützung**: System unterstützt weiterhin V11-V12, daher können nicht alle V13+ Features genutzt werden
2. **Partial Migration**: Nicht alle globalen Klassen ersetzt (z.B. ChatMessage, Items bleiben vorerst)
3. **Template-Abhängigkeit**: FIELD_NAMES muss bei Template-Änderungen aktualisiert werden

---

## Versionshistorie

### v0.2.0 (Modernisierung)
- ✅ V13+ Namespace-Migration mit Fallbacks
- ✅ Modularisierung in logic/helpers.js und logic/dice.js
- ✅ DOMHelpers für robuste Selektoren
- ✅ FIELD_NAMES für zentrale Konfiguration
- ✅ Handlebars Helpers zentralisiert

### v0.1.1 (Vorherige Version)
- Initiales System mit V11+ Kompatibilität
- Alle Features in einer Datei
- Globale Klassen verwendet

---

## Entwickler-Notizen

**Wann DOMHelpers verwenden?**
- Immer bei `html.find('input[name="..."]').val()`
- Bei allen Form-Interaktionen im activateListeners()

**Wann FIELD_NAMES verwenden?**
- Bei allen Feld-Zugriffen in Templates und JS
- Bei actor.update({ [FIELD_NAMES.LP_CURRENT]: value })

**Wann DiceHelpers verwenden?**
- Bei allen Würfelwürfen
- Bei Schadens-/Angriffsberechnungen
- Bei Proben-Systemen

**Performance-Impact:**
- DOMHelpers: Minimal (nur try/catch overhead)
- FIELD_NAMES: Keine (nur String-Konstanten)
- DiceHelpers: Minimal (zentralisiert bestehende Logik)
- Modularisierung: Positiv (bessere Code-Splitting möglich)

---

## Support & Beitragen

Bei Fragen oder Verbesserungsvorschlägen:
1. Prüfe SETTINGS.md für Spieleinstellungen
2. Prüfe diese Datei für technische Details
3. Konsultiere Foundry V13 API Docs für neue Features

**Wichtige Links:**
- [Foundry V13 API Changes](https://foundryvtt.com/api/v13/)
- [ApplicationV2 Guide](https://foundryvtt.com/article/v2-applications/)
- [Module Best Practices](https://foundryvtt.com/article/module-development/)
