# Hugo Boilerplate

The boilerplate I use for creating [Hugo](gohugo.io "Hugo") projects. It compiles Sass, transpiles JavaScript, creates SVG sprites and adds some other extras.

It uses:

* [Sass](sass-lang.com "Sass")
* [Normalize](necolas.github.io/normalize.css "Normalize")
* [PostCSS](postcss.org "PostCSS")
* [Babel](babeljs.io "Babel")
* [stylelint](stylelint.io "stylelint")
* [ESLint](eslint.org "ESLint")
* [svgo](github.com/svg/svgo "svgo")
* [svg-sprite-generator](github.com/frexy/svg-sprite-generator "svg-sprite-generator")
* [npm scripts](docs.npmjs.com/misc/scripts "npm scripts")

## Installation

1. `git clone https://gitlab.com/adamrutter/hugo-boilerplate.git`
2. `cd hugo-boilerplate`
3. `npm install`

## Usage

### `npm start`

Use this for development. Changes made to any files will be immediately reflected on [http://localhost:1313](http://localhost:1313).

##### What it does:

1. Starts the Hugo live server.
2. Compiles/minifies Sass to `static/css` upon changes in `src/scss`.
3. Transpiles/minifies JavaScript to `static/js` upon changes in `src/js`.
4. Optimises SVG to `static/svg` and builds a sprite at `static/svg/sprite.svg` upon changes in `src/svg`.

### `npm run build`

Use this to build the website.

##### What it does:

1. Cleans the `public` directory.
2. Builds all source code. See [Build Process](#build-process "Build Process") for more details.
3. Builds the website with `hugo`.
4. Minifies the resulting `html` files.

### `npm run start:preview`

Similar to `npm start`. Use this when you want the live server to include drafts and future content during development.

##### What it does:

1. Starts the Hugo live server with the flags `--buildFuture --buildDrafts`.
2. Compiles/minifies Sass to `static/css` upon changes in `src/scss`.
3. Transpiles/minifies JavaScript to `static/js` upon changes in `src/js`.
4. Optimises SVG to `static/svg` and builds a sprite at `static/svg/sprite.svg` upon changes in `src/svg`.

### `npm run build:preview`

Similar to `npm run build`. Use this to include drafts and future content in your build.

##### What it does:

1. Cleans the `public` directory.
2. Builds all source code. See [Build Process](#build-process "Build Process") for more details.
3. Builds the website with `hugo --buildFuture --buildDrafts`.
4. Minifies the resulting `html` files.

### `npm run lint:all`

Use this to lint your source code.

##### What it does:

1. Lints all `.scss` code in `src` with stylelint, using `stylelint-config-standard`.
2. Lints all `.js` code in `src` with ESLint, using `eslint-config-google`.

## Build Process

### Sass
1. `static/css` is cleaned.
2. All `.scss` files are compiled to `.css` using `node-sass`, compressed and output to `static/css`.
3. The compiled `.css` files are run through `postcss -u autoprefixer` with source maps disabled.

### JavaScript
1. `static/js` is cleaned.
2. All `.js` are transpiled to ES5 and piped to `uglify-js`.
3. `uglify-js` minifies and mangles the code, and outputs to `static/js/main.js`.

### SVG
1. Any files in `src/svg` are optimised using `svgo` and output to `static/svg`.
2. `static/svg/sprite.svg` is cleaned.
3. All `.svg` files in `static/svg` are used to build a sprite at `static/svg/sprite.svg`.

__Note__: *`src/svg` must have no further sub-directories or the build will fail due to errors from `svgo`*

### HTML
1. `hugo` generates the `.html` files.
2. All `.html` files are minified using `html-minifier`.

## Structure

```
├── archetypes                 - For storing archetypes
│   ├── default.md             - The default archetype
│   └── sections.md            - An archetype for sections of content, see below
│
├── content                    - For storing content
│   ├── _index.md              - Main homepage .md file; use for front matter
│   ├── page-1.md              - A default page
│   │
│   ├── sections               - The content sections directory
│   │   │
│   │   └── home               - For storing homepage content sections
│   │       ├── index.md       - Signifies the headless bundle
│   │       └── section-00.md  - A section of content
│   │
│   └── sub-directory          - A sub-directory
│       ├── _index.html        - Front matter/content for the directory list page
│       └── page-2.md          - A default page
│
├── layouts                    - For layouts
│   ├── 404.html               - 404 template
│   ├── index.html             - Homepage template
│   │
│   ├── _default               - For default layouts
│   │   ├── baseof.html        - Containing everything up to the <body> tag
│   │   ├── list.html          - Default list page
│   │   └── single.html        - Default single page
│   │
│   ├── page                   - For top level page templates
│   │   └── page-1.html        - A template example for page-1.md
│   │
│   ├── partials               - For partial templates
│   │   ├── footer.html        - The footer
│   │   └── header.html        - The header
│   │
│   └── sub-directory          - Templates for content of the sub-directory type
│       └── single.html        - Single page template for sub-directory
│
├── src                        - For source code
│   │
│   ├── js                     - For JavaScript source code
│   │   └── main.js            - Main JavaScript file
│   │
│   ├── scss                   - For Sass source code
│   │   ├── main.scss          - The main .scss file; imports normalize
│   │   │
│   │   ├── layouts            - For layouts; grids etc
│   │   ├── pages              - For page specific CSS
│   │   ├── variables          - For Sass variables
│   │   └── components         - For components; header, footer etc
│   │
│   └── svg                    - For SVG files
│
└── static                     - For static assets; images etc.
```

## Other information

### Custom Templates

Pages use templates based on their __content type__. This means you can override the default layouts (the ones in `layouts/_default`), if you have defined a custom layout for the page's content type.

Content type is set using the front matter variable `type`. If the variable is not set, it defaults to the page's parent directory within `content`.

##### Single/List Templates

To use custom layouts for pages in `content/sub-directory`, you need one or both of:

* `content/sub-directory/single.html`
* `content/sub-directory/list.html`

Pages within `sub-directory` will now use these layouts by default.

##### Individual Templates

Custom layouts for specific, individual pages follow the same structure. However, the layout must be specified in the front matter.

For example, to use a custom layout for `content/sub-directory/page-2.md`:

1. Create your layout in `content/sub-directory/page-2.html`.
2. Specify `layout: "page-2"` in the front matter of `page-2.md`.

*__Note__: The content type must still match*

##### Top-level pages

As pages at the top-level of the `content` directory (eg. `content/page-1.md`) have a `type` of `pages`, their layouts go in `layouts/pages`.

You could also set a custom `type`.

### Multiple content sections

Homepages often use multiple sections of content and the boilerplate attempts to provide an "out-of-the-box" solution to this. It provides:

* A headless bundle `content/sections/home` for storing these sections.
* A pre-written `.GetPage` method in the homepage template to fetch them.
* A custom archetype.

##### How to use:

1. Create a section with `hugo new sections/home/section-nn.md` (where `nn` is the zero-indexed number of the section).
2. Reference these sections in the homepage template using `{{ (index $section nn).Content }}`.

*__Note__: the front matter of these sections must only contain `title` to ensure correct sorting; hence the custom archetype.*

##### This system can be extended to other pages too:

1. Create another headless bundle `content/sections/my-page`.
2. Include `{{ $section := (.Site.GetPage "/sections/my-page").Resources.Match "section*" }}` in the page's template.
3. `hugo new /sections/my-page/section-nn.md`.
4. Reference the sections with `{{ (index $section nn).Content }}`.

Again, the front matter should only contain a title; using `hugo new` ensures this.

### Menus

Two menus are included:

* Header
* Footer

The header menu includes `class="active"` for the current page.
