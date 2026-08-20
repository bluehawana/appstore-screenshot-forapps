---
name: mac-app-store-screenshots
description: Generate Mac App Store and marketing screenshots for a macOS app. Composites real app captures onto branded slides with headlines, and exports every size the Mac App Store accepts. Use when someone needs App Store screenshots, store listing images, or marketing shots for a Mac app. Not for iOS or Android — those have different sizes and different tools.
---

# Mac App Store screenshots

Most screenshot generators target iPhone and Android. The Mac App Store wants
something different, and nothing else in the ecosystem covers it well.

## What Apple requires

| Requirement | Value |
|---|---|
| Aspect ratio | 16:10 landscape |
| Accepted sizes | 1280×800, 1440×900, 2560×1600, 2880×1800 |
| Format | RGB PNG or JPEG, **flattened, no alpha** |
| Count | 1 minimum, 10 maximum |
| Content | Must show the **real** app interface |

That last rule is the one that gets listings rejected. Apple will reject a
marketing illustration, a mockup, or an iOS build shown as a Mac app. You may
put the real window on a background and add captions — that is exactly what
this generates.

## How to use it

**1. Capture the real app.** Take normal screenshots of the running app.
Window captures (⌘⇧4 then Space) work well; the shadow is trimmed
automatically by the rounded frame.

**2. Write a config.** Copy `screenshots.example.json`:

```json
{
  "imageDir": "docs/images",
  "outputDir": "docs/appstore",
  "slides": [
    {
      "file": "capture.png",
      "headline": "One claim,\nbroken across two lines.",
      "subhead": "One supporting sentence. Keep it under about twenty words.",
      "tint": [0.31, 0.76, 0.85]
    }
  ]
}
```

`tint` is sRGB 0–1. It colours the accent rule and a soft background wash, so
slides read as a set while staying individually distinguishable in a listing.

**3. Generate.**

```bash
swift makeshots.swift screenshots.json
```

Outputs every accepted size into subdirectories of `outputDir`. Upload the
2880×1800 set — Apple downscales for smaller displays, and starting from the
largest gives the sharpest result on Retina.

## Writing the slides

The first slide does almost all the work; many people never scroll past it.
Lead with the problem in the user's own words, not a feature name.

- **Headline**: a claim, not a label. "Your charger says 100 W. You are
  getting 60." beats "Power monitoring". Use `\n` to control the line break
  rather than letting it wrap somewhere awkward.
- **Subhead**: one sentence. Say what the app does about the problem the
  headline names.
- **Order**: problem → mechanism → resolution → who it is for. Each slide
  should still make sense alone, since people land on them out of order.

## Check before uploading

Verify each capture shows the **current** build. A screenshot taken before a
bug fix will happily advertise the bug — particularly damaging when the
headline claims the app gets something right and the image shows it getting
that thing wrong. Compare the app's build time against the screenshot's
timestamp if there is any doubt.

Also confirm no personal data is visible: account names, file paths
containing your home directory, notification content, other windows.

## Requirements

macOS with Xcode command line tools. Uses only CoreGraphics, CoreText and
ImageIO — no dependencies, no network, nothing to install.
