# Hugo Boilerplate

Boilerplate for creating Hugo projects using [Sass](sass-lang.com "Sass"), [PostCSS](postcss.org "PostCSS"), [Babel](babeljs.io "Babel") and [npm scripts](docs.npmjs.com/misc/scripts "npm scripts").

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

### `npm run build`

Use this to build the website.

##### What it does: 

1. Cleans the `public` directory.
2. Builds all source code. See [Build Process](#build-process "Build Process") for more details.
3. Build the website with `hugo`
4. Minifies the resulting `html` files.

## Structure

```
├── archetypes                // For storing archetypes
│   ├── default.md            // The default archetype
│   └── homepage-content.md   // An archetype for homepage content
├── content                   // For storing content
│   ├── homepage-sections     // For storing homepage content sections
│   │   ├── index.md          // Signifies the headless bundle
│   │   └── section-00.md     // A section of content
│   └── _index.md             // Main homepage .md file; use for front matter
├── layouts                   // For layouts
│   ├── _default              // For default layouts
│   │   ├── baseof.html       // Containing everything up to the <body> tag
│   │   ├── list.html         // Default list page
│   │   └── single.html       // Default single page
│   ├── index.html            // Homepage template
│   └── partials              // For partial templates
│       ├── footer.html       // The footer
│       └── header.html       // The header
├── src                       // For source code
│   ├── js                    // For JavaScript source code
│   │   └── main.js           // Main JavaScript file
│   └── scss                  // For Sass source code
│       ├── components        // For components; header, footer etc
│       ├── layouts           // For layouts; grids etc
│       ├── pages             // For page specific CSS
│       ├── variables         // For Sass variables
│       └── main.scss         // The main .scss file; imports normalize
└── static                    // For static assets; images etc.
```

## Build Process

### Sass
1. `static/css` is cleaned.
2. All `.scss` files are compiled to `.css` using `node-sass`, compressed and outputted to `static/css`.
3. The compiled `.css` files are run through `postcss -u autoprefixer` with source maps disabled.

### JavaScript
1. `static/js` is cleaned.
2. All `.js` are transpiled to ES5 and piped to `uglify-js`.
3. `uglify-js` minifies and mangles the code, and outputs to `static/js/main.js`.

### HTML
1. `hugo` generates the `.html` files.
2. All `.html` files are minified using `html-minifier`.

## Other information

### Multiple content sections

Homepages often use multiple sections of content and the boilerplate attempts to provide an "out-of-the-box" solution to this. It provides:

* A headless leaf bundle `content/homepage-sections` for storing these sections.
* A pre-written `.GetPage` method in the homepage template to fetch them.
* A custom archetype.

##### How to use:

1. Create a section with `hugo new homepage-section/section-nn` (where `nn` is the zero-indexed number of the section).
2. Reference these sections in the homepage template using `{{ (index $section nn).Content }}`.

*__Note__: the front matter of these sections must only contain `title` to ensure correct sorting; hence the custom archetype.*

##### This system can be extended to other pages too:

1. Create another headless leaf bundle `content/my-sections`.
2. Include `{{ $section := (.Site.GetPage "/my-sections").Resources.Match "section*" }}` in the page's template.
3. Name your sections `section-nn`. 
4. Reference the sections with `{{ (index $section nn).Content }}`.

Again, the front matter should only contain a title; either use a custom archetype or ensure by hand.
