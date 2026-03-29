# Feature Specification: Curated Knowledge Worker Marketplace

**Feature Branch**: `001-skill-marketplace-scaffold`
**Created**: 2026-03-27
**Status**: Draft
**Input**: Bash-basiertes System zur Buendelung, Klassifizierung und Qualitaetssicherung von Skills/Plugins fuer Knowledge-Worker-Aufgabenbereiche mit Hash-Tree-Tracking, Skill-Audits und Plugin-Lifecycle-Management.

## Clarifications

### Session 2026-03-27

- Q: Verhaeltnis Modul vs Bundle vs Marketplace? → A: Modul = Marketplace-Verzeichnis (Organisationseinheit mit eigenem marketplace.json), Bundle = Profil-Aktivierung (kann Skills quer ueber mehrere Module referenzieren).
- Q: Wo werden Hash-Trees gespeichert? → A: Co-located im Plugin-Verzeichnis: `plugins/<modul>/<plugin>/.hash-tree.json`, versioniert mit Git.
- Q: Wie werden Skills den Modulen zugeordnet? → A: Zentrale Zuordnungsdatei (`module-registry.json`) mit Skill-Quellen pro Modul, Skript liest diese.
- Q: Was passiert bei QA-Fehler eines Skills? → A: Warn & Import mit Flag — Skill wird importiert aber als `status: "quarantine"` im Katalog markiert, Import läuft weiter.
- Q: Verhältnis Plugin-Lifecycle-Skill zu Plugin Forge? → A: Eigenständiger projektlokaler Skill; Plugin Forge (Quelle: Anthropic `claude-plugins-official`, lokal unter `.claude/plugin-forge/`) dient als Ausgangslage. SKILL.md oder README enthält Attribution mit Herkunfts-URL.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Marketplace-Modul aus Skript erzeugen (Priority: P1)

Ein Kurator fuehrt ein Bash-Skript aus und gibt einen Modulnamen (z.B. `ai-engineer`) an. Das Skript erstellt die vollstaendige Plugin-Verzeichnisstruktur fuer dieses Modul im Marketplace: Katalog-Eintraege, Verzeichnisse, marketplace.json-Fragment und Platzhalter fuer Skills. Es werden dabei die bestehenden Skills aus den konfigurierten Quellen (lokale Verzeichnisse, externe Git-URLs, bestehende Plugins) herangezogen und in das Modul uebernommen.

**Why this priority**: Ohne das Grundgeruest-Skript kann kein Modul erstellt werden. Es ist die Basis fuer alles Weitere.

**Independent Test**: Kann vollstaendig getestet werden, indem das Skript mit `ai-engineer` aufgerufen wird und die erzeugte Verzeichnisstruktur gegen eine definierte Soll-Struktur validiert wird.

**Acceptance Scenarios**:

1. **Given** ein leerer Marketplace-Ordner, **When** das Skript mit Modulname `ai-engineer` aufgerufen wird, **Then** entsteht die korrekte Verzeichnisstruktur unter `plugins/ai-engineer/` mit marketplace.json-Fragment, README und Skills-Verzeichnis.
2. **Given** ein Marketplace mit bestehendem Modul `ui-ux`, **When** das Skript mit `ai-engineer` aufgerufen wird, **Then** wird nur das neue Modul angelegt, bestehende Module bleiben unberuehrt.
3. **Given** ein Modulname mit zugeordneten Skill-Quellen (URLs, lokale Pfade), **When** das Skript laeuft, **Then** werden die Skills in den Katalog aufgenommen mit Herkunfts-URL, Autor und Version.

---

### User Story 2 - Hash-Tree fuer Skill-Integritaet erzeugen (Priority: P2)

Nach dem Hinzufuegen oder Aktualisieren eines Skills im Katalog wird automatisch ein Hash-Tree erstellt. Dieser erfasst die SHA-256-Hashes aller zugehoerigen Dateien (SKILL.md, Referenzdateien, plugin.json) und speichert sie als Baum-Struktur. Damit koennen Aenderungen zwischen Versionen sofort erkannt und Aehnlichkeiten zwischen Skills identifiziert werden.

**Why this priority**: Hash-Trees sind die Grundlage fuer Qualitaetssicherung, Duplikaterkennung und Versionskontrolle — zentrale Anforderungen des Marketplace.

**Independent Test**: Kann getestet werden, indem ein Skill hinzugefuegt, der Hash-Tree generiert, eine Datei geaendert und der neue Hash-Tree mit dem alten verglichen wird.

**Acceptance Scenarios**:

1. **Given** ein neu hinzugefuegter Skill mit 3 Dateien, **When** der Hash-Tree-Generator laeuft, **Then** entsteht eine JSON-Datei mit SHA-256 pro Datei und einem Root-Hash ueber alle Dateien.
2. **Given** ein bestehender Skill mit Hash-Tree, **When** eine Datei geaendert wird und der Generator erneut laeuft, **Then** unterscheidet sich der Root-Hash, und die geaenderte Datei ist im Diff identifizierbar.
3. **Given** zwei Skills mit identischem SKILL.md-Inhalt, **When** der Hash-Vergleich durchgefuehrt wird, **Then** werden die Skills als potenzielle Duplikate gekennzeichnet.

---

### User Story 3 - Skill-Audit-Report generieren (Priority: P3)

Fuer jeden Skill im Marketplace wird ein strukturierter Audit-Report unter `skill-audits/<author>/<skill>/` erstellt. Der Report dokumentiert Herkunft, Qualitaetsbewertung (Vollstaendigkeit der SKILL.md, vorhandene Referenzdateien, Sicherheitsbefunde) und Klassifizierung nach Aufgabenbereich.

**Why this priority**: Reports sind die Grundlage fuer Kuration und Qualitaetsentscheidungen, setzen aber voraus, dass Modules und Hash-Trees bereits existieren.

**Independent Test**: Kann getestet werden, indem ein einzelner Skill auditiert und der Report auf Vollstaendigkeit und korrektes Format geprueft wird.

**Acceptance Scenarios**:

1. **Given** ein Skill `bencium-innovative-ux-designer` im Marketplace, **When** das Audit-Skript auf diesen Skill laeuft, **Then** entsteht ein Report unter `skill-audits/bencium/bencium-innovative-ux-designer/` mit Herkunft, Version, Hash-Referenz und Qualitaetsbewertung.
2. **Given** ein Skill ohne SKILL.md-Frontmatter-Felder `name` oder `description`, **When** das Audit laeuft, **Then** wird ein Befund "INCOMPLETE: missing required frontmatter" im Report dokumentiert.

---

### User Story 4 - Bundle/Profil fuer Aufgabenbereich aktivieren (Priority: P4)

Ein Knowledge Worker aktiviert ueber ein einfaches Kommando (z.B. `/plugin-profile ai-engineer`) ein vordefiniertes Bundle, das die relevanten Skills und Plugins fuer seinen Aufgabenbereich laedt. Die Bundles werden zentral im Marketplace definiert und enthalten sowohl eigene Marketplace-Plugins als auch Referenzen auf externe Plugins.

**Why this priority**: Dies ist die Endanwender-Funktion, die auf allen vorherigen Stories aufbaut.

**Independent Test**: Kann getestet werden, indem ein neues Bundle `ai-engineer` definiert und ueber den Plugin-Profile-Skill aktiviert wird — resultierende Plugin-Liste wird gegen Soll-Werte verglichen.

**Acceptance Scenarios**:

1. **Given** ein Bundle `ai-engineer` in bundles.json mit 5 Marketplace-Plugins und 3 externen Plugins, **When** der Knowledge Worker `/plugin-profile ai-engineer` ausfuehrt, **Then** werden die externen Plugins in settings.json getoggelt und die Marketplace-Plugins als verfuegbar angezeigt.
2. **Given** der bestehende plugin-profile Skill, **When** das neue Bundle-System integriert wird, **Then** bleiben bestehende Bundles (saas, uiux, landingpage, founder) unveraendert funktionsfaehig.

---

### User Story 5 - Plugin-Lifecycle-Skill fuer projektlokale Verwaltung (Priority: P5)

Ein projektlokaler Skill steuert den gesamten Lifecycle eines Plugins innerhalb des Marketplace: Erstellen, Validieren, Auditieren, Versionieren und Veroeffentlichen. Dieser Skill basiert konzeptionell auf dem bestehenden Plugin-Profile-Skill und erweitert ihn um Lifecycle-Operationen.

**Why this priority**: Lifecycle-Management ist ein fortgeschrittenes Feature, das erst relevant wird, wenn das Grundgeruest steht.

**Independent Test**: Kann getestet werden, indem ein neuer Skill ueber den Lifecycle-Skill erstellt, validiert und als Plugin in den lokalen Marketplace aufgenommen wird.

**Acceptance Scenarios**:

1. **Given** ein lokaler Marketplace mit gueltigem marketplace.json, **When** der Lifecycle-Skill mit `create <skill-name>` aufgerufen wird, **Then** wird die vollstaendige Plugin-Verzeichnisstruktur inkl. SKILL.md-Vorlage, plugin.json und README erzeugt.
2. **Given** ein fertiges Plugin im lokalen Marketplace, **When** der Lifecycle-Skill mit `validate <plugin-name>` aufgerufen wird, **Then** wird die Struktur gegen das erwartete Schema geprueft und ein Validierungsbericht ausgegeben.

---

### Edge Cases

- Was passiert, wenn ein Skill-Quell-URL nicht erreichbar ist? Das Skript muss den Skill uebergehen und eine Warnung ausgeben, nicht abbrechen.
- Was passiert, wenn zwei Skills aus verschiedenen Quellen denselben Namen haben? Der Katalog muss Namenskonflikte erkennen und mit `<author>/<skill-name>` disambiguieren.
- Was passiert, wenn ein Hash-Tree fuer einen Skill generiert wird, der Symlinks enthaelt? Symlinks muessen aufgeloest werden, bevor der Hash berechnet wird.
- Was passiert, wenn ein Bundle auf ein nicht existierendes Plugin verweist? Das Aktivierungsskript muss eine Warnung ausgeben und die restlichen Plugins trotzdem laden.
- Was passiert, wenn die bestehende marketplace.json ein unguentiges Format hat? Das Skript validiert die Struktur vor Aenderungen und bricht bei ungueltigem Schema mit Fehlermeldung ab.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST ein Bash-Skript bereitstellen, das fuer einen gegebenen Modulnamen die vollstaendige Plugin-Verzeichnisstruktur erzeugt (Verzeichnisse, marketplace.json-Fragment, Platzhalter-Dateien).
- **FR-002**: System MUST die fuenf initialen Module unterstuetzen: `ai-engineer`, `ui-ux-design`, `landingpage`, `founder-os`, `best-of-breed`.
- **FR-003**: System MUST Skills aus mehreren Quellen importieren koennen: lokale Verzeichnisse, Git-Repository-URLs, bestehende Plugin-Verzeichnisse. Die Zuordnung von Skills zu Modulen wird ueber eine zentrale `module-registry.json` definiert, die pro Modul die Skill-Quellen (Pfade, URLs, Plugin-Referenzen) konfiguriert.
- **FR-004**: System MUST fuer jeden importierten Skill Metadaten erfassen: Herkunfts-URL, Autor, Version, Lizenz.
- **FR-005**: System MUST fuer jeden Skill einen Hash-Tree erzeugen, der SHA-256-Hashes aller zugehoerigen Dateien und einen Root-Hash enthaelt.
- **FR-006**: System MUST Hash-Trees als JSON-Datei pro Skill persistieren, um Aenderungen und Aehnlichkeiten ueber Vergleiche erkennen zu koennen.
- **FR-007**: System MUST fuer jeden Skill einen Audit-Report unter `skill-audits/<author>/<skill>/` erzeugen, der Herkunft, Qualitaetsbewertung und Klassifizierung dokumentiert.
- **FR-008**: System MUST eine QA-Pipeline bereitstellen, die pruefen kann: SKILL.md-Vollstaendigkeit, Pflichtfelder im Frontmatter, Verzeichnisstruktur-Konformitaet. Skills, die Pruefungen nicht bestehen, werden mit `status: "quarantine"` importiert und im Audit-Report als fehlerhaft markiert — der Import wird nicht blockiert.
- **FR-009**: System MUST den bestehenden `bundles.json`-Mechanismus nutzen und um die neuen Module erweitern, so dass `marketplace_plugins` und `external_plugins` korrekt aufgeloest werden.
- **FR-010**: System MUST einen projektlokalen Plugin-Lifecycle-Skill bereitstellen, der Plugins erstellen, validieren, auditieren und zum Marketplace hinzufuegen kann. Der Skill ist eigenstaendig (kein Wrapper), nutzt Plugin Forge (Anthropic `claude-plugins-official`) als inhaltliche Ausgangslage und enthaelt in SKILL.md oder README eine Attribution mit Herkunfts-URL (`https://github.com/anthropics/claude-plugins-official`).
- **FR-011**: System MUST Namenskonflikte bei Skills erkennen und durch `<author>/<skill-name>`-Namensraum aufloesen.
- **FR-012**: System MUST die bestehende Plugin-Struktur (marketplace.json, plugin.json, skills/<name>/SKILL.md) des Projekts respektieren und erweitern, nicht ersetzen.

### Key Entities

- **Modul**: Ein thematischer Aufgabenbereich (z.B. `ai-engineer`, `ui-ux-design`) als eigenstaendiges Marketplace-Verzeichnis unter `plugins/` mit eigenem `marketplace.json`. Organisationseinheit, die mehrere Plugins/Skills thematisch buendelt. Entspricht einem Claude Code Marketplace.
- **Skill**: Eine einzelne Faehigkeit mit SKILL.md, optionalen Referenzdateien und Metadaten. Kleinste kuratierbare Einheit.
- **Plugin**: Container fuer einen oder mehrere Skills im Claude-Code-Plugin-Format (plugin.json + skills/). Installierbare Einheit innerhalb eines Moduls.
- **Katalog-Eintrag**: Metadaten-Record pro Skill: Name, Beschreibung, Autor, Version, Herkunfts-URL, Klassifizierung, Hash-Referenz, Status (`active` | `quarantine`).
- **Hash-Tree**: JSON-Struktur mit SHA-256-Hashes aller Dateien eines Skills plus Root-Hash. Gespeichert co-located als `.hash-tree.json` im jeweiligen Plugin-Verzeichnis (`plugins/<modul>/<plugin>/.hash-tree.json`). Dient zur Aenderungserkennung und Duplikat-Identifikation.
- **Skill-Audit-Report**: Strukturierter Bericht pro Skill mit Herkunft, Qualitaetsbewertung, Sicherheitsbefunden und Klassifizierung.
- **Bundle**: Profil-Aktivierung, die Plugins quer ueber mehrere Module und externe Plugin-Quellen referenzieren kann. Definiert in bundles.json mit Vererbungshierarchie. Ein Bundle ist NICHT an ein einzelnes Modul gebunden.
- **Module-Registry**: Zentrale Zuordnungsdatei (`module-registry.json`) im Projekt-Root, die pro Modul definiert, welche Skill-Quellen (lokale Pfade, Git-URLs, Plugin-Referenzen) ihm zugeordnet sind. Das Skript liest diese Datei, um Skills in die richtige Modul-Struktur zu importieren.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Kurator kann ein neues Modul mit allen zugehoerigen Plugins in unter 5 Minuten ueber ein einziges Skript erstellen.
- **SC-002**: Fuer 100% der Skills im Marketplace existiert ein gueltiger Hash-Tree mit Root-Hash.
- **SC-003**: Fuer 100% der Skills existiert ein Audit-Report unter dem korrekten Pfad `skill-audits/<author>/<skill>/`.
- **SC-004**: Die fuenf initialen Module (ai-engineer, ui-ux-design, landingpage, founder-os, best-of-breed) sind vollstaendig im Marketplace mit mindestens je 3 Skills verfuegbar.
- **SC-005**: Ein Knowledge Worker kann ein Profil ueber `/plugin-profile <modulname>` aktivieren und hat danach alle relevanten Skills fuer seinen Aufgabenbereich geladen.
- **SC-006**: Aenderungen an einem Skill werden durch Hash-Tree-Vergleich in unter 1 Sekunde erkannt.
- **SC-007**: Die QA-Pipeline erkennt 100% der fehlenden Pflichtfelder (name, description) in SKILL.md-Frontmatter.

## Assumptions

- Das bestehende Plugin-Profile-System (`plugin-profile/SKILL.md`, `profiles.json`) bleibt als Basis erhalten und wird erweitert, nicht ersetzt.
- Die bestehende Verzeichnisstruktur unter `plugins/ui-ux/` mit marketplace.json und plugin.json-Dateien dient als Referenz fuer das Plugin-Format.
- Bash-Skripte sind die primaere Automatisierungsschicht; Python oder andere Sprachen koennen fuer spezifische QA-Aufgaben ergaenzend eingesetzt werden.
- Skills werden primaer als lokale Dateien importiert; Git-Clone fuer externe Quellen ist ein optionaler Erweiterungsschritt.
- Der Marketplace bleibt ein lokales Git-Repository — Distribution ueber `claude plugin marketplace add` ist ein Post-MVP-Ziel.
- `bundles.json` im `docs/`-Ordner dient als bestehende Referenz fuer das Bundle-Format und wird ins Marketplace-Root migriert.
- SHA-256 wird als Hash-Algorithmus verwendet (Industriestandard, ausreichend fuer Integritaets- und Aehnlichkeitserkennung).
- Ein "Plugin Forge"-Skill existiert konzeptionell, aber noch nicht als Datei im Projekt — er wird als neuer projektlokaler Plugin-Lifecycle-Skill spezifiziert und gebaut.
