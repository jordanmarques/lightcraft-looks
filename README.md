# lightcraft-looks

A versioned library of looks for [LightCraft](https://github.com/storytold/lightcraft).
One `.lcpreset` file per look: it is plain JSON, so `git diff` shows exactly which setting changed.

## The looks

| File | Look | In short |
|---|---|---|
| `leica-m.lcpreset` | Leica M | Colour rendering of recent digital Leica M bodies: dense blacks, gentle highlights, calmer greens, deep blues. |
| `tri-x-400.lcpreset` | Tri-X 400 | Tri-X 400 at box speed: classic black and white, full tonal range, firm but not harsh contrast, moderate grain. |
| `tri-x-400-pushed-moriyama.lcpreset` | Tri-X 400 Pushed Moriyama | Tri-X 400 pushed two stops, after Daido Moriyama: hard black and white, blocked shadows, heavy grain. |
| `portra-400.lcpreset` | Portra 400 | Soft, slightly overexposed colour negative: low contrast, lifted blacks, warm pastel tones. |
| `cinestill-800t.lcpreset` | CineStill 800T | Tungsten cinema stock: cold blue-green mood, warm lights, a glow on the highlights. |
| `wong-kar-wai.lcpreset` | Wong Kar-wai | *Chungking Express* / *Fallen Angels*: green cast, saturated colours, strong grain, marked contrast. |
| `kodachrome-64.lcpreset` | Kodachrome 64 | Slide film: punchy contrast, dense neutral blacks, deep saturated reds, darker blue skies, very fine grain. |
| `kodak-gold-200.lcpreset` | Kodak Gold 200 | Consumer colour negative: golden warmth, boosted yellows and oranges, muted blues, medium contrast, visible grain. |
| `ektar-100.lcpreset` | Ektar 100 | Colour negative with slide-like punch: vivid saturation, deep reds and blues, firm contrast, very fine grain. |

In LightCraft they live in the **Looks** preset group.

## What a look contains

Every look writes the same groups of settings, including at their neutral value, so that one look
fully replaces the previous one instead of stacking on top of it:

colour or black and white, light, tone curve, colour, colour mixer, colour grading, calibration,
effects, vignette, grain, detail, masks.

These stay **out** of the looks because they belong to each photo: white balance, crop, geometry,
lens corrections.

As a consequence, applying a look overwrites manual edits made in those groups. Apply the look
first, then fine-tune.

## Known limits

- These looks are interpretations tuned by eye from published descriptions, not profiles measured
  on the cameras or film stocks.
- Anything that comes from the optics or from the shooting itself is only imitated, or missing:
  Leica micro-contrast and bokeh, Moriyama's motion blur, Wong Kar-wai's step-printed blur.
- **CineStill 800T**: the halation is approximated by a mask that tints the highlights; it does
  not bleed around light sources the way the real one does. For the full effect, cool the white
  balance by hand (3200–3400 K at night).
- **Wong Kar-wai**: tuned on a night scene. In daylight, lower the preset amount (50–70 %).
- **Portra 400**: not yet checked on a portrait.
- **Kodachrome 64**: the stock was discontinued in 2010, so the look is rebuilt from scans and
  recollections; sources disagree on its blues and greens.
- **Kodak Gold 200**: the warmth comes from colour grading and the mixer, not from white balance.
- **Ektar 100**: like the real stock, it pushes skin tones towards red; not checked on a portrait.
- A full reset of a photo turns off its lens correction (dark corners and curved lines on a wide
  lens): turn it back on before judging a look.

## Installing the looks

In LightCraft: *File → Import Presets*, then pick the `.lcpreset` files (or the folder).

From the command line, against the app started with `--control 7980`:

```sh
lightcraft-cli run --connect 127.0.0.1:7980 preset.import 'paths=["/path/to/lightcraft-looks"]' group=Looks
```

## Updating the repository

The repository does not follow LightCraft on its own: after changing a look in the app, re-export
its file and commit.

```sh
lightcraft-cli run --connect 127.0.0.1:7980 presets.list          # find the preset id
lightcraft-cli run --connect 127.0.0.1:7980 preset.export \
    path="$PWD/leica-m.lcpreset" 'ids=["user.41.leica-m"]'
```

The ids (`user.41.leica-m`, …) are those of the original library; they may differ after an import
on another machine.
