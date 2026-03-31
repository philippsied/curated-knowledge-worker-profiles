---
name: landingpage-builder
description: "Erstellt datengestützte, high-converting Landingpage-Konzepte auf Basis eines Lean Canvas und setzt sie als HTML/CSS/JS um. Nutzt den 10-Sektionen-Blueprint, psychologische Frameworks (AIDA, PAS, StoryBrand, Cialdini) und CRO-Best-Practices. Beinhaltet Struktur, Copy-Texte, Layout, Schrift- und Farbwahl, SEO, Geo-Targeting, rechtliche Pflichtseiten (Impressum, Datenschutz, AGB), Kontaktformular und Newsletter-Integration. Wird ausgelöst bei: 'Landingpage erstellen', 'Landing Page bauen', 'Landingpage-Konzept', 'Landingpage aus Lean Canvas', 'conversion-optimierte Seite', 'Sales Page erstellen', 'Landingpage für SaaS/Kurs/Service', 'LP builder', 'Landingpage Entwurf'. Auch auslösen wenn ein Lean Canvas vorhanden ist und der Nutzer eine Landingpage, Website oder Sales Page daraus ableiten möchte."
---

# Landingpage Builder

Erstellt ein vollständiges, conversion-optimiertes Landingpage-Konzept auf Basis eines Lean Canvas und setzt es als HTML/CSS/JS-Prototyp um. Jede Entscheidung – von der Sektionsreihenfolge über Copy-Texte bis zur Farbwahl – ist psychologisch und datengestützt begründet.

## Wann Referenz-Dateien lesen

Lies die Referenzen in dieser Reihenfolge, passend zur aktuellen Phase:

| Phase | Referenz-Datei | Wann lesen |
|---|---|---|
| Phase 1–2 | `references/blueprint-10-sections.md` | **Immer** vor dem Konzept lesen. Enthält den vollständigen 10-Sektionen-Blueprint, Copywriting-Formeln, CTA-Regeln und Sektionsbeschreibungen. |
| Phase 2 | `references/design-psychology.md` | **Immer** vor Design-Entscheidungen lesen. Enthält Farb-/Schrift-/Layout-Psychologie und branchenspezifische Design-Patterns. |
| Phase 4 | `references/checklist.md` | **Immer** vor der finalen Prüfung lesen. Enthält die vollständige QA-Checkliste. |
| Phase 2–3 | `references/legal-templates-dach.md` | Bei Zielmarkt DACH lesen. Enthält Templates für Impressum, Datenschutz, AGB. |

## Gesamtablauf

```
Phase 0: Lean Canvas einlesen → Kundensegment wählen
    ↓
Phase 1: Design-Recherche → Branchenübliche Patterns identifizieren
    ↓
Phase 2: Konzept erstellen → Markdown-Dokument mit allen Sektionen
    ↓  ← Benutzer-Freigabe erforderlich
Phase 3: HTML/CSS/JS-Prototyp bauen
    ↓
Phase 4: Sektions-Review → Feedback pro Bereich einholen
    ↓
Phase 5: Checkliste durchgehen → Lücken identifizieren
```

---

## Phase 0: Lean Canvas einlesen und Kundensegment wählen

### Eingabe-Quellen (in Prioritätsreihenfolge)

1. Lean Canvas im Chat (als Text, JSON oder Markdown)
2. Lean Canvas in den Projekt-Dateien (mit `project_knowledge_search` suchen)
3. Lean Canvas im Gespräch erarbeiten (Minimal: Problem, Kundensegment, UVP, Lösung)

### Minimale Pflichtfelder aus dem Lean Canvas

Ohne diese Felder kann kein sinnvolles Konzept erstellt werden:

| Lean-Canvas-Feld | Warum zwingend für Landingpage |
|---|---|
| **Problem** | → Sektion 3 (Problem-Sektion), PAS-Framework |
| **Kundensegmente** | → Tonalität, Ansprache, Design-Stil, SEO-Keywords |
| **Wertversprechen (UVP)** | → Hero-Headline, Subheadline |
| **Lösung** | → Sektion 4 (So funktioniert's), Features |
| **Kanäle** | → SEO-Strategie, Traffic-Quellen, Message Match |
| **Einnahmequellen** | → Sektion 7 (Pricing/Angebot) |

Falls Felder fehlen, den Nutzer fragen – aber mit konkretem Vorschlag basierend auf den vorhandenen Daten.

### Kundensegment-Auswahl

Wenn mehrere Kundensegmente vorhanden sind, mit dem Benutzer abstimmen:
- Für welches Segment soll die Landingpage erstellt werden?
- Empfehlung geben: Das Segment mit dem stärksten Problem-Lösungs-Fit priorisieren.
- Pro Segment wird eine separate Landingpage empfohlen (1 Seite = 1 Zielgruppe = 1 CTA).

### Zielmarkt und Sprache klären

Folgende Entscheidungen mit dem Benutzer treffen:

1. **Zielmarkt** (z.B. Deutschland, DACH, International) → bestimmt rechtliche Anforderungen
2. **Sprache** der Landingpage (Deutsch, Englisch, mehrsprachig)
3. **Primäres Conversion-Ziel** (Lead-Generierung, Free Trial, Direktkauf, Warteliste)
4. **Traffic-Quellen** (Google Ads, Social, Organic, E-Mail) → bestimmt Message Match

---

## Phase 1: Design-Recherche

Vor dem Konzept recherchieren, welche Design-Patterns in der Branche des Produkts üblich und erfolgreich sind. Nutze Web Search für aktuelle Beispiele.

### Recherche-Fragen

1. **Branchenstandard**: Welche Landingpage-Designs sind in der Branche üblich? (z.B. SaaS: minimalistisch mit Produktscreenshot; Coaching: persönlich mit Video; E-Commerce: produktzentriert)
2. **Wettbewerber-Analyse**: Wie sehen die Top-3-Wettbewerber-Landingpages aus? Welche Muster wiederholen sich?
3. **Farbpsychologie**: Welche Farben passen zum Kundensegment und zur Branche?
4. **Typografie**: Welche Schriftarten kommunizieren die richtige Tonalität?

### Design-Vorschlag an den Benutzer

Präsentiere dem Benutzer 2–3 Design-Richtungen als Vergleichstabelle:

```
| Aspekt | Option A: [Name] | Option B: [Name] | Option C: [Name] |
|--------|-------------------|-------------------|-------------------|
| Stil   | z.B. Minimalist   | z.B. Bold/Modern  | z.B. Warm/Persönl.|
| Farben | ...               | ...               | ...               |
| Fonts  | ...               | ...               | ...               |
| Passt zu| ...              | ...               | ...               |
```

Lass den Benutzer wählen oder eine Kombination zusammenstellen. Lies `references/design-psychology.md` für psychologisch fundierte Empfehlungen.

---

## Phase 2: Konzept erstellen (Markdown-Dokument)

Lies zuerst `references/blueprint-10-sections.md` vollständig.

### Psychologisches Framework wählen

Basierend auf dem Lean Canvas und Kundensegment das passende Framework empfehlen:

| Situation | Empfohlenes Framework |
|---|---|
| Besucher kennt das Problem nicht | AIDA |
| Besucher hat aktiven Schmerz | PAS |
| Emotional aufgeladenes Thema (Coaching, Gesundheit) | PASTOR oder BAB |
| B2B/SaaS mit rationalem Entscheider | StoryBrand + AIDA |
| High-Ticket (>500€) | PASTOR + Long-Form |

### Markdown-Dokument-Struktur

Das Konzept-Dokument muss diese Abschnitte enthalten:

```markdown
# Landingpage-Konzept: [Produktname]
## Meta-Informationen
- Zielmarkt, Sprache, Kundensegment, Conversion-Ziel, Framework

## Design-Entscheidungen
- Farbpalette (Primär, Sekundär, Akzent, Hintergrund, Text) mit Hex-Codes und psychologischer Begründung
- Typografie (Headline-Font, Body-Font, CTA-Font) mit Begründung
- Layout-Stil und Whitespace-Strategie
- Bildsprache und Visual Direction

## SEO-Strategie
- Primär-Keyword, Sekundär-Keywords, Long-Tail-Keywords
- Meta-Title, Meta-Description
- URL-Struktur
- Schema Markup (FAQ, Product, Organization)
- Open Graph Tags

## Geo-Targeting
- Lokale SEO-Signale
- Sprach-Tags (hreflang)
- Zielmarkt-spezifische Anpassungen (Währung, Telefon, Adresse)

## Sektionen 1–10 (jeweils)
Für jede Sektion:
- Zweck und psychologisches Prinzip
- Headline (exakter Text)
- Subheadline (exakter Text)
- Body Copy (exakter Text)
- CTA-Text und -Farbe (wo zutreffend)
- Layout-Beschreibung (Spalten, Anordnung, Abstände)
- Visual-Platzhalter mit Beschreibung und Erstellungsvorschlag
  (z.B. "Hero-Image: Produktscreenshot → eigener Screenshot"
   oder "Icon-Set: 3 abstrakte Icons → als SVG generieren")

## Pflichtkomponenten
- Kontaktformular (Felder, Platzierung, Validierung)
- Newsletter-Integration (Platzierung, Incentive, Double Opt-In)
- Impressum (Volltext oder Verweis auf Unterseite)
- Datenschutzerklärung (Volltext oder Verweis)
- AGB (Volltext oder Verweis)
- Cookie-Consent-Banner

## Checkliste
→ Wird in Phase 5 ausgefüllt
```

### Copywriting-Regeln (zwingend befolgen)

1. **Headline**: Nutzenorientiert, 10–15 Wörter, beantwortet "Was bringt mir das?"
2. **Subheadline**: Ergänzt (wiederholt nicht!), liefert das "Wie" oder "Für wen"
3. **CTA-Texte**: Erste Person, Aktionsverb, 2–5 Wörter, konkreter Nutzen
4. **Body Copy**: Du/Sie-Ansprache, Benefits vor Features, Fünftklässler-Leseniveau
5. **Microcopy**: Unter jedem CTA-Button angstreduzierend ("Keine Kreditkarte nötig")
6. **Wortanzahl**: 250–725 Wörter Gesamttext (ohne rechtliche Seiten)

### Visual-Platzhalter-Format

Für jedes Bild/Visual einen standardisierten Platzhalter erstellen:

```
[VISUAL: Hero-Image]
Beschreibung: Produktscreenshot mit Dashboard-Ansicht, Person im Hintergrund
Maße: 1200x800px, 16:9
Erstellungsvorschlag:
  - Option A: Eigener Screenshot des Produkts (empfohlen für SaaS)
  - Option B: Gemini/DALL-E Prompt: "Clean SaaS dashboard mockup, ..."
  - Option C: Stockphoto von Unsplash (Suchbegriff: "...")
  - Option D: Als SVG-Illustration nachträglich generieren
Fallback: Einfarbiger Gradient mit Headline-Text
```

---

## Phase 3: HTML/CSS/JS-Prototyp

Erst nach expliziter Benutzer-Freigabe des Markdown-Konzepts starten.

### Technische Anforderungen

- **Rein HTML, CSS, JavaScript** – keine Frameworks, keine Build-Tools
- **Responsive Design**: Mobile-First (82.9% Mobile-Traffic)
- **Performance**: Kein externes CSS/JS außer Google Fonts, Bilder als Platzhalter
- **Accessibility**: ARIA-Labels, Kontrastverhältnis ≥4.5:1, Fokus-Styles
- **Single-File oder Multi-File**: Je nach Komplexität entscheiden

### Pflicht-Implementierungen

1. **Kontaktformular**: HTML-Formular mit Client-seitiger Validierung, action-Attribut als Platzhalter (z.B. `action="#contact-submit"`), maximal 3–5 Felder
2. **Newsletter-Formular**: E-Mail-Feld + CTA-Button, Platzhalter für Provider-Integration (Mailchimp, ConvertKit etc.), Double-Opt-In-Hinweis
3. **Cookie-Consent-Banner**: DSGVO-konform, Akzeptieren/Ablehnen/Einstellungen
4. **Impressum/Datenschutz/AGB**: Als Overlay-Modals oder separate Sektionen am Footer, mit Platzhalter-Texten aus `references/legal-templates-dach.md`
5. **Scroll-Verhalten**: Smooth Scrolling zu Ankern
6. **CTA-Buttons**: Alle verlinkt zu einem einzigen Conversion-Ziel

### Visual-Platzhalter im HTML

```html
<!-- Platzhalter-Pattern für Bilder -->
<div class="visual-placeholder" style="aspect-ratio: 16/9; background: var(--gradient-placeholder);">
  <span class="placeholder-label">Hero-Image: [Beschreibung]</span>
  <span class="placeholder-suggestion">→ Eigener Screenshot empfohlen</span>
</div>
```

### Code-Qualität

- CSS Custom Properties für alle Design-Tokens (Farben, Fonts, Spacing)
- Semantisches HTML (header, main, section, footer, nav)
- Kommentare an jeder Sektion mit Verweis auf das psychologische Prinzip
- FAQ als Akkordeon mit reinem CSS/JS
- Testimonials mit Platzhalter-Daten

Lies `references/blueprint-10-sections.md` für die exakte Sektionsreihenfolge und das frontend-design Skill für ästhetische Qualität.

---

## Phase 4: Sektions-Review mit dem Benutzer

Nach Erstellung des HTML-Prototyps systematisch durch jede Sektion gehen und gezieltes Feedback einholen.

### Review-Reihenfolge

Gehe diese Bereiche **einzeln und nacheinander** mit dem Benutzer durch:

1. **Gesamtstruktur**: Stimmt die Reihenfolge der 10 Sektionen? Fehlt etwas?
2. **Hero-Bereich**: Headline, Subheadline, CTA – trifft es den Kern?
3. **Problem-Sektion**: Beschreibt sie den echten Schmerz des Kundensegments?
4. **Lösung & Features**: Sind die richtigen Benefits herausgestellt?
5. **Social Proof**: Welche echten Testimonials/Logos können eingefügt werden?
6. **Pricing/Angebot**: Stimmt die Darstellung des Angebots?
7. **Copy-Texte insgesamt**: Tonalität, Du/Sie, Fachsprache-Level
8. **CTA-Texte und Platzierung**: Motivierend genug? Richtige Position?
9. **Design**: Farben, Fonts, Layout – passt es zur Marke?
10. **Visuals**: Welche Bilder kann der Benutzer liefern? Was muss generiert werden?
11. **Rechtliches**: Impressum-Daten, Datenschutz-Details, AGB-Anpassungen
12. **SEO**: Keywords korrekt? Meta-Daten passend?

Für jeden Bereich: Konkretes Feedback erfragen, Änderungen sofort umsetzen, dann zum nächsten Bereich.

---

## Phase 5: Finale Checkliste

Lies `references/checklist.md` und gehe die vollständige Checkliste mit dem Benutzer durch.

Die Checkliste hat drei Spalten:
- ✅ **Vorhanden & optimiert**
- ⚠️ **Vorhanden, aber verbesserbar**
- ❌ **Fehlt noch**

Am Ende: Zusammenfassung der offenen Punkte als priorisierte To-Do-Liste.

---

## Kernregeln

1. **1 Landingpage = 1 Kundensegment = 1 Conversion-Ziel.** Niemals mehrere Zielgruppen oder CTAs auf einer Seite mischen.
2. **Attention Ratio 1:1.** Keine Navigation auf der Landingpage. Jeder Link ist ein Conversion-Leck. Einzige Ausnahme: rechtlich verpflichtende Links (Impressum etc.).
3. **Message Match.** Die Headline muss die Erwartung spiegeln, die die Traffic-Quelle (Ad, E-Mail, Social Post) gesetzt hat.
4. **Lesegrad Klasse 5–7.** Einfache Sprache, kurze Sätze, konkrete Zahlen. Kein Fachjargon ohne Erklärung.
5. **Mobile First.** 82.9% des Traffics ist mobil. Touch-Targets ≥48px.
6. **Formularfelder minimieren.** Maximal 3–5 Felder. Jedes zusätzliche Feld kostet Conversions.
7. **Social Proof durchgängig.** Logo-Leiste nach Hero, Testimonials nach Lösung, Trust Badges neben CTA.
8. **Kein Stockphoto-Gefühl.** Authentische Bilder oder stylische Illustrationen/SVGs. Generische Stockphotos zerstören Vertrauen.
9. **DSGVO/Impressumspflicht.** Im DACH-Raum ist ein fehlendes Impressum nicht nur illegal, sondern ein Conversion-Killer.
10. **Immer A/B-Test-Empfehlungen geben.** Für mindestens Headline und CTA konkrete Varianten vorschlagen.
