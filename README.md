# Subscriber Counter Simulator

A YouTube Studio-inspired subscriber counter built with plain HTML, CSS, and
JavaScript. Use it for UI practice, mockups, recordings, and milestone demos.

**All counts are simulated.** This page does not connect to YouTube, read real
channel statistics, or change any channel's subscriber count. It is an
independent project with no affiliation to YouTube, Google, or any creator.

## Run locally

Download or clone the repository, then open `index.html` in a modern browser.
Keep the `fonts/` and `img/` folders beside it. No installation, API key,
account, or build step is needed.

For a local HTTP preview, run this command from the project folder with Python 3:

```sh
python3 -m http.server 5173 --bind 127.0.0.1
```

Open [http://127.0.0.1:5173](http://127.0.0.1:5173). Press `Ctrl+C` in the
terminal to stop the server. The page's scripts, fonts, and avatar are all local;
there are no analytics, cookies, or external API requests.

## Counter behavior

The default count starts at **9,980** and changes by one at random intervals
between **6 and 15 seconds**. On its first visit to 9,999, it decreases twice:

```text
9,998 → 9,999 → 9,998 → 9,997 → 9,998 → 9,999 → [6-second hold] → 10,000 → …
```

The dip and hold happen once per page load. After the second visit to 9,999,
the next update is scheduled for six seconds later, then normal growth resumes.
Reloading starts the sequence over; the count is not saved.

Changed digits slide up for increases and down for decreases. Each slide lasts
2.4 seconds and stays clipped inside its digit row. The blue live indicator uses
`#2fa5cb`, with one 3px ring that expands for two seconds and stays hidden for
seven seconds before the next pulse. Reduced-motion preferences disable the
pulse and make digit transitions nearly instant.

Browser timers may run late in background tabs or while the device is asleep.
The toolbar and footer icons are visual elements; they do not open menus or
analytics panels.

## Customize

Edit the constants near the bottom of `index.html`:

| Setting | Default | Purpose |
| --- | --- | --- |
| `START_COUNT` | `9980` | Count shown after loading the page |
| `MIN_DELAY_MS` | `6000` | Shortest regular interval, in milliseconds |
| `MAX_DELAY_MS` | `15000` | Longest regular interval, in milliseconds |
| `MILESTONE` | `9999` | Count that triggers the two decreases and later hold |
| `PAUSE_MS` | `6000` | Interval before leaving the milestone on its second visit |

To use updates every 3–5 seconds, set `MIN_DELAY_MS` to `3000` and
`MAX_DELAY_MS` to `5000`. Use whole, nonnegative counts and positive intervals,
with the minimum no greater than the maximum. The dip needs a milestone of at
least `2`; a starting count above the milestone continues increasing without it.
Keep both the shortest interval and the hold longer than the digit animation.

At the top of the CSS, `--live-color` changes the dot and ring color, and
`--digit-slide-duration` controls both slide directions. Ring thickness is the
border in `.dot::before`. Its nine-second animation cycle contains a two-second
pulse and a seven-second gap; if changing that cycle, update the percentage in
`@keyframes live-pulse` to match.

For a different channel name, replace `Demo Channel` in the page title, avatar
`alt` text, and `<h1 class="name">`. To change the avatar, update the image's
`src` and the favicon link, or replace `img/demo-avatar.svg`. Use an image you
have permission to redistribute. Keep the simulation label clear when sharing
the page or a recording.

## Publish on GitHub

Create an empty public repository on GitHub. Add the files listed below,
including the dotfiles, then commit and push them. This project already supplies
a README and license, so GitHub does not need to generate those files.

The existing `img/channel_profile.jpg`, if present in a local working folder,
is a personal asset excluded by `.gitignore`. The public demo uses the generic
SVG avatar. `.gitignore` does not filter manual browser uploads or remove files
that were already committed; review the files you upload.

To host the page itself, open **Settings → Pages**, choose **Deploy from a
branch**, then select your source branch (usually `main`) and **/(root)**.
Save and use the URL GitHub provides. The included `.nojekyll` file keeps the
site as plain static files. See GitHub's [publishing source instructions](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
and [static site guidance](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site).

## Files

```text
index.html                  Page, styles, and counter logic
img/demo-avatar.svg         Generic demo avatar
fonts/Roboto-Regular.ttf    Bundled font, regular weight
fonts/Roboto-Medium.ttf     Bundled font, medium weight
fonts/Roboto-Bold.ttf       Bundled font, bold weight
fonts/LICENSE.txt          Apache 2.0 license for the fonts
README.md                  Setup and customization
LICENSE                    MIT license for the project
THIRD_PARTY_NOTICES.md      Font attribution and branding notes
.gitignore                 Files to keep out of Git
.gitattributes             Text and binary file handling
.nojekyll                  Static GitHub Pages support
```

## Contributing

Issues and pull requests are welcome. Keep the page buildless and avoid adding
dependencies for changes that plain HTML, CSS, and JavaScript can handle.
Include a screenshot for visual changes and describe how behavior changes were
checked. To check the milestone sequence quickly, temporarily start at `9998`,
then restore the default before submitting. Check both desktop and mobile
layouts and respect reduced-motion preferences.

## License

Project code, documentation, the demo avatar, and project SVG icons use the
[MIT license](LICENSE). You can use, modify, share, and sell copies, including
for commercial projects, provided you retain the copyright and license notice.
See the [MIT license text](https://opensource.org/license/mit) for the full terms.

Bundled Roboto fonts remain under [Apache 2.0](fonts/LICENSE.txt), with credits
in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). Those font terms apply
alongside the project's MIT license.
