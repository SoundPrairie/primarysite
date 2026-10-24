# Sound Prairie — Static Site

A plain HTML/CSS rebuild of soundprairie.com. No frameworks, no npm, no build
step — every page is hand-written HTML that links a single shared stylesheet.

## File layout

```
/
├── index.html                 Home (/)
├── bio/index.html             Bio (/bio/)
├── store/index.html           Store (/store/)
├── blog/index.html            Blog post list (/blog/)
├── blog/_post-template/       Copy-and-rename starting point for a new post
├── contact/index.html         Contact (/contact/)
├── 404.html                   Not-found page
├── styles.css                 Shared stylesheet for every page
├── nav.js                     Mobile menu toggle (vanilla JS)
├── images/                    All site images (headshot, score covers,
│                               logos, favicons)
└── README.md                  This file
```

Each page folder keeps the original Squarespace URL structure, so a page
lives at `/<folder>/` (served as `<folder>/index.html`) rather than
`/<folder>.html`. The home page is the one exception, living at the site root
as `index.html`.

All links and image paths are **relative** (`../images/x.jpg`,
`../contact/`), so the whole folder can be moved, zipped, or served from any
sub-path without edits.

## Editing a page

1. Open the page's `index.html` (e.g. `bio/index.html` for the Bio page).
2. Each page is built from a repeated section pattern:
   ```html
   <section class="block block-dark">
     <div class="block-inner">
       <p class="eyebrow">KICKER TEXT</p>
       <h2>Headline</h2>
       <p class="subtext">Supporting copy.</p>
       <div class="btn-wrap">
         <a href="..." class="btn">Button Label</a>
       </div>
     </div>
   </section>
   ```
   Edit the text inside the tags directly — no templating, no build step.
   `block-dark` gives a black section, `block-light` gives a white one.
3. The header/nav and footer markup is duplicated at the top/bottom of every
   page (there's no includes mechanism without a build step). If you change
   the nav or footer, update it in **every** `index.html` file and
   `404.html`, or run a find-and-replace across the project.
4. Images live in `/images/`. Add a new file there and reference it with a
   relative path such as `../images/new-photo.jpg` (one `../` per folder
   level the page sits below the root).
5. Styling lives entirely in `/styles.css`. Shared rules (buttons, section
   blocks, header, footer) are near the top; page-specific rules (bio copy,
   store products) are grouped by page further down.
6. Each product on the Store page is one `<section class="block ...">`
   containing a `.product` grid: cover image, title, a `<dl>` of specs,
   price, description, an optional "Look Inside" link, and a Buy button.
   Sections alternate `block-light` / `block-dark`. To add a product, copy
   one section and edit it.
7. The logo appears in the header (left of the SOUND PRAIRIE wordmark) and
   above the home-page tagline, both using `images/logo-dark.png` (white and
   red, for the black backgrounds). `images/logo-light.png` (gray and red)
   is the version for white backgrounds.
8. The favicon is `images/favicon.svg`, which switches between the gray mark
   (light browser theme) and the white mark (dark browser theme).
   `images/favicon.png` (gray) is the fallback for browsers that don't use
   SVG favicons, such as Safari.
9. Buy buttons and the Contact button are `mailto:` links to the placeholder
   address `your-email@example.com`. Buy links use the subject
   `Order: <title>` (URL-encoded, e.g. `?subject=Order%3A%20Suspended`).
   Replace each Buy `href` with a real checkout link when one exists.

## Adding a blog post

1. Copy the `blog/_post-template/` folder and rename the copy to the post's
   URL slug, e.g. `blog/first-post/`. The post will live at `/blog/first-post/`.
2. In the copy's `index.html`, replace `Post Title` (in `<title>` and `<h1>`),
   `Month DD, YYYY`, and the paragraphs. Delete the
   `<meta name="robots" content="noindex">` line.
3. In `blog/index.html`, copy the example `<article class="post-item">` block
   out of the HTML comment and paste it just below the comment (newest post
   first). Fill in the date, title, summary, and slug, and delete the
   `Posts coming soon.` line.

Post pages sit two folders deep, so their paths use `../../` (e.g.
`../../styles.css`); the template already has these set.

The template folder starts with an underscore, so GitHub Pages' default
Jekyll build leaves it out of the published site. On other hosts it is
published, but its `noindex` tag keeps search engines from listing it.

## Previewing locally

From this folder:

```
python3 -m http.server 8000
```

Then open `http://localhost:8000/` in a browser. Because every page lives in
its own folder with an `index.html`, links like `/bio/` resolve correctly
without any server configuration.
