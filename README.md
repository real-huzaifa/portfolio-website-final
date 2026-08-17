# Personal Portfolio Website

---

**Live site:** _add your URL here_
 
---
 
## Overview
 
Most developer portfolios are a scrolling page of cards. I wanted mine to be something a
person actually remembers, so I built it as a **game menu** instead — the whole site behaves
like a console UI, driven by keyboard or mouse, with each section arriving behind an
animated scene transition.
 
The visual language is drawn from the *Persona* series: hard diagonal cuts, letters knocked
out into white boxes at irregular angles, and heavy skewed panels. Reproducing that in the
browser meant solving a set of problems that a normal portfolio never runs into — which is
most of what this project is really about.
 
Everything is hand-written HTML, CSS and vanilla JavaScript. No framework, no build step,
no dependencies, no package manager. The entire site is one `index.html` file plus media.

---

## What I built
 
**Procedural title typography.** The name on the landing page isn't an image or hand-marked
spans — a script splits the string per character, randomly boxes roughly a third of the
letters and applies a random rotation within ±5°. Every page load produces a different
arrangement, so the effect is generated rather than authored.
 
**Three-layer scene transition.** Section changes fire three skewed panels — cream, black,
then blue — sweeping across the viewport on a 60ms stagger, with the page swap hidden at
peak coverage. Getting the panels to fully clear afterwards took some debugging; an
incompletely reset clip state left a panel parked over the left of the screen, silently
clipping page content.
 
**Staggered entrance choreography.** Every section replays its own entrance on each visit:
title letters snap in with an overshoot easing, menu items slide in sequentially, cards
rise on a delay chain, and skill bars fill one after another rather than all at once.
Animations are driven through the standalone `translate` and `scale` properties instead of
`transform`, so they compose with the layout's existing `skewX()` rather than overwriting it.
 
**Per-section live video backgrounds.** Each section carries its own animated wallpaper
behind a diagonally clipped panel. Because the panel is portrait and the source clips are
16:9, `object-fit: cover` only exposes the middle ~46% of each frame — enough to crop a
character's face straight out of view. Each video is therefore framed individually with
`object-position`, tuned against the actual subject placement in that clip.
 
**Media pipeline.** Source wallpapers arrived at roughly 26MB each (1080p60 with audio) and
the music track at 11MB. Everything was re-encoded through `ffmpeg` — video to 720p30 with
audio stripped, audio to 96kbps — taking total media weight from about 160MB to under 10MB
with no visible quality loss on flat cel-shaded artwork.
 
**Deliberate loading strategy.** Video sources are held in `data-src` and only assigned by
script above a 900px viewport, so mobile visitors never download them at all. Only the
active section's video plays; the rest stay paused. Poster frames render instantly while
video streams in behind them.
 
**Accessibility.** Full keyboard navigation (`↑` `↓` `Enter` `Esc`), managed focus across
section changes, visible focus states, and `prefers-reduced-motion` support that disables
every animation for users who ask for it.

---

## Features
 
- Game-menu navigation — mouse or keyboard
- Six sections: Home, Projects, Experience, Skills, About, Contact
- Animated diagonal scene transitions
- Live video wallpaper per section, with poster-frame fallbacks
- Background music with a toggle control
- Animated skill meters
- Responsive down to mobile, collapsing to a stacked layout
- Zero dependencies — opens straight from the filesystem

---

## Tools & Technologies
 
**Languages**
| | |
|---|---|
| HTML5 | semantic structure, `<video>` and `<audio>` media elements |
| CSS3 | `clip-path`, `object-fit` / `object-position`, keyframe animation, custom properties, grid, flexbox, media queries |
| JavaScript (ES6) | DOM manipulation, state handling, conditional media loading, keyboard events |
 
**Media processing**
| | |
|---|---|
| FFmpeg | video re-encoding (H.264, 720p30), audio transcoding, poster-frame extraction |
 
**Typography**
| | |
|---|---|
| Anton | display face — titles and menu |
| Archivo / Archivo Narrow | body and UI text |
 
**Tooling**
| | |
|---|---|
| VS Code | development |
| Git & GitHub | version control |
| Vercel | hosting |
 
---

## Structure
 
```
.
├── index.html                    entire site — markup, styles and scripts
├── README.md
├── .gitignore
├── Ahmed_Huzaifa_Malik_CV.pdf    linked from the About section
└── art/
    ├── *.mp4                     section background wallpapers
    ├── *.jpg                     poster frames
    └── theme.mp3                 background music
```
 
Paths are relative — `index.html` and `art/` need to stay together.

---

## Running locally
 
Open `index.html` in a browser, or serve the folder:
 
```bash
python -m http.server 8000
```
 
> The window must be wider than **900px** for the artwork panel to appear. Below that it is
> hidden by design and the videos are never requested.
 
---

## Author
 
**Ahmed Huzaifa Malik** — Data Analyst / Data Scientist
BS Data Science, Pak-Austria Fachhochschule Institute of Applied Sciences and Technology
 
[LinkedIn](https://www.linkedin.com/in/ahmed-huzaifa-malik/) · [GitHub](https://github.com/real-huzaifa) · ahmedhuzaifamalik@gmail.com

---

## Credits
 
- UI design language inspired by **Persona 5** and **Persona 3 Reload** (ATLUS)
- Background wallpapers: **Persona 3 Reload** artwork, © ATLUS / SEGA
- Background music: *Full Moon Full Life*, © ATLUS / SEGA
> **Note on assets.** The artwork and music bundled here are copyrighted by ATLUS and are
> included for personal, non-commercial use only. If you fork this project, replace them
> with media you own or have licensed before publishing.

---

## Licence
 
The code in this repository is free to use and adapt. The bundled media assets are not —
see Credits.
