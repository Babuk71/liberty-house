# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Progetto

**Liberty House Hospitality** — brand ombrello su più strutture ricettive a Messina. Attualmente: Casa Montante e Casa Scendente (case vacanze settimanali, 6 posti, vista Stretto, Contrada Fortino). Entro fine 2026 si aggiunge almeno un B&B.

Pubblicato su GitHub Pages per preview/testing: `https://babuk71.github.io/liberty-house/`
Dominio proprio (`libertyhousemessina.it` o simile) da acquistare al momento del go-live.

## Sviluppo

Nessun build step, nessun bundler. CSS e JS inline in ogni file HTML. Per sviluppare:

```bash
python -m http.server 8080
# oppure
npx serve .
```

## Pagine

| File | Scopo |
|---|---|
| `index.html` | Home — presentazione brand e case |
| `la-zona.html` | SEO informazionale — Messina, attrazioni, come arrivare |

**Prossime pagine pianificate:** `/casa-montante`, `/casa-scendente`, `/bb-[nome]`, `/prenota`, sezione news/blog stagionale.

## Struttura di `index.html`

1. **`<head>`** — meta SEO, Open Graph, Twitter Card, JSON-LD Schema (`LodgingBusiness` con due `Accommodation`)
2. **`<script id="tweak-defaults">`** — valori di default del pannello Tweaks
3. **`<style>`** — CSS organizzato per componente con commenti `/* ── NOME ── */`
4. **Body** — sezioni: `nav`, `hero`, `#intro`, `#spazi`, `#servizi`, `#galleria`, `#contatti`, `footer`, `#tweaks-panel`
5. **`<script>`** — i18n, nav scroll, reveal on scroll, form, tweaks, hamburger, email offuscata

## Design system

Variabili CSS in `:root`:

| Variabile | Valore | Uso |
|---|---|---|
| `--antracite` | `#2B2B2B` | Sfondo principale |
| `--oro` | `#C9A96E` | Accento / CTA |
| `--champagne` | `#E8DFC8` | Testo primario chiaro |
| `--grigio-caldo` | `#A29085` | Testo secondario (4.63:1 su antracite — WCAG AA) |
| `--navy-dark` | `#1A2A4A` | Sfondo sezione Servizi e gallery |

Font: Georgia serif esclusivamente. Stile luxury/minimal, letterspacing elevato, font-weight 300.

## Accessibilità (score attuale: ~100)

- Tutti i campi form hanno `for`/`id` espliciti (`nome`, `cognome`, `email`, `arrivo`, `partenza`, `ospiti`, `messaggio`)
- `--grigio-caldo` alzato a `#A29085` per WCAG AA su sfondo antracite
- `.servizio-desc` usa `#B0A898` per contrasto sufficiente su navy
- Footer testo/link a `rgba(154,143,126,0.9)`

## Internazionalizzazione (i18n)

Supporto IT/EN tramite oggetto `T` nel `<script>` finale. Tre attributi HTML:
- `data-i18n="chiave"` → `textContent`
- `data-i18n-html="chiave"` → `innerHTML`
- `data-i18n-placeholder="chiave"` → `placeholder`

Rilevamento automatico: `localStorage('lh-lang')` → lingua browser → fallback IT.

## Email offuscata

L'email `turric1@doworks.eu` non è nel DOM — viene ricostruita a runtime da tre frammenti JS e iniettata nell'elemento `#email-link`. Nel JSON-LD è presente in chiaro (accettabile per i motori di ricerca).

## Tweaks panel

Nascosto di default, si attiva via `postMessage({ type: '__activate_edit_mode' })`. Controlla sfondo hero, griglia spazi, colore accento.

## Immagini

Tutte le sezioni usano placeholder (`.img-placeholder`, `.card-img-ph`, `.gal-item`). Quando arrivano le foto reali, aggiungere:
- `loading="lazy"` su tutte tranne l'hero logo
- `fetchpriority="high"` sull'hero logo
- `srcset` con versioni ridimensionate per mobile
- `width` e `height` proporzionali alle dimensioni CSS

## Asset

| File | Uso |
|---|---|
| `liberty-house-logo.webp` | Logo standard (nav: 207×36, footer: 161×28) |
| `liberty-house-logo-oro.webp` | Logo dorato hero (280px wide, height auto) |
| `favicon.ico` / `favicon_32x32.png` / `favicon_64x64.png` | Favicon |
| `liberty_house_favicon_192.webp` / `_512.webp` | Apple touch icon / PWA |

## SEO — prossimi passi concordati

1. Acquisto dominio proprio + aggiornamento canonical, og:url, JSON-LD
2. Registrazione Google Business Profile
3. Google Search Console + sitemap.xml
4. CIN (Codice Identificativo Nazionale) da esporre su sito e annunci OTA
5. Sezione news/blog stagionale — cadenza ~mensile, articoli su eventi Messina
6. Pagine dedicate per ogni struttura (dipendono dalle foto reali)

## Stile editoriale (per articoli news)

Tono diretto, evocativo, senza filler. Riferimenti storici e geografici specifici. Niente aggettivi generici. Chiusura con CTA sulla disponibilità. Esempio di registro: *"La tradizione enogastronomica messinese trova il suo Re nel pesce spada"*.
