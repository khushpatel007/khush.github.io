# Portfolio

A one-page site plus short notes. It is static: HTML, one stylesheet, and an SVG favicon. There is no build step, no framework, and no package manager. You change the site by editing the files and refreshing the browser.

The words on the page right now are samples (a made-up name, three projects, two jobs, two notes). Replace them before you send the link to anyone.

## See it on your machine

From this folder:

```bash
python3 -m http.server 8080
```

Open [http://127.0.0.1:8080](http://127.0.0.1:8080).

Use a local server. Opening `index.html` as a file works for the homepage, but it is easier to break relative links (`notes/setup.html`, `../styles.css`) that way, and you will not be looking at the site the way a host will serve it.

Stop the server with Ctrl-C.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole portfolio: name, intro, work, experience, notes |
| `styles.css` | All of the layout and color |
| `favicon.svg` | The tab icon. The letter inside it should match your name |
| `notes/setup.html` | A sample second page, linked from Notes |
| `README.md` | This file |

Anything you add (a photo, another note, a résumé PDF) lives in this same folder. Paths in the HTML are relative, so the site can be hosted from any directory.

## Rules that keep the page looking like this

Follow these when you add something. They matter more than any single CSS trick.

- One page is the portfolio. Extra pages are for a single note or a single project write-up, not for splitting Work / Experience / Notes into their own marketing pages.
- An item is a year, a title, and one or two sentences. A third sentence usually means it should be its own note.
- Say what the thing is and what you did. Skip slogans ("passionate", "seamless", "robust", "cutting-edge", "leveraging").
- The stack line is optional. If you include it, list tools you actually used, lowercase, separated by a middle dot `·`.
- Do not add a card grid, icon row, skill pills, gradient, or a second typeface from a font CDN. The page is Palatino (or Georgia, if Palatino is missing) plus the system monospace.
- Do not invent metrics. If you want a number, it has to be a real one you can defend in an interview.
- Keep the heading order: one `h1` (your name, or the note title), then `h2` for sections, then `h3` for each item.

## Replace the samples before you publish

Search the folder for these strings. Each one is fake.

| Find | Where | Replace with |
| --- | --- | --- |
| `Your Name` | `index.html` (title, h1, meta description), `notes/setup.html` (title, home link), `favicon.svg` | Your name |
| `City` | the kicker in `index.html` | Where you are, or delete `· City` |
| `you@example.com` | the mailto link | Your email. Change both the `href` and the visible text |
| `your-handle` | GitHub and LinkedIn links | Your real usernames, or delete the link |
| `Invoice service`, `Shift roster`, `Log shelf` | Work section | Your projects. Delete any you do not replace |
| `Northwind Labs`, `Campus IT help desk` | Experience section | Your jobs |
| `Setup notes`, `Reading list` | Notes section | Your notes, or delete the section |
| `Sample content. Replace the name…` | the `.draft` paragraph | Delete the whole paragraph |
| `<meta name="robots" content="noindex">` | both HTML files | Delete the tag so the site can be indexed |

Also rewrite the lede and the Now line. They are written in a generic voice so the layout has real sentences in it. They are not your bio.

## Edit your name and intro

In `index.html`, the top of the page is this block:

```html
<p class="kicker">Software engineer · City</p>
<h1>Your Name</h1>
<p class="lede">
  I build software for a living, mostly services and the data behind them.
  I like a small codebase, a clear owner, and a change I can explain six months later.
</p>
<p class="now">
  <span class="label">Now</span>
  Looking for a software engineering role. Backend and product work both fit.
</p>
```

- `kicker` is one line: role, then an optional place. Two clauses is the limit. Example: `Software engineer · Austin`.
- `h1` is your name, and only your name. Do not put a tagline in the `h1`.
- The lede is two or three sentences. Concrete beats a list of virtues. Mention the kind of work you want a stranger to associate with you.
- The Now line is what is true this season: a job search, a team, a thing you are building. Update it when it goes stale. Keep the `<span class="label">Now</span>` so the label stays styled. To remove the line, delete the whole `<p class="now">…</p>`.

Then update the same name in:

1. `<title>` in `index.html`
2. `<meta name="description">` in `index.html`
3. The letter in `favicon.svg` (see below)
4. `<title>` and the home link in every file under `notes/`

The `<title>` pattern is `Your Name — Software engineer` on the homepage and `Note title — Your Name` on a note.

## Edit links

Links live in a plain list under the lede:

```html
<ul class="links">
  <li><a href="mailto:you@example.com">you@example.com</a></li>
  <li><a href="https://github.com/your-handle">GitHub</a></li>
  <li><a href="https://www.linkedin.com/in/your-handle">LinkedIn</a></li>
</ul>
```

To add one, duplicate an `<li>` and put it in the order you want people to try. The visible text should say where the link goes (`GitHub`, `Résumé`, a full email). Do not use "click here".

Résumé PDF, if you want one:

1. Put the file in this folder, for example `resume.pdf`.
2. Add `<li><a href="resume.pdf">Résumé</a></li>`.

To remove a link, delete its `<li>`, including the closing tag. Leave at least the email.

Do not add `target="_blank"`. Let the browser open links normally.

The small nav under the links (`Work · Experience · Notes`) points at section ids. If you rename a section, change the `href`, the section `id`, and the `aria-labelledby` target together. If you delete a section, delete its `<a>` and the `<span class="sep">` in front of it.

## Add a project

Projects are the articles inside `<section id="work">`. Copy an existing `<article class="entry">` and paste it where you want it in the list. Top of the list is the newest, or the one you most want read. The page does not sort for you.

Blank project:

```html
<article class="entry">
  <p class="year">2026</p>
  <div class="entry-body">
    <h3>Project name</h3>
    <p>
      One or two sentences. What it is, who it was for, and what you personally did.
    </p>
    <p class="stack">language · datastore · where it runs</p>
  </div>
</article>
```

Fields:

| Piece | Required | Notes |
| --- | --- | --- |
| `year` | yes | Four digits, the year you shipped it or the year you want it judged by. A range is fine: `2024–2026`. Use a real en dash `–`, not a hyphen |
| `h3` | yes | The name a person would recognize. Not a slogan |
| description | yes | One or two sentences in a `<p>`. No bullet list inside a project |
| `stack` | no | Delete the whole `<p class="stack">` if you have nothing honest to put there |
| link | no | Wrap the name in `<a href="...">` only when there is a real URL |

With a link, only the title changes:

```html
<h3><a href="https://github.com/your-handle/project">Project name</a></h3>
```

The URL can be a repo, a live site, or a write-up. If the repo is private, do not link it. Describe it in the sentences instead.

A project with no stack and no link is still valid. Shift roster on the homepage is that shape.

If a project needs more than two sentences, make a note (next sections) and link the title to that note:

```html
<h3><a href="notes/invoice-service.html">Invoice service</a></h3>
```

## Change or remove a project

To change one, edit its article in place. You do not need to touch CSS.

To remove one, delete from `<article class="entry">` through the matching `</article>`. Do not leave an empty article. Afterward, the first project should still sit directly under the Work `h2`.

To reorder, cut the whole article and paste it above or below another one.

## Add a job

Jobs use the same entry shape, inside `<section id="experience">`, with a role line added:

```html
<article class="entry">
  <p class="year">2024–2026</p>
  <div class="entry-body">
    <h3>Company</h3>
    <p class="role">Software engineer</p>
    <p>
      One sentence on what you owned. A second sentence only if the first one hid the interesting part.
    </p>
  </div>
</article>
```

- The year column is the span you worked there: `2024–2026`, or `2026–` if you are still there.
- `h3` is the company or the organization, not your title.
- `role` is the title. Keep the class `role` so it stays in the muted color.
- No stack line on a job, unless the tools are the story. If they are, reuse `<p class="stack">`.

School, a research group, or a long contract can be an entry here. Freelance can be one entry (`Independent`, role `Contract engineer`) rather than a fake company per client. Put the notable client work in Work, not as five extra jobs.

## Add a note

A note has two parts: a row on the homepage, and a page.

**1. The row**, inside `<section id="notes">`:

```html
<article class="entry">
  <p class="year">2026</p>
  <div class="entry-body">
    <h3><a href="notes/my-note.html">Short title</a></h3>
    <p>One sentence on why someone should open it.</p>
  </div>
</article>
```

A note with no page yet is allowed. Drop the `<a>` and leave the title as text, the way Reading list is done. Use that for something you might write later. Do not link to a file you have not created.

**2. The page.** Copy `notes/setup.html` to `notes/my-note.html` and replace the title, the dateline, and the text inside `<main>`.

Skeleton:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="theme-color" content="#f2f3f0">
  <title>Short title — Your Name</title>
  <meta name="description" content="One sentence, same idea as the homepage row.">
  <link rel="icon" href="../favicon.svg" type="image/svg+xml">
  <link rel="stylesheet" href="../styles.css">
</head>
<body>
  <a class="skip" href="#main">Skip to content</a>
  <div class="page">
    <header>
      <p class="home"><a href="../index.html">Your Name</a></p>
      <h1>Short title</h1>
      <p class="dateline">October 2026</p>
    </header>
    <main id="main" class="essay">
      <p>First paragraph.</p>
      <p>Second paragraph.</p>
    </main>
    <footer>
      <p class="colophon">Last updated October 2026.</p>
    </footer>
  </div>
</body>
</html>
```

The stylesheet link has to stay `../styles.css`. The home link has to stay `../index.html`. Both point one folder up. If you nest deeper (`notes/2026/my-note.html`), those become `../../styles.css` and `../../index.html`. Prefer keeping notes flat in `notes/`.

Inside `<main class="essay">` you can use:

- `<p>` for paragraphs
- `<ul>` and `<li>` for a real list
- `<code>` for a command, filename, or identifier
- a figure, documented below

Do not paste a second `<h1>`. Subsections inside a note, if you need them, are `<h2>`.

To take a note down, delete the file and delete the homepage article that links to it. A link to a missing file is worse than no note.

## Add a picture

Create a folder named `images` next to `index.html` and put the file there. Export photos no wider than 1600 pixels. Prefer `.jpg` for photos and `.svg` or `.png` for a diagram you drew.

On a note page, inside `<main class="essay">`:

```html
<figure class="figure">
  <img src="../images/invoice-flow.jpg" alt="Orders move through a queue into an invoices table, with failures recorded beside them." width="1200" height="800">
  <figcaption>The posting path, minus the retry loop.</figcaption>
</figure>
```

On the homepage, the same figure works inside `.entry-body`, but change the path to `images/invoice-flow.jpg` (no `../`) and think twice. A homepage full of screenshots stops being a list someone can scan. One image on a project you really want seen is enough.

`alt` describes the picture for someone who cannot see it, and for you in six months. Do not write "screenshot" and stop. If the image is decorative, use `alt=""` and drop the figcaption.

Set `width` and `height` to the file’s pixel size so the page does not jump when the image loads.

## Add a section

You probably do not need a new section. Work, experience, and notes cover a portfolio. If you do (talks, teaching), copy this under `<main>`, after a section you want it to follow:

```html
<section id="talks" aria-labelledby="talks-heading">
  <h2 id="talks-heading">Talks</h2>

  <article class="entry">
    <p class="year">2026</p>
    <div class="entry-body">
      <h3>Title of the talk</h3>
      <p>Where it was, and one sentence on the argument.</p>
    </div>
  </article>
</section>
```

Then add it to the nav:

```html
<span class="sep" aria-hidden="true"> · </span>
<a href="#talks">Talks</a>
```

The `id` on the section and the `href` must match, including the `#`. The `aria-labelledby` value must match the `h2`’s `id`.

A section with zero articles should not be on the page. Delete it until you have one real item.

## Colors and type

All of the color is at the top of `styles.css`:

```css
:root {
  --bg: #f2f3f0;
  --ink: #1a1c1a;
  --muted: #4c514c;
  --line: #d4d7d1;
  --link: #1d3f73;
  --mark: #d7e4cf;
}
```

| Variable | Used for |
| --- | --- |
| `--bg` | Page background. Also update `<meta name="theme-color">` in each HTML file to the same value |
| `--ink` | Text, and the focus outline |
| `--muted` | Kicker, years, role, stack, footer |
| `--line` | Rules above section titles and the footer |
| `--link` | Link text |
| `--mark` | Text selection |

After you change `--bg` or `--ink`, check two things: body text is still easy to read, and `--muted` is not so light it disappears. A quick test is to look at the year column and the Work label. If you have to squint, darken `--muted`.

Type is `--serif` and `--mono` in the same block. Leave them as system fonts unless you have a reason. If you add a webfont, host the files yourself in a `fonts/` folder and add a single `@font-face` at the top of `styles.css`. Do not add a stylesheet from Google Fonts or Adobe Fonts. The page should still be readable if that request fails, which is why the current stack names Palatino, then Georgia.

Spacing you might actually touch:

- `.page` `max-width` is the line length. `40rem` is intentional. Wider makes the lede hard to read.
- `.entry` columns are `6.25rem` and `1fr`. Widen the first number only if a year label overflows. Ranges like `2023–2025` fit.
- Below 560px the year moves above the title. That breakpoint is the `@media (max-width: 560px)` block. You should not need to edit it for a new project.

## Favicon and the footer date

`favicon.svg` is a dark square and one letter. Change the letter between the `<text>` tags to your initial:

```svg
<text x="16" y="22" text-anchor="middle" font-family="Palatino, Georgia, serif" font-size="18" fill="#f2f3f0">Y</text>
```

If you change `--ink` or `--bg`, change the `fill` on the `<rect>` and on the `<text>` so the icon still matches the page. Rect fill should match the ink color. Text fill should match the page background.

The footer line is hand-written, not generated:

```html
<p class="colophon">Last updated October 2026. Set in Palatino.</p>
```

Change the month when you make a real edit. There is a copy on every note page too. Update the one you touched.

## Delete the draft state

When the copy is yours:

1. Delete the `<p class="draft">…</p>` on `index.html`.
2. Delete `<meta name="robots" content="noindex">` from `index.html` and from every file in `notes/`.
3. On `notes/setup.html`, either rewrite it or delete it. If you delete it, remove the Setup notes article on the homepage.
4. Search the folder for `example.com`, `your-handle`, `Your Name`, `Northwind`, and `Sample`. Nothing should remain.

`noindex` is there so a half-finished sample does not get indexed if you deploy early. Once it is gone, search engines are allowed to index the site. That is what you want for a public portfolio.

## Put it on the internet

Any static host works. The site is the files. You do not run a build.

**GitHub Pages**

1. Put the folder in a git repository.
2. Push it to GitHub.
3. In the repo settings, turn on Pages from the branch you pushed, folder `/`.
4. The site will be at `https://<user>.github.io/<repo>/` unless it is a repository named `<user>.github.io`, in which case it is at `https://<user>.github.io/`.

**Netlify, Cloudflare Pages, or any “upload a folder” host**

Upload this directory as the site root. Build command: none. Publish directory: this folder.

After it is live, click every link: email, GitHub, each project, each note, and the name on a note page (it should return home). View the source of `index.html` and confirm the title and description are yours.

## When the layout breaks

Almost every break is an unclosed tag. Compare what you pasted with the blank project above. In particular:

- Every `<article>` needs `</article>`.
- The year stays outside `.entry-body`. If you put the year inside it, the column alignment collapses.
- Class names are exact: `entry`, `entry-body`, `year`, `stack`, `role`, `lede`, `now`, `label`. A missing hyphen (`entrybody`) silently drops the styling.
- A note page that looks unstyled is almost always `href="styles.css"` instead of `href="../styles.css"`.
- An image box with a broken icon is a wrong path. From `index.html` the path is `images/photo.jpg`. From a file in `notes/` it is `../images/photo.jpg`.
- Smart quotes are fine. A stray `”` that replaced a `"` in a tag will eat the rest of the attribute. Keep straight quotes inside tags.

You do not need to know CSS to add content. If you do edit CSS, change the variables first and reload. Avoid adding a class for a single project. If one project needs a special layout, it is probably a note, not a new homepage design.
