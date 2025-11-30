# GoT RPG - RollTables Setup

## Native Foundry RollTables nutzen

Statt eines eigenen Systems nutzen wir die nativen Foundry RollTables. Diese sind besser integriert und einfacher zu verwenden.

## 🎲 RollTables erstellen

### Methode 1: Über die Konsole (Einfachste Methode)

1. **Foundry VTT öffnen** und als GM anmelden
2. **F12 drücken** für die Konsole
3. **Folgenden Code kopieren und ausführen:**

```javascript
// ========================================
// GoT RPG RollTables Creator
// ========================================

async function createGoTRollTables() {
  
  const tables = {
    "Intrigen-Ereignisse": [
      "Ein Spion wird in den eigenen Reihen entdeckt",
      "Ein Komplott gegen einen Verbündeten wird aufgedeckt",
      "Kompromittierende Briefe fallen in die falschen Hände",
      "Ein Gerücht über einen Skandal verbreitet sich am Hof",
      "Ein ehemaliger Feind macht ein überraschendes Friedensangebot",
      "Ein Attentatsversuch schlägt fehl, der Auftraggeber bleibt unbekannt",
      "Eine arrangierte Hochzeit wird in letzter Minute abgesagt",
      "Ein wichtiger Verbündeter wechselt unerwartet die Seiten",
      "Eine geheime Liaison wird öffentlich bekannt",
      "Ein alter Schwur wird eingefordert",
      "Erpressungsversuche durch einen unbekannten Absender",
      "Ein Bastard erhebt Anspruch auf ein Erbe",
      "Vergiftungsgerüchte bei einem Festmahl",
      "Ein Maester wird der Spionage bezichtigt",
      "Eine politische Gefangene entkommt",
      "Ein Konkurrent verbreitet falsche Gerüchte über deine Familie",
      "Ein junges Familienmitglied verliebt sich in den falschen",
      "Ein alter Feind fordert ein Duell",
      "Geheimdokumente werden gestohlen",
      "Ein vertrauenswürdiger Berater wird bestochen"
    ],
    
    "Reise-Komplikationen": [
      "Plötzlicher Wetterumschwung - Sturm oder Schneesturm",
      "Banditen lauern am Wegesrand",
      "Eine wichtige Brücke ist eingestürzt",
      "Wildtiere greifen das Lager an (Wölfe, Bären)",
      "Ein Pferd lahmt oder wird krank",
      "Vorräte sind verdorben oder wurden gestohlen",
      "Der Weg ist durch einen Erdrutsch blockiert",
      "Begegnung mit feindlichen Soldaten oder Deserteuren",
      "Ein Fluss ist über die Ufer getreten",
      "Die Gruppe verirrt sich im Nebel",
      "Ein Mitreisender wird schwer krank",
      "Räuber haben die nächste Herberge überfallen",
      "Ein wichtiger Gegenstand geht verloren",
      "Begegnung mit misstrauischen Dorfbewohnern",
      "Ein Rad am Wagen bricht",
      "Wildlinge werden gesichtet (im Norden)",
      "Eine verlassene Siedlung mit bösen Vorzeichen",
      "Streit innerhalb der Reisegruppe eskaliert",
      "Ein mysteriöser Fremder folgt der Gruppe",
      "Unpassierbares Gelände erzwingt einen Umweg"
    ],
    
    "Gerüchte": [
      "Der Winter naht, und er wird härter als je zuvor",
      "Wildlinge sammeln sich jenseits der Mauer in großer Zahl",
      "Ein Drache wurde im Osten gesichtet",
      "Die Eisenmänner planen neue Raubzüge",
      "Eine reiche Handelskarawane wird bald durch die Gegend kommen",
      "Ein Septon verkündet düstere Prophezeiungen",
      "In den Flusslanden wurden Leichen ohne Köpfe gefunden",
      "Ein Lord plant heimlich eine Rebellion",
      "Gold wurde in den nahen Bergen entdeckt",
      "Eine schöne Jungfer sucht einen Gemahl von Stand",
      "Piraten treiben ihr Unwesen an der Küste",
      "Eine geheimnisvolle Krankheit breitet sich aus",
      "Der König plant eine große Hochzeit oder ein Turnier",
      "Ein verschollener Erbe ist zurückgekehrt",
      "Wölfe haben das Vieh angegriffen - oder waren es Schattenwölfe?",
      "Ein alter Schatz soll in Ruinen verborgen sein",
      "Die Ernte war schlecht, Hunger droht im Winter",
      "Ein berühmter Ritter ist auf Wanderschaft",
      "Hexerei wird in einem nahen Dorf praktiziert",
      "Die Weißen Wanderer sind nur eine Legende - oder doch nicht?"
    ],
    
    "Haus-Ereignisse": [
      "Ein Vasall rebelliert und verweigert den Lehnseid",
      "Ein Bote bringt schlechte Nachrichten vom König",
      "Ein wichtiger Verbündeter stirbt unerwartet",
      "Die Ernte auf den Ländereien ist außergewöhnlich gut",
      "Ein Erbe wird geboren",
      "Ein Familienmitglied erkrankt schwer",
      "Ein Nachbarhaus fordert alte Schulden ein",
      "Räuber plündern Dörfer auf eurem Land",
      "Ein talentierter Handwerker bietet seine Dienste an",
      "Die Minen bringen weniger Ertrag als erwartet",
      "Ein Skandal erschüttert das Ansehen des Hauses",
      "Ein alter Anspruch auf Land wird erhoben",
      "Die Burgmauern zeigen Risse und müssen repariert werden",
      "Ein wichtiger Handelsvertrag wird angeboten",
      "Söldner bieten ihre Dienste an - zu hohem Preis",
      "Ein Vasall bittet um militärische Unterstützung",
      "Ein junges Familienmitglied verschwindet spurlos",
      "Eine reiche Witwe sucht Schutz bei eurem Haus",
      "Der Maester des Hauses wird abberufen",
      "Ein Turnier soll auf der Burg ausgerichtet werden"
    ]
  };
  
  let created = 0;
  
  for (const [name, entries] of Object.entries(tables)) {
    // Prüfe ob Tabelle bereits existiert
    const existing = game.tables.find(t => t.name === name);
    if (existing) {
      console.log(`Tabelle "${name}" existiert bereits`);
      continue;
    }
    
    // Erstelle RollTable
    const table = await RollTable.create({
      name: name,
      formula: `1d${entries.length}`,
      replacement: true,
      displayRoll: false
    });
    
    // Füge Einträge hinzu
    const results = entries.map((text, index) => ({
      type: CONST.TABLE_RESULT_TYPES.TEXT,
      text: text,
      weight: 1,
      range: [index + 1, index + 1]
    }));
    
    await table.createEmbeddedDocuments("TableResult", results);
    created++;
    console.log(`✓ Tabelle "${name}" erstellt`);
  }
  
  ui.notifications.info(`${created} GoT-Rolltabellen wurden erstellt!`);
  console.log(`Insgesamt ${created} Tabellen erstellt.`);
}

// Ausführen
createGoTRollTables();
```

4. **Warten** bis die Meldung erscheint
5. **Fertig!** Die Tabellen sind jetzt unter "Rollable Tables" verfügbar

### Methode 2: Manuell erstellen

1. **Rollable Tables** öffnen (Würfel-Icon in der Sidebar)
2. **"Create RollTable"** klicken
3. Namen eingeben (z.B. "Intrigen-Ereignisse")
4. **Formula:** `1d20` (für 20 Einträge)
5. **Results** hinzufügen (kopiere aus der Liste unten)

## 📖 Verwendung

### RollTable würfeln

1. **Rollable Tables** öffnen
2. **Tabelle auswählen** (z.B. "Gerüchte")
3. **Würfel-Icon** in der Tabelle klicken
4. Ergebnis wird im Chat gepostet

### Quick-Roll per Konsole

```javascript
// Tabelle finden und würfeln
game.tables.getName("Gerüchte").draw();
game.tables.getName("Haus-Ereignisse").draw();
game.tables.getName("Intrigen-Ereignisse").draw();
game.tables.getName("Reise-Komplikationen").draw();
```

### Makros erstellen

Erstelle ein Makro mit:

```javascript
// Makro: Zufälliges Gerücht
game.tables.getName("Gerüchte")?.draw();
```

Oder mit Dialog:

```javascript
// Makro: GoT Rolltabellen
new Dialog({
  title: "GoT Rolltabellen",
  content: `
    <form>
      <div class="form-group">
        <label>Wähle eine Tabelle:</label>
        <select name="table">
          <option value="Gerüchte">Gerüchte</option>
          <option value="Haus-Ereignisse">Haus-Ereignisse</option>
          <option value="Intrigen-Ereignisse">Intrigen-Ereignisse</option>
          <option value="Reise-Komplikationen">Reise-Komplikationen</option>
        </select>
      </div>
    </form>
  `,
  buttons: {
    roll: {
      label: "Würfeln",
      callback: (html) => {
        const name = html.find('[name="table"]').val();
        game.tables.getName(name)?.draw();
      }
    }
  },
  default: "roll"
}).render(true);
```

## 🎯 Vorteile der nativen Lösung

- ✅ Nutzt Foundrys eingebautes System
- ✅ Keine zusätzlichen Module nötig
- ✅ Volle Integration mit Chat, Makros, Compendien
- ✅ Kann exportiert/importiert werden
- ✅ Unterstützt Bilder, Formeln, komplexe Ergebnisse
- ✅ GM kann Tabellen leicht bearbeiten
- ✅ Spieler können (wenn berechtigt) selbst würfeln

## 🗑️ Alte Dateien entfernen (optional)

Falls du das alte Custom-System nicht mehr brauchst:

- `module/logic/rolltables.js`
- `module/logic/macros.js`
- `module/logic/rolltables-init.js`
- `module/init-rolltables.js`
- `styles/rolltables.css`
- `ROLLTABLES-README.md`
