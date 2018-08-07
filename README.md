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

## Build Process

### Sass
1. `static/css` is cleaned.
2. All `.scss` files are compiled to `.css` using `node-sass`, compressed and outputted to `static/css`.
3. The compiled `.css` files are run through `postcss -u autoprefixer` with source maps disabled.

### JavaScript
1. `static/js` is cleaned.
2. All `.js` are transpiled to ES5 and piped to `uglify-js`.
3. `uglify-js` minifies and mangles the code, and outputs to `static/js/main.js`

### HTML
1. `hugo` generates the `.html` files.
2. All `.html` files are minified using `html-minifier`.

## Other information

### Multiple content sections

Homepages often use multiple sections of content.

This content can be stored in the headless leaf bundle `content/homepage-sections` as `section-nn.md` (where `nn` is the zero-indexed number of the section) and be referenced in the homepage template using `{{ (index $section nn).Content }}`. You could also use `range` to loop through the variable.

Use `hugo new homepage-sections/section-nn` when creating these sections (to use the custom archetype) or make sure to include only a `title` field in the front matter to ensure the correct ordering of the sections.

This system can be extended to other pages too.

Create another headless leaf bundle `content/my-sections` to store your content sections and include `{{ $section := (.Site.GetPage "/my-sections").Resources.Match "section*" }}` in the page's template to reference them with `{{ (index $section nn).Content }}`.

Again, make sure the front matter only contains a title, either using a custom archetype or by hand.
