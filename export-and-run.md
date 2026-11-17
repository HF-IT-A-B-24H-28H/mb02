# Slides: Start and Export

The project requires Node.js `>=22.22.0`.

## Install dependencies

From the project root:

```bash
npm ci
```

## Live presentation

Start the Slidev development server:

```bash
npm run dev
```

Open the audience view at:

```text
http://localhost:3030/
```

Open the presenter view at:

```text
http://localhost:3030/presenter
```

The presenter view contains the current slide, next slide, notes, timer, and navigation controls.

## Build HTML

Build the static HTML deck into `slide-deck/`:

```bash
npx slidev build slides.md --out slide-deck --router-mode hash
```

Serve the generated HTML locally:

```bash
python3 -m http.server 8000 --directory slide-deck
```

Open the audience view at `http://localhost:8000`.

The static build is for viewing only. Use the live server for the presenter view and interactive features.

## Export PDF

Export the slides with Slidev's official exporter:

```bash
npx slidev export slides.md --output slide-deck.pdf --wait 1000 --with-toc
```

The PDF is written to `slide-deck.pdf`.

## Rebuild both artifacts

Run both commands after changing `slides.md` or the styles:

```bash
npx slidev build slides.md --out slide-deck --router-mode hash
npx slidev export slides.md --output slide-deck.pdf --wait 1000 --with-toc
```
