# Plan: Plugin-Profile → Eigener Marketplace

## Context

**Problem**: Das aktuelle `plugin-profile` System verwaltet Skills manuell via Symlinks aus `~/.agents/skills/` und toggled Plugins in `settings.json`. Es fehlt: Discovery, Versionierung, Kuration, einfache Installation.

**Ziel**: Ein eigener Marketplace als GitHub-Repository, installierbar via `claude plugin marketplace add think-slick/curated-marketplace-knowledge-worker`. Enthält eigene + kuratierte Third-Party Skills. Das Bundle-System steuert weiterhin auch externe Plugins via `enabledPlugins`.

**Repository**: https://github.com/think-slick/curated-marketplace-knowledge-worker

**Inspiration**: Trail of Bits `skills-curated` — Format, Struktur und Security-Scanner.

**KWPM**: Bleibt als separates Projekt. Die SaaS-Specs können später als Web-Frontend für den Marketplace dienen.

---

## Architektur-Übersicht

```
NEUES REPO: think-slick/curated-marketplace-knowledge-worker
├── .claude-plugin/marketplace.json    ← Claude Code erkennt dies als Marketplace
├── plugins/
│   ├── eigene-skills/                 ← Migriert aus ~/.agents/skills/
│   └── kuratierte-third-party/        ← Geprüfte externe Skills
├── bundles/bundles.json               ← Ersetzt profiles.json
└── scripts/                           ← Security-Scanner, Validierung

BESTEHENDES: plugin-profile Skill
├── SKILL.md                           ← Angepasst: liest bundles.json aus Marketplace-Repo
├── profiles.json                      ← Archiviert, ersetzt durch bundles.json
└── Logik: togglet enabledPlugins      ← Bleibt für externe Plugins (superpowers, bencium, etc.)
```

### Datenfluss nach Migration

```
User: /plugin-profile saas
  │
  ├─→ Lese bundles.json aus ~/.claude/plugins/cache/<marketplace>/bundles/
  │
  ├─→ Eigene Marketplace-Plugins: bereits installiert durch Marketplace → nichts zu tun
  │
  ├─→ Externe Plugins (superpowers@..., bencium@..., vercel@...):
  │     → Toggle enabledPlugins in ~/.claude/settings.json (wie heute)
  │
  └─→ Zeige Zusammenfassung
```

---

## Repository-Struktur

```
curated-marketplace-knowledge-worker/
├── .claude-plugin/
│   └── marketplace.json              # Zentrale Registry (ToB-Format)
├── plugins/
│   ├── next-best-practices/          # Migriert aus ~/.agents/skills/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json           # { name, version, description, author }
│   │   ├── skills/
│   │   │   └── next-best-practices/
│   │   │       ├── SKILL.md
│   │   │       └── references/       # Bisherige .md-Dateien
│   │   └── README.md
│   ├── ui-ux-pro-max/
│   ├── vercel-composition-patterns/
│   ├── vercel-react-best-practices/
│   ├── vercel-react-native-skills/
│   ├── web-design-guidelines/
│   ├── deploy-to-vercel/
│   ├── saas-launch-audit/
│   ├── audit-saas-gtm/
│   ├── seo-audit/
│   ├── find-skills/
│   ├── plugin-profile/               # Der Manager selbst als Plugin
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/plugin-profile/SKILL.md
│   │   └── README.md
│   └── [kuratierte-third-party]/     # Phase 2+
├── bundles/
│   └── bundles.json                  # Ersetzt profiles.json
├── scripts/
│   ├── scan_plugin.py                # Fork von Trail of Bits
│   ├── convert_skill.sh              # Konvertiert ~/.agents/skills/ → Plugin-Format
│   └── validate_marketplace.py       # Prüft Version-Sync + Struktur
├── docs/
│   └── plan.md                       # Dieser Plan
├── .github/
│   ├── workflows/ci.yml
│   └── CODEOWNERS
├── CLAUDE.md
├── README.md
└── LICENSE
```

---

## bundles.json Format (ersetzt profiles.json)

```json
{
  "bundles": {
    "minimal": {
      "description": "Core essentials only (~5 plugins, ~15KB context)",
      "marketplace_plugins": [],
      "external_plugins": [
        "superpowers@superpowers-marketplace",
        "commit-commands@claude-plugins-official",
        "semgrep@claude-plugins-official",
        "explanatory-output-style@claude-plugins-official",
        "claude-md-management@claude-plugins-official"
      ]
    },
    "highquality": {
      "description": "Universal curated best-of set (~14 plugins, ~35KB context)",
      "extends": "minimal",
      "marketplace_plugins": [],
      "external_plugins": [
        "compound-engineering@compound-engineering-plugin",
        "everything-claude-code@everything-claude-code",
        "code-review@claude-plugins-official",
        "feature-dev@claude-plugins-official",
        "voltagent-core-dev@voltagent-subagents",
        "voltagent-qa-sec@voltagent-subagents",
        "taches-cc-resources@taches-cc-resources",
        "elements-of-style@superpowers-marketplace",
        "productivity@knowledge-work-plugins"
      ]
    },
    "saas": {
      "description": "Next.js/Vercel SaaS development (~16 plugins, ~40KB context)",
      "extends": "highquality",
      "marketplace_plugins": ["next-best-practices", "vercel-composition-patterns"],
      "external_plugins": [
        "vercel@claude-plugins-official",
        "playwright@claude-plugins-official"
      ]
    },
    "uiux": {
      "description": "UI/UX Design — colors, typography, a11y, components (~21 plugins, ~50KB context)",
      "extends": "highquality",
      "marketplace_plugins": ["ui-ux-pro-max", "vercel-composition-patterns", "seo-audit"],
      "external_plugins": [
        "frontend-design@claude-plugins-official",
        "bencium-innovative-ux-designer@bencium-marketplace",
        "design-audit@bencium-marketplace",
        "typography@bencium-marketplace",
        "a11y-audit@claude-code-skills",
        "web-dev-tools@claudekit-skills",
        "playwright@claude-plugins-official"
      ]
    },
    "landingpage": {
      "description": "Landing Page creation — design + content + SEO + deploy (~26 plugins, ~60KB context)",
      "extends": "uiux",
      "marketplace_plugins": ["next-best-practices", "audit-saas-gtm"],
      "external_plugins": [
        "vercel@claude-plugins-official",
        "content-creator@claude-code-skills",
        "marketing-skills@claude-code-skills",
        "bencium-aeo@bencium-marketplace",
        "bencium-code-conventions@bencium-marketplace"
      ]
    },
    "founder": {
      "description": "Startup/Founder — business, product, marketing, research (~24 plugins, ~55KB context)",
      "extends": "highquality",
      "marketplace_plugins": ["audit-saas-gtm"],
      "external_plugins": [
        "business-growth-skills@claude-code-skills",
        "c-level-skills@claude-code-skills",
        "marketing-skills@claude-code-skills",
        "content-creator@claude-code-skills",
        "pm-skills@claude-code-skills",
        "product-manager@claude-code-skills",
        "product-skills@claude-code-skills",
        "voltagent-biz@voltagent-subagents",
        "scrum-master@claude-code-skills",
        "voltagent-research@voltagent-subagents"
      ]
    },
    "research": {
      "description": "Research and analysis workflow (~9 plugins, ~20KB context)",
      "extends": "minimal",
      "marketplace_plugins": [],
      "external_plugins": [
        "voltagent-research@voltagent-subagents",
        "voltagent-biz@voltagent-subagents",
        "autoresearch-agent@claude-code-skills",
        "research-summarizer@claude-code-skills"
      ]
    },
    "fullstack": {
      "description": "Full engineering stack (~34 plugins, ~70KB context)",
      "extends": "saas",
      "marketplace_plugins": [],
      "external_plugins": [
        "voltagent-biz@voltagent-subagents",
        "voltagent-research@voltagent-subagents",
        "voltagent-meta@voltagent-subagents",
        "voltagent-lang@voltagent-subagents",
        "voltagent-dev-exp@voltagent-subagents",
        "voltagent-infra@voltagent-subagents",
        "voltagent-data-ai@voltagent-subagents",
        "ai-ml-tools@claudekit-skills",
        "backend-tools@claudekit-skills",
        "debugging-tools@claudekit-skills",
        "devops-tools@claudekit-skills",
        "problem-solving-tools@claudekit-skills",
        "research-tools@claudekit-skills",
        "specialized-tools@claudekit-skills",
        "web-dev-tools@claudekit-skills",
        "fullstack-engineer@claude-code-skills",
        "engineering-skills@claude-code-skills",
        "engineering-advanced-skills@claude-code-skills"
      ]
    },
    "all": {
      "description": "Everything enabled (~72 plugins, ~130KB context)",
      "marketplace_plugins": "__ALL__",
      "external_plugins": "__ALL__"
    }
  },
  "addons": {
    "ralph": {
      "description": "Ralph Loop — autonomous implementation workflow",
      "external_plugins": ["ralph-loop@claude-plugins-official"]
    }
  },
  "projectTypes": {
    "saas": { "bundle": "saas", "label": "SaaS", "description": "Next.js/Vercel SaaS-Anwendung" },
    "landingpage": { "bundle": "landingpage", "label": "Landing Page", "description": "Landing Page — Design + Content + SEO + Deploy" },
    "uiux": { "bundle": "uiux", "label": "UI/UX", "description": "UI/UX Design — Farben, Typografie, a11y, Komponenten" },
    "api": { "bundle": "highquality", "label": "API / Backend", "description": "API oder Backend-Service ohne Frontend" },
    "research": { "bundle": "research", "label": "Research", "description": "Recherche- und Analyse-Workflow" },
    "fullstack": { "bundle": "fullstack", "label": "Full Stack", "description": "Kompletter Engineering-Stack" },
    "custom": { "bundle": "minimal", "label": "Custom", "description": "Minimale Basis — manuell erweitern" }
  }
}
```

**Schlüsseländerung vs. profiles.json**: `plugins` aufgeteilt in `marketplace_plugins` (Name reicht, da im gleichen Repo) und `external_plugins` (mit `@marketplace`-Suffix). Die alte `skills`-Liste fällt weg — Marketplace-Plugins liefern ihre Skills automatisch.

---

## Änderungen am plugin-profile SKILL.md

### Was bleibt
- Bundle-Hierarchie mit `extends`
- `enabledPlugins`-Manipulation in `~/.claude/settings.json` für externe Plugins
- Backup-Mechanik vor Änderungen
- Kommando-Routing (list, show, apply, init, backup)

### Was sich ändert
- **Datenquelle**: Statt `profiles.json` → `bundles.json` aus Marketplace-Repo-Cache
- **Pfad**: `~/.claude/plugins/cache/<marketplace-hash>/bundles/bundles.json`
- **Symlink-Management entfällt komplett**: Marketplace liefert Skills automatisch
- **`marketplace_plugins`-Info**: Werden beim `apply` nur angezeigt ("Diese Marketplace-Skills sind verfügbar"), nicht aktiv installiert (sind bereits da)
- **Kommando-Alias**: `/plugin-profile` bleibt, `/plugin bundle` als neuer Alias

### Was neu kommt
- `validate`-Befehl: Prüft ob Marketplace installiert und aktuell
- Erkennung des Marketplace-Cache-Pfads (dynamisch, nicht hardcoded)

---

## Kritische Dateien

| Datei | Aktion |
|-------|--------|
| `~/.claude/skills/plugin-profile/SKILL.md` | Anpassen: bundles.json-Pfad, Symlink-Logik entfernen |
| `~/.claude/skills/plugin-profile/profiles.json` | Archivieren → wird zu `bundles/bundles.json` im neuen Repo |
| `~/.agents/skills/*` (10 Skill-Verzeichnisse) | Konvertieren ins Plugin-Format → `plugins/` im neuen Repo |
| KWPM `specs/001-kwpm-mvp/contracts/plugin-manifest.md` | Referenz für plugin.json Schema (bereits kompatibel) |

---

## Implementierung in 5 Phasen

### Phase 1: Repository Setup (1 Session)

1. `.claude-plugin/marketplace.json` mit leerer `plugins`-Liste anlegen
2. `bundles/bundles.json` aus `profiles.json` konvertieren (vollständiges Mapping oben)
3. `scripts/convert_skill.sh` schreiben — automatisiert Skill→Plugin Konvertierung:
   - SKILL.md Frontmatter parsen → plugin.json generieren
   - Verzeichnisstruktur umbauen (SKILL.md → `skills/<name>/SKILL.md`, Rest → `references/`)
   - README.md aus Frontmatter generieren
   - marketplace.json-Eintrag hinzufügen
4. `README.md` + `CLAUDE.md` + `LICENSE` anlegen

### Phase 2: Skills migrieren (1 Session)

5. `convert_skill.sh` auf alle 10 Skills in `~/.agents/skills/` anwenden
6. Ergebnisse nach `plugins/` kopieren
7. `marketplace.json` mit allen Plugins befüllen
8. `plugin-profile` selbst als Plugin in den Marketplace aufnehmen
9. Manuell prüfen: Jedes Plugin hat gültige Struktur

### Phase 3: plugin-profile SKILL.md anpassen (1 Session)

10. SKILL.md: `profiles.json`-Logik → `bundles.json`-Logik umschreiben
11. Symlink-Management komplett entfernen
12. Marketplace-Cache-Pfad-Erkennung einbauen
13. `validate`-Befehl hinzufügen
14. Testen: `/plugin-profile list`, `/plugin-profile saas`

### Phase 4: CI & Security (1 Session)

15. `scan_plugin.py` von Trail of Bits forken und anpassen
16. `validate_marketplace.py` schreiben (Version-Sync, Struktur-Prüfung)
17. GitHub Actions CI: beide Scanner auf jedem PR
18. `CODEOWNERS` anlegen

### Phase 5: Test & Go-Live (1 Session)

19. Repo pushen
20. `claude plugin marketplace add think-slick/curated-marketplace-knowledge-worker` testen
21. `/plugin-profile list` → Bundle-Liste aus Marketplace
22. `/plugin-profile saas` → externe Plugins togglen + Marketplace-Plugins anzeigen
23. In Cowork testen
24. `~/.agents/skills/`-Verzeichnis archivieren

---

## Post-MVP (kann warten)

- **Third-Party Kuration**: Geprüfte Skills von anderen Quellen in den Marketplace aufnehmen
- **KWPM als Web-Frontend**: Next.js App die `marketplace.json` liest und als browsbare Webseite rendert
- **Personalisierungs-Quiz**: KWPM User Story 1 als Marketplace-Feature ("Welches Bundle passt zu dir?")
- **Update-Notifications**: Automatische Benachrichtigung bei neuen Plugin-Versionen
- **PR-Template**: Strukturiertes Template für Third-Party Submissions
- **Download-Analytics**: Tracking welche Plugins wie oft installiert werden

---

## Verifikation

1. `claude plugin marketplace add think-slick/curated-marketplace-knowledge-worker` → Marketplace wird erkannt und Plugins verfügbar
2. `/plugin-profile list` → Zeigt Bundles aus `bundles.json`
3. `/plugin-profile saas` → Toggled externe Plugins in `settings.json`, zeigt Marketplace-Plugins an
4. Jedes Plugin im Marketplace → `scan_plugin.py` ohne Findings
5. Version-Sync → `validate_marketplace.py` ohne Fehler
6. In frischer Claude Code Session nach Marketplace-Install → Skills aus Marketplace verfügbar
