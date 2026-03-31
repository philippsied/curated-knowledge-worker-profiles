# Design-Psychologie: Farben, Typografie, Layout

> Entscheidungsgrundlage für Farbpaletten, Schriftarten und Layout-Strategien basierend auf Marketingpsychologie und Branchenstandards.

## Inhaltsverzeichnis

1. [Farbpsychologie nach Branche](#farbpsychologie-nach-branche)
2. [Farbpaletten-Erstellung](#farbpaletten-erstellung)
3. [Typografie-Psychologie](#typografie-psychologie)
4. [Layout-Prinzipien](#layout-prinzipien)
5. [Branchenspezifische Design-Patterns](#branchenspezifische-design-patterns)
6. [Barrierefreiheit & Kontrast](#barrierefreiheit)

---

## Farbpsychologie nach Branche

### Farb-Assoziationen und Einsatzbereiche

| Farbe | Psychologische Wirkung | Am besten für | Vermeiden bei |
|---|---|---|---|
| **Blau** (#2563EB) | Vertrauen, Stabilität, Professionalität | SaaS, Fintech, B2B, Healthcare | Essen, Kreativ-Agenturen |
| **Grün** (#16A34A) | Wachstum, Gesundheit, Nachhaltigkeit | Finanzen, Health, Umwelt, Education | Luxury, Entertainment |
| **Orange** (#EA580C) | Energie, Dringlichkeit, Freundlichkeit | CTAs, E-Commerce, Startups | Luxury, Trauer, Medizin |
| **Rot** (#DC2626) | Dringlichkeit, Leidenschaft, Energie | Sales, Food, Entertainment | Finanzen, Gesundheit, B2B |
| **Violett** (#7C3AED) | Kreativität, Luxus, Innovation | Premium SaaS, Kreativ-Tools | Traditional B2B, Finanzen |
| **Gelb** (#EAB308) | Optimismus, Klarheit, Aufmerksamkeit | Akzente, Highlights, Warnungen | Hintergründe (Lesbarkeit!) |
| **Schwarz** (#0F172A) | Eleganz, Kraft, Exklusivität | Luxury, Fashion, Premium | Kinder, Health, Community |
| **Weiß** (#FFFFFF) | Klarheit, Einfachheit, Raum | Minimalistisches SaaS, Medical | Dark-Mode-Präferenzen |
| **Türkis** (#0891B2) | Modernität, Frische, Tech | SaaS, Startups, Health-Tech | Traditional, Luxury |

### CTA-Farb-Regeln

1. **CTA-Farbe ≠ Primärfarbe**: Der CTA muss sich abheben (Complementärfarbe oder hoher Kontrast)
2. **Grün und Orange** sind die meistgetesteten CTA-Farben mit den besten Ergebnissen
3. **Rot** funktioniert für Urgency-CTAs ("Nur noch 3 Plätze"), aber nicht für Formulare
4. **Kontrastverhältnis**: Mindestens 4.5:1 gegen den Hintergrund (WCAG AA)

### Emotion-basierte Farbwahl

Frage: Welches Gefühl soll der Besucher nach 5 Sekunden auf der Seite haben?

| Gewünschtes Gefühl | Primärfarbe | Akzentfarbe |
|---|---|---|
| "Das ist vertrauenswürdig" | Blau oder Dunkelblau | Grün oder Orange CTA |
| "Das ist modern/innovativ" | Dunkel (near-black) + Türkis | Leuchtgrün oder Violett CTA |
| "Das ist warm/persönlich" | Warmes Weiß + Terrakotta | Orange oder warmes Rot CTA |
| "Das ist professionell/enterprise" | Navy + Grau | Blau oder Grün CTA |
| "Das ist energisch/startup" | Weiß + lebhaftes Primär | Kontrastierender CTA |
| "Das ist luxuriös/premium" | Schwarz + Gold/Champagner | Gold oder Weiß CTA |

---

## Farbpaletten-Erstellung

### 60-30-10-Regel

| Anteil | Rolle | Beispiel |
|---|---|---|
| 60% | Hintergrund | Weiß, Hellgrau, Dunkelblau |
| 30% | Sekundär | Karten, Sektionen, Akzentbereiche |
| 10% | Akzent/CTA | CTA-Buttons, Links, Highlights |

### Pflicht-Farben für jedes Konzept

Definiere immer diese CSS-Custom-Properties:

```css
:root {
  --color-bg-primary:     /* 60% – Haupthintergrund */
  --color-bg-secondary:   /* Alternierender Sektions-Hintergrund */
  --color-bg-dark:        /* Dunkle Sektionen (Problem, Footer) */
  --color-text-primary:   /* Haupttext */
  --color-text-secondary: /* Sekundärtext, Labels */
  --color-text-on-dark:   /* Text auf dunklem Hintergrund */
  --color-accent:         /* Primäre Akzentfarbe */
  --color-cta:            /* CTA-Button-Hintergrund */
  --color-cta-text:       /* CTA-Button-Text */
  --color-cta-hover:      /* CTA-Button-Hover */
  --color-success:        /* Bestätigungen, Checkmarks */
  --color-border:         /* Trennlinien, Card-Borders */
}
```

---

## Typografie-Psychologie

### Schriftarten-Kategorien und ihre Wirkung

| Kategorie | Wirkung | Einsatz | Google-Fonts-Beispiele |
|---|---|---|---|
| **Geometric Sans** | Modern, clean, tech | SaaS, Startups | Plus Jakarta Sans, DM Sans, Outfit |
| **Humanist Sans** | Freundlich, zugänglich | Education, Health, Community | Source Sans 3, Nunito, Open Sans |
| **Neo-Grotesque** | Neutral, professionell | B2B, Enterprise | Switzer, General Sans (nicht Inter!) |
| **Serif** | Vertrauenswürdig, editorial | Finance, Legal, Premium | Playfair Display, Lora, Merriweather |
| **Slab Serif** | Stark, selbstbewusst | Bold Brands, Statements | Roboto Slab, Zilla Slab |
| **Display/Decorative** | Einzigartig, emotional | Headlines für Kreativ-Brands | Space Grotesk, Syne, Clash Display |
| **Handwritten** | Persönlich, authentisch | Coaching, Kreative | Caveat, Dancing Script (nur Akzente!) |

### Font-Pairing-Regeln

1. **Maximal 2 Schriftarten** (eine für Headlines, eine für Body)
2. **Kontrast erzeugen**: Serif-Headline + Sans-Body oder umgekehrt
3. **Nie zwei ähnliche Fonts paaren** (z.B. zwei Geometric Sans)
4. **CTA-Buttons**: Gleiche Font wie Body, aber bold/uppercase

### Bewährte Pairings nach Branche

| Branche | Headline | Body | Wirkung |
|---|---|---|---|
| SaaS/Tech | Plus Jakarta Sans (700) | Plus Jakarta Sans (400) | Clean, modern |
| Premium SaaS | Playfair Display (700) | DM Sans (400) | Sophisticated |
| Coaching | Syne (700) | Source Sans 3 (400) | Persönlich, modern |
| Fintech | General Sans (700) | General Sans (400) | Professionell |
| Education | Nunito (800) | Nunito (400) | Freundlich, zugänglich |
| E-Commerce | Outfit (700) | Outfit (400) | Energisch, klar |
| Creative | Clash Display (600) | DM Sans (400) | Bold, einzigartig |

### Typografie-Größen (Mobile-First)

```css
/* Mobile Basis */
--font-size-hero:    clamp(2rem, 5vw + 1rem, 3.5rem);
--font-size-h2:      clamp(1.5rem, 3vw + 0.5rem, 2.5rem);
--font-size-h3:      clamp(1.25rem, 2vw + 0.5rem, 1.75rem);
--font-size-body:    clamp(1rem, 1vw + 0.5rem, 1.125rem);
--font-size-small:   clamp(0.875rem, 1vw, 1rem);
--font-size-cta:     clamp(1rem, 1.5vw, 1.125rem);

/* Line Heights */
--lh-heading:  1.2;
--lh-body:     1.6;

/* Letter Spacing */
--ls-heading:  -0.02em;
--ls-body:     0;
--ls-cta:      0.02em; /* leicht gesperrt für CTAs */
```

---

## Layout-Prinzipien

### Visueller Rhythmus (Sektions-Abfolge)

Alterniere zwischen Layouts für visuellen Rhythmus:
1. Hell → Dunkel → Hell → Dunkel (Sektions-Hintergründe)
2. Text-links/Bild-rechts → Bild-links/Text-rechts
3. Volle Breite → Constrained → Volle Breite

### Whitespace-Regeln

| Element | Minimum Spacing |
|---|---|
| Zwischen Sektionen | 80–120px (Desktop), 60–80px (Mobile) |
| Headline → Subheadline | 16–24px |
| Subheadline → Body | 24–32px |
| Body → CTA | 32–48px |
| Card-Padding | 24–32px |
| Absatz-zu-Absatz | 16–24px |

### Container-Breiten

```css
--container-max:     1200px;  /* Hauptcontainer */
--container-narrow:  720px;   /* Textlastige Sektionen */
--container-wide:    1400px;  /* Full-bleed Sektionen */
```

### Z-Pattern und F-Pattern

- **Hero**: Z-Pattern (Logo oben links → CTA oben rechts → Visual unten links → Text unten rechts)
- **Body-Sektionen**: F-Pattern (Headline scannen → linke Seite scannen → selektiv lesen)
- **CTA-Bereiche**: Mittig zentriert für maximale Aufmerksamkeit

### Richtungscues

Nutze visuelle Elemente, die den Blick zum CTA lenken:
- Pfeil-Icons oder Linien, die auf den CTA zeigen
- Personen auf Fotos, die zum CTA schauen
- Farbverlauf, der zum CTA-Bereich hin stärker wird
- Weißraum-Kanalisierung: CTA in der einzigen "leeren" Zone

---

## Branchenspezifische Design-Patterns

### SaaS / Software

- **Dominanter Pattern**: Minimalistisch, viel Whitespace, Produktscreenshots
- **Hero**: Split-Layout mit Headline links, App-Screenshot rechts
- **Farben**: Blau/Türkis-Basis, dunkler Hintergrund-Trend
- **Typografie**: Geometric Sans (clean, modern)
- **Visuals**: App-Screenshots in Device-Mockups, Feature-Icons
- **CTA**: "Kostenlos testen" / "Demo buchen"

### Online-Kurse / Education

- **Dominanter Pattern**: Persönlich, Instructor im Vordergrund, Social Proof schwer
- **Hero**: Instructor-Foto oder Video, starke Benefit-Headline
- **Farben**: Warm, zugänglich (Grün, Orange-Akzente)
- **Typografie**: Humanist Sans (freundlich, vertrauenswürdig)
- **Visuals**: Instructor-Fotos, Testimonial-Fotos, Curriculum-Preview
- **CTA**: "Jetzt einschreiben" / "Kurs starten"
- **Besonderheit**: Long-Form Pages, ausführliche FAQ, Geld-zurück-Garantie prominent

### Coaching / Beratung

- **Dominanter Pattern**: Persönlich, Story-driven, Transformation
- **Hero**: Coach-Foto mit Headline, die Transformation verspricht
- **Farben**: Warm, vertrauenswürdig (Dunkelblau + Terrakotta, Navy + Gold)
- **Typografie**: Serif-Headline + Sans-Body für Autorität + Zugänglichkeit
- **Visuals**: Authentische Fotos, Vorher-Nachher, Video-Testimonials
- **CTA**: "Kostenloses Erstgespräch buchen" / "Jetzt bewerben"

### Agentur / Dienstleistung

- **Dominanter Pattern**: Portfolio-orientiert, Ergebnisse zeigen
- **Hero**: Case-Study-Highlight oder Bold Statement
- **Farben**: Schwarz/Weiß-Basis mit einem starken Akzent
- **Typografie**: Bold Display-Font für Headline
- **Visuals**: Portfolio-Beispiele, Ergebnis-Metriken
- **CTA**: "Projekt anfragen" / "Kostenloses Audit"

### E-Commerce / Digitale Produkte

- **Dominanter Pattern**: Produktzentriert, Social Proof schwer
- **Hero**: Produkt im Einsatz, nicht isoliert
- **Farben**: Markenfarben dominant, CTA in Komplementärfarbe
- **Typografie**: Klar und groß, wenig Text
- **Visuals**: Produktbilder, Unboxing, User-Generated Content
- **CTA**: "Jetzt kaufen" / "In den Warenkorb"

---

## Barrierefreiheit

### Kontrastverhältnisse (WCAG 2.1 AA)

| Element | Mindest-Kontrast |
|---|---|
| Normaler Text | 4.5:1 |
| Großer Text (≥18pt oder ≥14pt bold) | 3:1 |
| UI-Elemente (Buttons, Inputs) | 3:1 gegen Hintergrund |
| Placeholder-Text | 4.5:1 (wird oft vergessen!) |

### Farbblindheits-Sicherheit

- Nie NUR Farbe als Informationsträger nutzen (z.B. Rot/Grün für Fehler/Erfolg)
- Zusätzlich Icons, Text oder Muster verwenden
- Tools: Chrome DevTools → Rendering → Emulate Vision Deficiencies

### Fokus-Styles

```css
*:focus-visible {
  outline: 3px solid var(--color-accent);
  outline-offset: 2px;
  border-radius: 4px;
}
```

### Schriftgrößen

- Minimum Body: 16px (nie kleiner, auch nicht für Microcopy auf Desktop)
- Minimum CTA: 16px
- Minimum Footer-Links: 14px
- Line-Height Body: mindestens 1.5 (besser 1.6)
