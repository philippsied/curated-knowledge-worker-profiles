# Plan: integrate_inbox.py – Inbox-zu-Plugin-Integration

## Context

Das Projekt hat eine `inbox/`-Ordnerstruktur mit 4 nicht-integrierten Design-Artefakten,
die noch nicht in `plugins/` registriert oder in `marketplace.json` eingetragen sind.
Ziel: Ein Python-Skript (`scripts/integrate_inbox.py`), das diese Artefakte analysiert,
Zuordnungen vorschlägt und die Integration vorbereitet – entweder automatisch oder als
manuelle Schritte (JSON-Plan-Export).

---

## Inbox – Aktueller Bestand

| Item | Typ | Skills | Notiz |
|------|-----|--------|-------|
| `inbox/design/` | Vollständiges Plugin (hat plugin.json) | 7 | Anthropic Design v1.2.0, inkl. .mcp.json |
| `inbox/claudekit/` | Skill-Bundle | 6 (ckm-banner-design, ckm-brand, ckm-design, ckm-design-system, ckm-slides, ckm-ui-styling) | Kein Plugin-Wrapper; Quelle: nextlevelbuilder/ui-ux-pro-max-skill |
| `inbox/ui-ux-pro-max/` | Skill-Bundle | 1 (ui-ux-pro-max/) | Kein Plugin-Wrapper; Quelle: nextlevelbuilder/ui-ux-pro-max-skill |
| `inbox/vercel-labs/` | Skill-Bundle | 1 (web-design-guidelines/) | Kein Plugin-Wrapper; Quelle: vercel-labs/agent-skills |

**Bestehendes Plugin:** `plugins/ui-ux/` (v1.0.0, 9 Skills: adaptive-communication, bencium-aeo, bencium-code-conventions, bencium-controlled-ux-designer, bencium-impact-designer, bencium-innovative-ux-designer, design-audit, human-architect-mindset, typography)

---

## Zuordnung (bestaetigt)

Alle Inbox-Items werden in das bestehende Plugin `plugins/ui-ux/` integriert.

| Inbox-Item | Ziel |
|------------|------|
| `inbox/design/skills/accessibility-review/` | `plugins/ui-ux/skills/accessibility-review/` |
| `inbox/design/skills/design-critique/` | `plugins/ui-ux/skills/design-critique/` |
| `inbox/design/skills/design-handoff/` | `plugins/ui-ux/skills/design-handoff/` |
| `inbox/design/skills/design-system/` | `plugins/ui-ux/skills/design-system/` |
| `inbox/design/skills/research-synthesis/` | `plugins/ui-ux/skills/research-synthesis/` |
| `inbox/design/skills/user-research/` | `plugins/ui-ux/skills/user-research/` |
| `inbox/design/skills/ux-copy/` | `plugins/ui-ux/skills/ux-copy/` |
| `inbox/claudekit/ckm-banner-design/` | `plugins/ui-ux/skills/ckm-banner-design/` |
| `inbox/claudekit/ckm-brand/` | `plugins/ui-ux/skills/ckm-brand/` |
| `inbox/claudekit/ckm-design/` | `plugins/ui-ux/skills/ckm-design/` |
| `inbox/claudekit/ckm-design-system/` | `plugins/ui-ux/skills/ckm-design-system/` |
| `inbox/claudekit/ckm-slides/` | `plugins/ui-ux/skills/ckm-slides/` |
| `inbox/claudekit/ckm-ui-styling/` | `plugins/ui-ux/skills/ckm-ui-styling/` |
| `inbox/ui-ux-pro-max/ui-ux-pro-max/` | `plugins/ui-ux/skills/ui-ux-pro-max/` |
| `inbox/vercel-labs/web-design-guidelines/` | `plugins/ui-ux/skills/web-design-guidelines/` |

Kein neues Plugin wird erstellt. Die `plugin.json` und `.mcp.json` von `inbox/design/` werden nicht uebernommen (Metadaten vom bestehenden ui-ux Plugin bleiben massgeblich).

Die Herkunftsmetadaten aus vorhandenen `plugin.json`-Dateien werden in einem README auf Plugin-Ebene dokumentiert (`plugins/ui-ux/README.md`), damit die Quelle jedes Skills nachvollziehbar ist.

> Versionsbump (zwei separate Dateien):
> - `plugins/ui-ux/.claude-plugin/plugin.json`: `1.0.0 -> 1.1.0`
> - `.claude-plugin/marketplace.json` Eintrag ui-ux: `1.0.0 -> 1.1.0`

---

## Skript-Spezifikation

### Pfad
`scripts/integrate_inbox.py`

### Interface

```
python scripts/integrate_inbox.py [OPTIONS]

Options:
  --marketplace-root PATH   Root des Marketplace (default: auto-detect via .claude-plugin/)
  --inbox PATH              Inbox-Verzeichnis (default: ./inbox)
  --dry-run                 Zeigt Plan, fuehrt keine Aenderungen aus
  --auto                    Keine interaktiven Prompts, Standardzuordnung verwenden
  --output-plan PATH        Schreibt Plan als JSON-Datei fuer manuelle Ausfuehrung
```

### Workflow (5 Phasen)

```
Phase 1: Scan
  scan_inbox(inbox_dir) -> Liste von InboxItem-Objekten
  classify_item(item) -> "full_plugin" | "standalone_skill" | "skill_bundle"

Phase 2: Suggest
  suggest_assignment(item, existing_plugins) -> Assignment(source, target, action, reasoning)

Phase 3: Review (interaktiv, ausser --auto)
  Zeigt Zuordnungstabelle, Nutzer bestaetigt / aendert / ueberspringt pro Item

Phase 4: Plan generieren
  generate_plan(assignments) -> Plan mit file_ops + manifest_updates + readme_updates + mcp_updates
  Zeigt Vorschau als lesbares Diff

Phase 5: Ausfuehren oder Exportieren
  --dry-run: nur anzeigen
  --output-plan: JSON-Export fuer manuelle Ausfuehrung
  sonst: execute_plan() mit shutil.copytree + JSON-Patch auf marketplace.json
```

### Kern-Operationen

**Skills aus Full-Plugin (`inbox/design/`):**
1. Skill-Verzeichnisse einzeln kopieren: `inbox/design/skills/*` -> `plugins/ui-ux/skills/`
2. `plugins/ui-ux/.claude-plugin/plugin.json` version bumpen (minor)
3. marketplace.json ui-ux-Eintrag version aktualisieren
4. README auf Plugin-Ebene erstellen/aktualisieren (siehe unten)

**Skill-Bundle (`inbox/claudekit/` mit 6 Skills):**
1. Fuer jeden Skill im Bundle: `shutil.copytree(skill_src, skill_dst)` (mit Pre-Check: skip if exists)
2. `plugins/ui-ux/.claude-plugin/plugin.json` version bumpen (minor) – einmalig, nicht pro Skill
3. marketplace.json ui-ux-Eintrag version aktualisieren
4. README auf Plugin-Ebene erstellen/aktualisieren (siehe unten)

**README-Generierung (`plugins/ui-ux/README.md`):**

Das Skript erstellt oder aktualisiert eine README mit einem `## Sources`-Abschnitt.
Fuer jeden kopierten Skill wird die Herkunft eingetragen:
- Wenn die Quelle eine `plugin.json` hat: Name, Version, Author, Description aus der JSON
- Wenn keine `plugin.json` vorhanden: GitHub-Quelle aus skills-lock.json oder Inbox-Pfad

Beispiel-Output in README:
```markdown
## Sources

| Skill | Source Plugin | Version | Author | Description |
|-------|--------------|---------|--------|-------------|
| accessibility-review | [Anthropic Design Plugin](https://github.com/anthropics) | 1.2.0 | Anthropic | Design critique, system management, UX writing, accessibility, research |
| design-critique | Anthropic Design Plugin | 1.2.0 | Anthropic | Structured design feedback |
| design-handoff | Anthropic Design Plugin | 1.2.0 | Anthropic | Developer handoff specs |
| design-system | Anthropic Design Plugin | 1.2.0 | Anthropic | Audit and extend design systems |
| research-synthesis | Anthropic Design Plugin | 1.2.0 | Anthropic | Synthesize user research |
| user-research | Anthropic Design Plugin | 1.2.0 | Anthropic | Plan and conduct research |
| ux-copy | Anthropic Design Plugin | 1.2.0 | Anthropic | UX microcopy, error messages, onboarding |
| ckm-banner-design | [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | v2.x | nextlevelbuilder | Design intelligence for UI/UX: 50+ styles, 161 palettes, 57 font pairings |
| ckm-brand | nextlevelbuilder/ui-ux-pro-max-skill | v2.x | nextlevelbuilder | Brand identity and design tokens |
| ckm-design | nextlevelbuilder/ui-ux-pro-max-skill | v2.x | nextlevelbuilder | Core design skill: logos (55 styles), CIP (50 deliverables) |
| ckm-design-system | nextlevelbuilder/ui-ux-pro-max-skill | v2.x | nextlevelbuilder | Design system components |
| ckm-slides | nextlevelbuilder/ui-ux-pro-max-skill | v2.x | nextlevelbuilder | Slide creation and presentation design |
| ckm-ui-styling | nextlevelbuilder/ui-ux-pro-max-skill | v2.x | nextlevelbuilder | UI styling guidelines |
| ui-ux-pro-max | nextlevelbuilder/ui-ux-pro-max-skill | v2.x | nextlevelbuilder | 50+ styles, 161 palettes, 99 UX rules, 25 chart types |
| web-design-guidelines | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) | latest | Vercel Labs | Audits UI code against Vercel Web Interface Guidelines (100+ rules) |
```

Wenn `README.md` bereits existiert: vorhandenen `## Sources`-Abschnitt ersetzen (Grenze: naechster `##`-Header oder EOF), Rest unveraendert lassen. Wenn nicht vorhanden: Neue README mit `## Sources` erstellen.

**MCP-Uebertragung:**

Wenn eine Inbox-Quelle eine `.mcp.json` besitzt (z.B. `inbox/design/.mcp.json`),
werden deren `mcpServers`-Eintraege in die `.mcp.json` des Ziel-Plugins uebernommen.

Vorgehen:
- Existiert `plugins/ui-ux/.mcp.json` bereits: Server zusammenfuehren (vorhandene behalten,
  neue hinzufuegen, Duplikate nach Name ueberspringen – vorhandener Eintrag hat Vorrang)
- Existiert sie nicht: `.mcp.json` direkt kopieren

`inbox/design/.mcp.json` enthaelt 9 Server:
`slack, figma, linear, asana, atlassian, notion, intercom, google-calendar, gmail`

### Datenstrukturen

```python
@dataclass
class InboxItem:
    path: Path
    name: str
    item_type: str      # "full_plugin" | "standalone_skill" | "skill_bundle"
    skills: list[Path]  # Pfade zu Skill-Verzeichnissen (mit SKILL.md)
    plugin_meta: dict | None  # Inhalt von plugin.json falls vorhanden

@dataclass
class Assignment:
    item: InboxItem
    action: str         # "add_to_plugin" | "skip"
    target_plugin: str  # z.B. "ui-ux"
    target_plugin_path: Path
    reasoning: str

@dataclass
class FileOp:
    op: str   # "copy_dir"
    src: Path
    dst: Path

@dataclass
class ManifestUpdate:
    file: Path
    description: str
    before: dict
    after: dict

@dataclass
class ReadmeUpdate:
    file: Path          # z.B. plugins/ui-ux/README.md
    sources: list[dict] # [{"skill": ..., "source_plugin": ..., "version": ..., "author": ...}]

@dataclass
class McpUpdate:
    file: Path          # z.B. plugins/ui-ux/.mcp.json
    servers_to_add: dict  # {"slack": {...}, ...}

@dataclass
class Plan:
    assignments: list[Assignment]
    file_ops: list[FileOp]
    manifest_updates: list[ManifestUpdate]
    readme_updates: list[ReadmeUpdate]
    mcp_updates: list[McpUpdate]
```

### Beispiel-Output (--dry-run)

```
INBOX INTEGRATION PLAN (DRY RUN)
=================================
Gefunden: 4 Inbox-Items, 15 neue Skills gesamt

Item                        Typ           Skills  Zuordnung
--------------------------  ------------  ------  ----------------------------------------
inbox/design/               full_plugin   7       -> plugins/ui-ux/skills/ (7 skills)
inbox/claudekit/            skill_bundle  6       -> plugins/ui-ux/skills/ (6 ckm-skills)
inbox/ui-ux-pro-max/        skill_bundle  1       -> plugins/ui-ux/skills/ui-ux-pro-max/
inbox/vercel-labs/          skill_bundle  1       -> plugins/ui-ux/skills/web-design-guidelines/

Validierung (vor dem Kopieren):
  WARN  inbox/claudekit/ckm-design/scripts/logo.py: DANGEROUS PATTERN 'subprocess.run'
  Fortfahren? [y/N]:

Dateioperationen (15 Skill-Verzeichnisse):
  COPY  inbox/design/skills/accessibility-review/  ->  plugins/ui-ux/skills/accessibility-review/
  ... (7 total aus inbox/design/)
  COPY  inbox/claudekit/ckm-banner-design/         ->  plugins/ui-ux/skills/ckm-banner-design/
  COPY  inbox/claudekit/ckm-brand/                 ->  plugins/ui-ux/skills/ckm-brand/
  COPY  inbox/claudekit/ckm-design/                ->  plugins/ui-ux/skills/ckm-design/
  COPY  inbox/claudekit/ckm-design-system/         ->  plugins/ui-ux/skills/ckm-design-system/
  COPY  inbox/claudekit/ckm-slides/                ->  plugins/ui-ux/skills/ckm-slides/
  COPY  inbox/claudekit/ckm-ui-styling/            ->  plugins/ui-ux/skills/ckm-ui-styling/
  COPY  inbox/ui-ux-pro-max/ui-ux-pro-max/         ->  plugins/ui-ux/skills/ui-ux-pro-max/
  COPY  inbox/vercel-labs/web-design-guidelines/   ->  plugins/ui-ux/skills/web-design-guidelines/

Manifest-Aenderungen:
  plugins/ui-ux/.claude-plugin/plugin.json:  version 1.0.0 -> 1.1.0
  .claude-plugin/marketplace.json:           ui-ux version 1.0.0 -> 1.1.0

MCP-Aenderungen:
  plugins/ui-ux/.mcp.json (NEU): slack, figma, linear, asana, atlassian, notion, intercom, google-calendar, gmail

README-Aenderung:
  plugins/ui-ux/README.md: ## Sources Abschnitt (15 neue Skills mit Herkunft)

Ausfuehren mit:  python scripts/integrate_inbox.py --auto
Plan speichern:  python scripts/integrate_inbox.py --output-plan plan.json
```

---

## Post-Integration: Manuelle Nacharbeiten

Diese Schritte werden vom Skript vorbereitet / markiert, aber manuell ausgefuehrt:

### 1. Neue plugin.json-Beschreibung

Die Beschreibung in `plugins/ui-ux/.claude-plugin/plugin.json` soll den erweiterten Umfang
(24 Skills statt 9: 9 original + 15 neu) widerspiegeln. Das Skript gibt einen Vorschlag aus.

**Vorher:** `"description": "Curated set of high quality skills for ui and ux related task like creating design systems."`
**Vorschlag:** Eine aktualisierte Beschreibung, die alle Skill-Kategorien abdeckt:
Design Critique, System Design, UX Research, Accessibility, Branding, UI Styling, Slides,
Web Guidelines, Pro Patterns.

### 2. README auf Plugin-Ebene vervollstaendigen

`plugins/ui-ux/README.md` wird vom Skript mit einem `## Sources`-Abschnitt generiert.
Zusaetzlich soll die README manuell um folgende Abschnitte ergaenzt werden:
- `## Overview` – Was kann dieses Plugin? Welche Aufgabenfelder deckt es ab?
- `## Skills` – Kurzbeschreibung jedes Skills (Name, Zweck, wann einsetzen)
- `## Sources` – automatisch generiert (Herkunftstabelle)

### 3. advise-design Skill erstellen

Neuer Skill: `plugins/ui-ux/skills/advise-design/SKILL.md`

**Zweck:** Gibt einen Ueberblick ueber alle verfuegbaren Skills im ui-ux Plugin und empfiehlt,
welcher Skill fuer welche Aufgabe am besten geeignet ist.

**Funktion:**
- Liest die `README.md` auf Plugin-Ebene (oder die SKILL.md-Dateien direkt) als Wissensquelle
- Analysiert die Aufgabenbeschreibung des Nutzers
- Empfiehlt den passenden Skill(s) mit Begruendung

**Trigger:** "Welchen Design-Skill soll ich verwenden?", "Ich moechte ein Design-System erstellen", etc.

**SKILL.md Struktur:**
```markdown
---
name: advise-design
description: Recommends which ui-ux skill to use for a given design task.
  Use when: user asks which design skill to use, or describes a design task
  and needs guidance.
---

# Design Skill Advisor

## Available Skills
[Uebersicht aller Skills mit Zweck und Triggerbedingungen]

## How to Choose
[Entscheidungsbaum: Aufgabe -> empfohlener Skill]
```

Das Skript erstellt ein Skelett dieser Datei mit den gefundenen Skills. Der Inhalt
(Beschreibungen und Empfehlungen) muss manuell vervollstaendigt werden.

---

## Betroffene Dateien

| Datei | Aktion |
|-------|--------|
| `scripts/integrate_inbox.py` | NEU erstellen |
| `.claude-plugin/marketplace.json` | version ui-ux bump: `1.0.0 -> 1.1.0` |
| `plugins/ui-ux/.claude-plugin/plugin.json` | version bump `1.0.0 -> 1.1.0` |
| `plugins/ui-ux/README.md` | NEU: Sources-Abschnitt mit Herkunftsmetadaten |
| `plugins/ui-ux/.mcp.json` | NEU: 9 MCP-Server aus inbox/design/.mcp.json |
| `plugins/ui-ux/skills/accessibility-review/` | NEU (aus inbox/design/) |
| `plugins/ui-ux/skills/design-critique/` | NEU (aus inbox/design/) |
| `plugins/ui-ux/skills/design-handoff/` | NEU (aus inbox/design/) |
| `plugins/ui-ux/skills/design-system/` | NEU (aus inbox/design/) |
| `plugins/ui-ux/skills/research-synthesis/` | NEU (aus inbox/design/) |
| `plugins/ui-ux/skills/user-research/` | NEU (aus inbox/design/) |
| `plugins/ui-ux/skills/ux-copy/` | NEU (aus inbox/design/) |
| `plugins/ui-ux/skills/ckm-banner-design/` | NEU (aus inbox/claudekit/) |
| `plugins/ui-ux/skills/ckm-brand/` | NEU (aus inbox/claudekit/) |
| `plugins/ui-ux/skills/ckm-design/` | NEU (aus inbox/claudekit/) |
| `plugins/ui-ux/skills/ckm-design-system/` | NEU (aus inbox/claudekit/) |
| `plugins/ui-ux/skills/ckm-slides/` | NEU (aus inbox/claudekit/) |
| `plugins/ui-ux/skills/ckm-ui-styling/` | NEU (aus inbox/claudekit/) |
| `plugins/ui-ux/skills/ui-ux-pro-max/` | NEU (aus inbox/ui-ux-pro-max/) |
| `plugins/ui-ux/skills/web-design-guidelines/` | NEU (aus inbox/vercel-labs/) |
| `plugins/ui-ux/skills/advise-design/SKILL.md` | NEU: Skill-Advisor Skelett (manuell vervollstaendigen) |

---

## Edge Cases & Fehlerbehandlung

| Situation | Verhalten |
|-----------|-----------|
| Skill-Verzeichnis existiert bereits im Ziel | Ueberspringen (SKIP) mit Hinweis – kein Ueberschreiben |
| Inbox-Verzeichnis existiert nicht | Abbruch mit klarer Fehlermeldung (Exit 1) |
| `plugin.json` / `.mcp.json` ist invalid JSON | Abbruch mit Dateiname und Fehlerdetail |
| MCP-Server-Name existiert bereits (gleicher oder anderer Inhalt) | Ueberspringen – vorhandener Eintrag hat immer Vorrang |
| marketplace.json enthaelt kein Ziel-Plugin | Abbruch mit Fehlermeldung |
| Dateisystem-Fehler mid-copy | Meldung welche Files bereits kopiert wurden; kein Auto-Rollback |
| Scan findet Items ohne bekannte Zuordnung | Im interaktiven Modus: Nutzer fragt; im --auto Modus: warnen + ueberspringen |

**Idempotenz:** Pre-Check `if dst.exists(): skip` vor jeder Kopier-Operation. Zweiter Lauf ist sicher.

**Rollback:** Kein automatischer Rollback. Abschlussmeldung zeigt:
- Kopiert (N): Liste der neuen Skills
- Uebersprungen (N): bereits vorhandene
- Fehlgeschlagen (N): Fehler mit Details

**README-Abschnittsgrenzen:** `## Sources`-Block endet beim naechsten `##`-Header oder EOF.

**Python-Version:** 3.8+

---

## Entschiedene Parameter

| Frage | Entscheidung |
|-------|-------------|
| Versionsstrategie ui-ux | `1.0.0 -> 1.1.0` (ein Minor-Bump fuer alle 15 neuen Skills) |
| ckm Skills Umfang | Alles kopieren: SKILL.md + references/ + scripts/ + data/ |
| design Plugin .mcp.json | Uebertragen als .mcp.json in plugins/ui-ux/ |
| claudekit Skills | Alle 6 integrieren |
| Skript-Typ | Allgemeines, wiederverwendbares Tool (nicht einmaliges Migrationsskript) |

---

## Quality Gates

### QG-1: TDD vor der Implementierung
**Skill:** `superpowers:test-driven-development`

| Funktion | Testfall |
|----------|----------|
| `scan_inbox()` | Leere inbox -> leere Liste |
| `scan_inbox()` | Full Plugin (mit plugin.json) -> korrekte Klassifizierung |
| `scan_inbox()` | Skill-Bundle (inbox/claudekit/ mit 6 ckm-Skills) -> 1 InboxItem mit 6 skills |
| `suggest_assignment()` | Bekannter Plugin -> korrekte Zuordnung |
| `suggest_assignment()` | Unbekannter Plugin -> Warning, skip im --auto Modus |
| `generate_plan()` | 4 Items / 15 Skills -> 15 FileOps + 2 ManifestUpdates + 1 McpUpdate + 1 ReadmeUpdate |
| `execute_plan()` | Ziel existiert bereits -> Skip, kein Ueberschreiben |
| `bump_minor()` | `1.0.0` -> `1.1.0`, `1.9.0` -> `1.10.0` |

### QG-2: Skill-Validierung im Skript (eingebaut)

```python
def validate_skill(skill_path: Path) -> list[str]:
    """Returns list of warnings/errors. Empty = valid."""
    issues = []
    if not (skill_path / "SKILL.md").exists():
        issues.append(f"MISSING SKILL.md in {skill_path}")
    DANGEROUS_PATTERNS = [
        r"subprocess\.(run|Popen|call|check_output)",
        r"os\.system\(",
        r"eval\(",
        r"exec\(",
        r"__import__\(",
        r"requests\.(get|post|put|delete)",
        r"urllib\.request\.",
    ]
    for script in skill_path.rglob("*.py"):
        content = script.read_text()
        for pattern in DANGEROUS_PATTERNS:
            if re.search(pattern, content):
                issues.append(f"DANGEROUS PATTERN '{pattern}' in {script}")
    return issues
```

Bei Warnings: anzeigen, Nutzer bestaetigt oder ueberspringt.
Bei Errors (fehlendes SKILL.md): Skill wird nicht kopiert.

### QG-3: skill-scanner nach Integration
**Agent:** `plugin-dev:skill-reviewer` + `skill-scanner:skill-scanner`

Nach Ausfuehrung: alle 15 neuen Skills scannen.

### QG-4: Code-Review nach Implementierung
**Agents (parallel):**
- `compound-engineering:review:correctness-reviewer`
- `compound-engineering:review:security-reviewer`

---

## Verifikation

**Skript (automatisch):**
1. `python scripts/integrate_inbox.py --dry-run` – Plan anzeigen (15 neue Skills erwartet)
2. `python scripts/integrate_inbox.py --auto` – Ausfuehren
3. `ls plugins/ui-ux/skills/ | wc -l` – 25 Skills? (9 original + 15 neu + 1 advise-design)
4. `cat .claude-plugin/marketplace.json` – ui-ux version 1.1.0?
5. `cat plugins/ui-ux/.claude-plugin/plugin.json` – version 1.1.0?
6. `cat plugins/ui-ux/.mcp.json` – 9 MCP-Server vorhanden?
7. `cat plugins/ui-ux/README.md` – Sources-Tabelle mit allen 15 neuen Skills?

**Manuelle Nacharbeiten:**
8. `plugins/ui-ux/.claude-plugin/plugin.json` – Beschreibung aktualisiert?
9. `plugins/ui-ux/README.md` – Overview und Skills-Abschnitte vervollstaendigt?
10. `plugins/ui-ux/skills/advise-design/SKILL.md` – Inhalt manuell ergaenzt?
