# Plan: Skill Collection als agenten-lesbaren Katalog neu strukturieren

## Enhancement Summary

**Deepened on:** 2026-03-25
**Research agents used:** 6 (Simplicity Reviewer, SKILL.md Conventions, Agent-Native Reviewer, Architecture Strategist, Pattern Recognition, Best Practices Researcher)
**Web research:** Anthropic official SKILL.md spec, Skill authoring best practices, Claude Code skills architecture

### Key Improvements (vs. Original Plan)
1. **JSON als Primary Format** statt Markdown-Tabellen → Agent liest 1 Datei statt 14
2. **`when_to_use` Feld pro Skill** → ermöglicht Intent-Matching ("help me deploy" → deploy-to-vercel)
3. **Flache Struktur statt Verzeichnishierarchie** → Kategorien als Tag, nicht als Ordner
4. **Kein Fake-SKILL.md-Frontmatter** → INDEX.md ≠ SKILL.md (offizielle Spezifikation)
5. **Markdown als generierte View** → nicht als Source of Truth

### Critical Findings from Research
- **Anthropic Official Spec:** SKILL.md Frontmatter hat NUR `name` + `description`. Kein `metadata`, `tags`, `category`. Diese Felder werden vom Runtime ignoriert.
- **Agent-Native Review:** Markdown-Tabellen sind das schlechteste Format für Agent-Parsing (Pipe-Escaping, Link-Syntax, Bold-Stripping). JSON = direkte Feld-Zugriffe.
- **Simplicity Review:** 14 Dateien für 173 Einträge = Over-Engineering. Ein File reicht.
- **SKILL.md-Konventionen:** Die `description` ist der EINZIGE Discovery-Mechanismus. Tags existieren nicht im Runtime. Descriptions müssen "pushy" sein — Trigger-Phrasen und Synonyme.

---

## Context

Die `Claude Skills Collection.md` ist eine monolithische 57KB-Datei mit 173 Skills in 13 Kategorien. Sie ist für Menschen lesbar (Markdown-Tabellen), aber nicht für Agenten optimiert. Ziel: Umstrukturieren in ein Format, das ein Agent effizient durchsuchen und nutzen kann.

## Architektur-Entscheidungen (überarbeitet nach Deepening)

### 1. Separater `catalog/`-Ordner (nicht in `.agents/skills/`)
- `.agents/skills/` = installierte, ausführbare Skills mit SKILL.md + rules
- `catalog/` = Discovery-Index mit Referenzen auf externe Skills
- Klare Trennung vermeidet Verschmutzung des aktiven Skill-Discovery

### 2. JSON als Primary, Markdown als Generated View
**Begründung (Agent-Native Review):** Ein Agent, der "finde einen Terraform-Skill" beantworten soll, müsste bei Markdown-Tabellen:
1. Master-INDEX.md lesen → Kategorie erraten → Kategorie-INDEX.md öffnen → Tabelle parsen → Zeilen matchen = 3-5 File-Reads.

Mit `catalog.json`: 1 File-Read, 1 grep/search. Fertig.

Markdown-Tabellen haben zusätzlich Parsing-Probleme: Pipe-Escaping, Bold-Stripping, URL-Extraktion aus `[Link](url)`. JSON hat keine Ambiguität.

### 3. Flache Skill-Liste mit Tags statt Verzeichnishierarchie
**Begründung (Agent-Native Review):** Feste Kategorie-Ordner schaffen harte Grenzen, die Cross-Category-Discovery verhindern. "Terraform" könnte in Development, DevOps oder Utility sein. Ein Agent müsste alle 13 Ordner durchsuchen.

Stattdessen: Flache Liste in `catalog.json`, Kategorie als Feld/Tag. Cross-Category-Suche = ein einziger Durchlauf.

### 4. `when_to_use` Feld pro Skill
**Begründung (Agent-Native Review + Anthropic Best Practices):** Ohne Trigger-Patterns kann ein Agent "help me set up infrastructure as code" nicht zu Terraform-Skills matchen. Die offizielle Anthropic-Doku empfiehlt explizit: "Be pushy — cast a wide net of trigger phrases."

### 5. Kein YAML-Frontmatter à la SKILL.md
**Begründung (SKILL.md-Konventionen-Research):** INDEX.md-Dateien sind keine Skills. `name`/`description` im Frontmatter wird nur von SKILL.md-Dateien gelesen. Katalog-Dateien brauchen ein eigenes, schlankes Schema.

### 6. Kein INSTALLED.md
**Begründung (Simplicity Review):** `skills-lock.json` ist die Source of Truth für installierte Skills. Eine separate Markdown-Datei driftet sofort out of sync. Bei Bedarf: on-demand aus der Lock-Datei generieren.

## Zielstruktur

```
catalog/
  catalog.json           # Primary: Flache Liste aller 173 Skills (agent-readable)
  README.md              # Human-readable View mit TOC + Kategorien (generated from JSON)
```

**Nur 2 Dateien statt 14.** Die JSON-Datei ist die Single Source of Truth.

## Datei-Formate

### catalog.json (Primary — Agent-readable)

```json
{
  "version": 1,
  "generated": "2026-03-25",
  "source": "Claude Skills Collection.md",
  "total_skills": 173,
  "categories": [
    "document-skills",
    "creative-and-design",
    "development-and-code-tools",
    "data-and-analysis",
    "scientific-and-research-tools",
    "writing-and-research",
    "learning-and-knowledge",
    "media-and-content",
    "collaboration-and-project-management",
    "marketing-and-seo",
    "career-and-job-search",
    "security-and-testing",
    "utility-and-automation"
  ],
  "skills": [
    {
      "name": "terraform-code-generation",
      "description": "Generates and validates Terraform HCL configuration files for cloud infrastructure provisioning.",
      "category": "development-and-code-tools",
      "tags": ["terraform", "hcl", "iac", "infrastructure", "hashicorp", "devops", "cloud"],
      "when_to_use": "User asks to write, generate, or validate Terraform configuration. User mentions infrastructure as code, HCL, or cloud provisioning.",
      "source_url": "https://github.com/hashicorp/agent-skills/tree/main/terraform/code-generation",
      "source_repo": "hashicorp/agent-skills"
    },
    {
      "name": "copywriting",
      "description": "Expert conversion copywriter skill that prioritizes clarity and business outcomes with proven frameworks.",
      "category": "marketing-and-seo",
      "tags": ["copywriting", "conversion", "frameworks", "marketing", "sales-copy"],
      "when_to_use": "User asks to write marketing copy, landing page text, ad copy, or sales emails. User mentions conversion optimization or copywriting frameworks like PAS, AIDA.",
      "source_url": "https://github.com/coreyhaines31/marketingskills/tree/main/skills/copywriting",
      "source_repo": "coreyhaines31/marketingskills"
    }
  ]
}
```

**Felder pro Skill:**
| Feld | Pflicht | Beschreibung |
|------|---------|--------------|
| `name` | ja | Kebab-case Identifier (wie SKILL.md-Konvention) |
| `description` | ja | Was der Skill tut (3. Person, spezifisch, max ~200 Zeichen) |
| `category` | ja | Primäre Kategorie (1 von 13) |
| `tags` | ja | Array von Keywords für Suche (Synonyme, verwandte Begriffe) |
| `when_to_use` | ja | Trigger-Patterns: Wann ein Agent diesen Skill empfehlen soll |
| `source_url` | ja | Direkt-Link zum Skill-Repository — **Join-Key** zu `skills-lock.json` |
| `source_repo` | ja | GitHub owner/repo Kurzform (normalisiert: `owner/repo`) |

**Cross-Reference-Architektur (aus Architecture Review):**
Die `source_url` / `source_repo` sind der **kanonische Join-Key** zwischen drei Systemen:
- `catalog.json` → `source_repo: "remotion-dev/skills"`
- `skills-lock.json` → `"source": "remotion-dev/skills"`
- `.agents/skills/remotion-best-practices/` → installierter Skill

Damit lässt sich programmatisch ermitteln: Welche Katalog-Skills sind bereits installiert?
```bash
jq -r '.skills | keys[]' skills-lock.json  # installierte source repos
jq '.skills[] | select(.source_repo == "remotion-dev/skills")' catalog/catalog.json  # match
```

### README.md (Human-readable View — generiert aus JSON)

```markdown
# Skills Catalog

> 173 external Claude Code skills across 13 categories.
> Installed skills: `.agents/skills/` | This catalog: discovery index only.

## Categories
- [Document Skills](#document-skills) (5)
- [Creative & Design](#creative-and-design) (8)
- [Development & Code Tools](#development-and-code-tools) (50)
...

---

## Document Skills

| Name | Description | Source |
|------|-------------|--------|
| pdf | Extracts text and tables from PDFs, fills forms, merges documents | [anthropics/skills](https://github.com/...) |
| docx | Creates and edits Word documents | [anthropics/skills](https://github.com/...) |
...

## Creative & Design
...
```

**Kein Frontmatter, keine Tags-Spalte, keine Keywords.** Nur Name, Description, Source-Link. Clean und scanbar.

## Schritte

### Schritt 1: `catalog/` Verzeichnis anlegen
- `mkdir -p catalog/`

### Schritt 2: `catalog.json` erstellen
- Parse `Claude Skills Collection.md` (alle 13 Kategorie-Tabellen)
- Für jeden der 173 Skills:
  - `name`: Aus Original-Name ableiten (kebab-case)
  - `description`: Aus Original übernehmen, ggf. in 3. Person umformulieren
  - `category`: Aus Abschnitt ableiten
  - `tags`: Aus Name, Description und Source-URL extrahieren + manuelle Synonyme
  - `when_to_use`: NEU erstellen — Trigger-Patterns basierend auf Description
  - `source_url` / `source_repo`: Aus Original-Link extrahieren

### Schritt 3: `catalog/README.md` generieren
- Aus `catalog.json` generieren (Script oder manuell)
- TOC mit Anchor-Links pro Kategorie
- Einfache 3-Spalten-Tabellen (Name, Description, Source)

### Schritt 4: CLAUDE.md aktualisieren
```markdown
## Skills Catalog

Katalog von 173 externen Claude Code Skills in `catalog/catalog.json`.
Bei Skill-Discovery oder -Empfehlungen: `catalog.json` lesen und
User-Intent gegen `when_to_use`, `tags` und `description` matchen.
Installierte (aktive) Skills: `.agents/skills/`
```

### Schritt 5: Originaldatei archivieren
- `Claude Skills Collection.md` → `catalog/LEGACY-collection.md` (oder löschen)

## Kritische Dateien

- `Claude Skills Collection.md` — Quelldatei mit allen 173 Skills (zu parsen)
- `skills-lock.json` — Cross-Reference: welche Katalog-Skills sind bereits installiert
- `CLAUDE.md` — Muss um Katalog-Referenz ergänzt werden
- `.agents/skills/*/SKILL.md` — Referenz für SKILL.md-Konventionen (für `when_to_use`-Patterns)

## Verifikation

1. `jq '.skills | length' catalog/catalog.json` → 173
2. `jq '.skills[] | select(.tags[] | contains("terraform"))' catalog/catalog.json` → findet Terraform-Skills
3. `jq '.skills[] | select(.category == "marketing-and-seo")' catalog/catalog.json` → 19 Einträge
4. `jq '.skills[] | select(.when_to_use | test("deploy"))' catalog/catalog.json` → findet Deploy-Skills
5. Alle `source_url`-Felder enthalten gültige GitHub-URLs
6. Jeder Skill hat `when_to_use` (nicht leer)
7. `README.md` Skill-Count stimmt mit `catalog.json` überein
8. Keine Skills aus dem Original verloren oder doppelt

## Kategorie-Naming (aus Pattern-Recognition-Review)

Die originalen Kategorienamen sind lang und grammatisch inkonsistent. Da Kategorien im JSON nur String-Werte sind (keine Ordnernamen), können sie kürzer sein:

| Original (aus Collection.md) | Normalisiert (für `category`-Feld) |
|------|------------|
| Document Skills | `documents` |
| Creative & Design | `design` |
| Development & Code Tools | `development` |
| Data & Analysis | `data-analysis` |
| Scientific & Research Tools | `scientific-research` |
| Writing & Research | `writing` |
| Learning & Knowledge | `learning` |
| Media & Content | `media` |
| Collaboration & Project Management | `collaboration` |
| Marketing & SEO | `marketing` |
| Career & Job Search | `career` |
| Security & Testing | `security-testing` |
| Utility & Automation | `automation` |

## Data Quality Issues (aus Pattern-Recognition-Review)

Bei der Konvertierung der 173 Skills beachten:

1. **Naming-Inkonsistenz:** Skill-Namen mischen kebab-case (`terraform-code-generation`), Title Case (`Image Enhancer`), PascalCase (`ResumeSkills`). → **Alles auf kebab-case normalisieren.**
2. **Near-Duplicates:** 6 Resume-Skills (`resume-tailoring-skill`, `claude-code-job-tailor`, `claude-resume-kit`, `Resume-Builder`, `ResumeSkills`, `proficiently-claude-skills`) sind funktionale Duplikate. → **Im JSON als Cluster markieren oder deduplizieren.**
3. **Cross-Category Ambiguität:** ~23 Skills könnten in mehreren Kategorien sein (z.B. `systematic-debugging` → Security oder Development). → **Gelöst durch Tags statt feste Kategorien — ein Skill kann mehrere Tags haben.**
4. **Duplicate Eintrag:** `using-superpowers` erscheint in 2 Kategorien mit unterschiedlichen Descriptions. → **Eine primäre Kategorie, eine Description.**
5. **Kontrolliertes Tag-Vokabular:** Ohne definierte Tags explodiert die Taxonomie (z.B. `testing` / `tests` / `test` / `qa`). → **~30 kanonische Tags definieren:**

**Funktions-Tags (1-2 pro Skill):** `coding`, `testing`, `deployment`, `debugging`, `documentation`, `automation`, `design`, `writing`, `research`, `security`, `data-analysis`, `media`, `career`, `collaboration`, `learning`

**Technologie-Tags (0-3 pro Skill):** `python`, `typescript`, `react`, `nextjs`, `aws`, `terraform`, `docker`, `pdf`, `excel`, `pptx`, `git`

**Workflow-Tags (0-2 pro Skill):** `code-review`, `ci-cd`, `pr-workflow`, `file-processing`, `seo`, `email`, `resume`

## Rejected Alternatives (mit Begründung)

| Alternative | Warum verworfen |
|------------|-----------------|
| 14 Markdown-Dateien (13 Kategorien + Master) | Over-Engineering: 3-5 File-Reads pro Agent-Suche statt 1. Markdown-Tabellen sind parsing-feindlich. |
| YAML-Frontmatter à la SKILL.md | INDEX.md ≠ SKILL.md. `name`/`description` im Frontmatter wird nur von SKILL.md gelesen. Falsche Signale. |
| Tags im Frontmatter | Claude Code Runtime hat keine Tag-basierte Suche. Tags im JSON-Array sind maschinenlesbar. |
| Separate INSTALLED.md | YAGNI. `skills-lock.json` ist Source of Truth. Zweite Datei driftet sofort. |
| 173 Einzeldateien pro Skill | 95% Filesystem-Overhead für 5% Content. Katalog-Einträge sind Referenzkarten, keine ausführbaren Skills. |

## References

- [Anthropic SKILL.md Best Practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [Anthropic Skills Overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [Claude Code Skills Architecture](https://mikhail.io/2025/10/claude-code-skills/)
- [Claude Agent Skills Deep Dive](https://leehanchung.github.io/blogs/2025/10/26/claude-skills-deep-dive/)
