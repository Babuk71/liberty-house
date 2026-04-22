# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Progetto

Sito web statico per **Liberty House Hospitality** — due case vacanze sul mare a Messina (Casa Montante e Casa Scendente) nel complesso Le Verande, Contrada Fortino. Pubblicato su GitHub Pages: `https://babuk71.github.io/liberty-house/`

## Sviluppo

Nessun build step, nessun bundler. Il sito è un singolo file `index.html` con CSS e JS inline. Per sviluppare, basta aprire il file nel browser o avviare un server locale:

```bash
python -m http.server 8080
# oppure
npx serve .
```

## Struttura del file `index.html`

Il file è organizzato in blocchi ben delimitati da commenti `<!-- ═══ ... ═══ -->`:

1. **`<head>`** — meta SEO, Open Graph, Twitter Card, JSON-LD Schema (`LodgingBusiness` con due `Accommodation`)
2. **`<script id="tweak-defaults">`** — valori di default del pannello Tweaks (heroLayout, accentColor, navStyle, spaziCard)
3. **`<style>`** — tutto il CSS, organizzato per componente con commenti `/* ── NOME ── */`
4. **HTML body** — sezioni nell'ordine: `nav`, `hero`, `#intro`, `#spazi`, `#servizi`, `#galleria`, `#contatti`, `footer`, `#tweaks-panel`
5. **`<script>`** — tutto il JS inline, incluso i18n, scroll, animazioni, tweaks, hamburger

## Design system

Variabili CSS in `:root`:

| Variabile | Valore | Uso |
|---|---|---|
| `--antracite` | `#2B2B2B` | Sfondo principale |
| `--oro` | `#C9A96E` | Accento / CTA |
| `--champagne` | `#E8DFC8` | Testo primario chiaro |
| `--grigio-caldo` | `#9A8F7E` | Testo secondario |
| `--navy-dark` | `#1A2A4A` | Sfondo sezione Servizi e gallery |

Font: Georgia serif esclusivamente. Stile luxury/minimal con letterspacing elevato e font-weight 300.

## Internazionalizzazione (i18n)

Il sito supporta IT/EN. Tutte le stringhe sono nell'oggetto `T` (IT e EN) nel `<script>` finale. Gli elementi HTML usano tre attributi:
- `data-i18n="chiave"` → sostituisce `textContent`
- `data-i18n-html="chiave"` → sostituisce `innerHTML` (usato dove ci sono tag `<strong>` o `<a>` nel testo)
- `data-i18n-placeholder="chiave"` → sostituisce `placeholder` dei form input

La lingua viene rilevata automaticamente: preferenza salvata in `localStorage('lh-lang')` → lingua del browser → fallback IT.

## Tweaks panel (modalità edit)

Il pannello `#tweaks-panel` è nascosto di default e si attiva via `window.postMessage({ type: '__activate_edit_mode' })`. Permette di cambiare live: sfondo hero, griglia spazi (2 o 3 colonne), colore accento. Comunica le modifiche al parent frame via `postMessage` con tipo `__edit_mode_set_keys`.

## Placeholder immagini

Le sezioni galleria, spazi e intro usano div `.img-placeholder` / `.card-img-ph` / `.gal-item` al posto delle foto reali. Quando si aggiungono immagini reali, questi vanno sostituiti con tag `<img>` mantenendo la stessa struttura di wrapper.

## Asset

| File | Uso |
|---|---|
| `liberty-house-logo.webp` | Logo standard (nav, footer) |
| `liberty-house-logo-oro.webp` | Logo dorato (hero) |
| `favicon.ico` / `favicon_32x32.png` / `favicon_64x64.png` | Favicon browser |
| `liberty_house_favicon_192.webp` / `_512.webp` | Apple touch icon / PWA |
| `File ed immagini non utilizzati/` | Cartella con asset scartati — non referenziata dal sito |
