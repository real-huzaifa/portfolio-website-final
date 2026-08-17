# Persona-Styled Developer Portfolio

A single-page personal portfolio built as a game-style menu, inspired by the UI design
language of the *Persona* series — jagged cut-out typography, skewed panels, diagonal
scene transitions and animated live-wallpaper backgrounds.

Built by **Ahmed Huzaifa Malik** — Data Analyst / Data Scientist.

**Live site:** _add your Vercel URL here after deploying_

---

## Features

- **Game-menu navigation** — five sections driven by mouse or keyboard (`↑` `↓` `Enter` `Esc`)
- **Three-layer diagonal wipe** between sections — cream, black and blue panels sweeping on
  a stagger, mimicking the reference UI
- **Staggered entrance animations** — title letters snap in, menu items slide in one by one,
  skill bars fill in sequence
- **Live video wallpapers** — a different animated background per section, desktop only
- **Background music** with a toggle control
- **Fully responsive** — collapses to a stacked single-column layout on mobile
- **Accessible** — visible keyboard focus, `prefers-reduced-motion` support, semantic markup

## Tech

Plain **HTML**, **CSS** and **vanilla JavaScript**. No framework, no build step, no
dependencies, no package manager. The entire site is one `index.html` file plus media assets.

Fonts are Anton and Archivo, loaded from Google Fonts.

## Structure

```
.
├── index.html                    entire site (markup, styles, scripts)
├── README.md
├── Ahmed_Huzaifa_Malik_CV.pdf    linked from the About section
└── art/
    ├── *.mp4                     section background wallpapers
    ├── *.jpg                     poster frames
    └── theme.mp3                 background music
```

Paths are relative — keep `index.html` and `art/` together.

## Running locally

Open `index.html` directly in a browser, or serve the folder:

```bash
python -m http.server 8000
```

Then visit <http://localhost:8000>.

> The browser window must be **wider than 900px** for the artwork panel to appear.
> Below that it is hidden by design and the videos are never downloaded.

## Deploying

The site is fully static, so any static host works.

**Vercel**

1. Push this repo to GitHub
2. Import it at [vercel.com/new](https://vercel.com/new)
3. Framework preset: **Other** — leave build command and output directory empty
4. Deploy

No `vercel.json` or configuration is required.

## Performance notes

Source wallpapers were roughly 26 MB each at 1080p60. They were re-encoded to 720p30 with
audio stripped, bringing the total media payload from ~160 MB to under 10 MB.

| Technique | Effect |
|---|---|
| `preload="none"` + JS-assigned `src` | mobile visitors never download the videos |
| Width-gated loading (`min-width: 901px`) | artwork skipped entirely on small screens |
| Only the active section's video plays | avoids six simultaneous decode loops |
| Poster frames (~20 KB each) | panel renders instantly while video streams |
| `prefers-reduced-motion` | disables all animation for users who ask for it |

To show wallpapers on tablets, lower the `901px` threshold in the `WIDE` media query near
the bottom of the script.

## Customising

| What | Where in `index.html` |
|---|---|
| Colour palette | `:root` block — six hex values |
| Name in the title | `const NAME` at the top of the script |
| Intro bio | `.bio` in the `home` section |
| Projects | the `.card` blocks in the `projects` section |
| Experience | the `.exp` blocks |
| Skill ratings | `data-v="88"` plus the matching `<span class="val">` |
| Contact details | `.contact-list` |

### Section → background mapping

| Section | Background |
|---|---|
| Home | `art/yuki.mp4` |
| Projects | `art/akihiko.mp4` |
| Experience | `art/mitsuru.mp4` |
| Skills | `art/aigis.mp4` |
| About | `art/kotone.mp4` |
| Contact | `art/yukari.mp4` |

To swap one, drop a new `.mp4` into `art/`, update that section's `data-src`, and generate
a poster frame:

```bash
ffmpeg -ss 1 -i art/new.mp4 -frames:v 1 -vf "scale=640:-2" -q:v 6 art/new.jpg
```

Each video is framed individually via `object-position`, because the panel only shows the
middle ~46% of a 16:9 clip and a character whose face sits near a frame edge would
otherwise be cropped out. Adjust the matching rule in the stylesheet if a face sits wrong.

## Credits

- UI design language inspired by **Persona 5** / **Persona 3 Reload** (ATLUS)
- Background wallpapers: **Persona 3 Reload** artwork, © ATLUS / SEGA
- Background music: *Full Moon Full Life*, © ATLUS / SEGA

> **Note on assets.** The artwork and music in this repository are copyrighted by ATLUS and
> are included here for personal, non-commercial use only. If you fork this project, replace
> them with media you own or have licensed before publishing.

## Licence

The code in this repository is free to use and adapt. The bundled media assets are **not** —
see Credits above.
