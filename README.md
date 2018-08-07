# Hugo Boilerplate

Boilerplate for creating Hugo projects using [Sass](sass-lang.com "Sass"), [PostCSS](postcss.org "PostCSS), [Babel](babeljs.io "Babel") and [npm scripts](docs.npmjs.com/misc/scripts "npm scripts").

## Installation

1. `git clone https://gitlab.com/adamrutter/hugo-boilerplate.git`
2. `cd hugo-boilerplate`
3. `npm install`

## Multiple content sections

Homepages often use multiple sections of content.

This content can be stored in the headless leaf bundle `content/homepage-sections` as `section-nn.md` (where `nn` is the zero-indexed number of the section) and be referenced in the homepage template using `{{ (index $section nn).Content }}`. You could also use `range` to loop through the variable.

Use `hugo new homepage-sections/section-nn` when creating these sections (to use the custom archetype) or make sure to include only a `title` field in the front matter to ensure the correct ordering of the sections.

This system can be extended to other pages too.

Create another headless leaf bundle `content/my-sections` to store your content sections and include `{{ $section := (.Site.GetPage "/my-sections").Resources.Match "section*" }}` in the page's template to reference them with `{{ (index $section nn).Content }}`.

Again, make sure the front matter only contains a title, either using a custom archetype or by hand.
