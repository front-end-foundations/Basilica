# Basilica!

- [Basilica!](#basilica)
  - [Homework](#homework)
  - [Goals](#goals)
  - [Initialize GIT with .gitignore](#initialize-git-with-gitignore)
  - [NPM Initialization and Installs](#npm-initialization-and-installs)
  - [Starter HTML](#starter-html)
    - [Starter CSS](#starter-css)
  - [Images](#images)
  - [Aside - Image Optimization](#aside---image-optimization)
  - [Flex Layout](#flex-layout)
  - [The Branding Header](#the-branding-header)
    - [Custom Fonts](#custom-fonts)
  - [CSS Nesting](#css-nesting)
    - [Header: Responsive Design](#header-responsive-design)
  - [The Navigation](#the-navigation)
    - [Nav Links and Gradients](#nav-links-and-gradients)
  - [CSS Grid](#css-grid)
  - [JavaScript](#javascript)
    - [Aside: Demo Arrays in Node](#aside-demo-arrays-in-node)
    - [Add a Script](#add-a-script)
  - [JavaScript Popover](#javascript-popover)
  - [DOM Scripting Methods Used](#dom-scripting-methods-used)
    - [Matches](#matches)
    - [Add Another Close Method](#add-another-close-method)
    - [Adding Animation to the Modal](#adding-animation-to-the-modal)
  - [A Dynamic Popover](#a-dynamic-popover)
  - [END of Exercise](#end-of-exercise)
  - [Notes](#notes)
  - [Expressions](#expressions)
  - [Statements](#statements)

## Homework

Continue with the JS section on Front End Masters

<!-- ## 1.3. Reading

- See how far you can get in [Grid Garden](http://cssgridgarden.com/)
- MDN on [CSS Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)
- The [Absolute Beginners Guide](https://nodesource.com/blog/an-absolute-beginners-guide-to-using-npm/) to NPM
- [What is GIT?](https://guides.github.com/introduction/git-handbook/) -->

## Goals

- Continue working with Flexbox and introduce CSS Grids
- Understand and set up a CSS toolchain using SASS
- Review basic DOM manipulation techiques
- Understand how to create an element and insert it into the DOM
- Review GIT and Github set up and branching
- Review NPM set up and installing

## Initialize GIT with .gitignore

Download a zip file of this repo and `cd` into the directory.

Initialize a new git repository:

```sh
$ git init
$ git add .
$ git commit -m 'initial commit'
```

Note: the project is prepared for npm. It has a `.gitignore` file with the text `node_modules` in it.

## NPM Initialization and Installs

Create a manifest (package.json) and install packages.

```sh
$ npm init
$ npm install browser-sync sass concurrently --save-dev
```

Review:

- package.json
- [sass](https://www.npmjs.com/package/sass), [concurrently](https://www.npmjs.com/package/concurrently)
- package-lock.json
- hard [dependencies](https://www.npmjs.com/package/flickity) vs devDependencies
- node_modules folder
- the need for a `.gitignore` file

Browser Sync [CLI documentation](https://www.browsersync.io/docs/command-line)

Add an npm command to the scripts section of `package.json`:

```json
"scripts": {
  "start": "browser-sync app -w"
},
```

In VSCode's integrated terminal:

`$ npm run start`

## Starter HTML

![Image of Basilica](other/FINAL.png)

Open `app/index.html` in VSCode and examine the HTML with regards to the [recipe schema](https://schema.org/Recipe). A schema is a semantic vocabulary of tags (or microdata) that you can add to your HTML to improve the way search engines read and represent your page in search engine result pages. There are [many different kinds of schemas](https://schema.org/docs/full.html).

Note the [itemscope](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/itemscope) attribute. There is a good example of [using the recipe schema](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/itemscope#representing_structured_data_for_a_recipe).

Demo: test the HTML with [Rich Results Test](https://search.google.com/test/rich-results?utm_campaign=devsite&utm_medium=jsonld&utm_source=recipe) and add any missing attributes.

There are other types of metadata that enrichness HTML. A popular one for sharing data is [Open Graph Metadata](https://ogp.me/).

Inspect the head of [this recipe page](https://www.epicurious.com/ingredients/our-best-basil-recipes-gallery) and note the `og:` metadata.

Note the `<abbr>` tag.

### Starter CSS

Examine the starter CSS.

Note:

1. The pseudo class `::selection`
1. the use of `max-width` on the body selector
1. the `li > h4` [child selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Child_combinator) used to select elements with a _specific parent_. In this case it will select `h4` tags _only_ when the _immediate_ parent is an `li` (compare this to `li h4`). Here's a [complete listing](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference#Combinators) of selector types in CSS.
1. the [css variables](https://developer.mozilla.org/en-US/docs/Web/CSS/--*):

```css
html {
  --basil-green: #88a308;
  --dark-gray: #333333;
  --light-gray: #e4e1d1;
  --light-green: #f5faef;
  --orange: #f90;
  --light-orange: #ebbd4e;
  --red: #f00;
  --max-width: 840px;
  --radius: 4px;
}
```

CSS variables are defined at a high level in the CSS (here the `html` selector is used ) to ensure that all the elements inherit and can use them.

CSS variables are applied as follows:

```css
p {
  color: var(--basil-green);
}
```

Note also: the transition property on the anchors. `transition: color 0.2s linear;` is a shortcut for:

```css
transition-property: color;
transition-duration: 1s;
transition-timing-function: linear;
```

## Images

[Responsive Images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images) work well on devices with different screen sizes and resolutions. Add:

```css
img {
  width: 100%;
}
```

Use `width: 100%` on images and videos to set the width to whatever the image's container's width is. CSS also provides control over [aspect ratio](https://developer.mozilla.org/en-US/docs/Web/CSS/aspect-ratio) which is especially useful for videos.

The problem with our responsive image as currently coded can be observed by [throttling the download speed](https://www.browserstack.com/guide/how-to-perform-network-throttling-in-chrome), turning off Cache in the Network tab of the dev tools and doing a hard refresh by right clicking on the browser's refresh icon.

Note the [Cumulative Layout Shift](https://web.dev/cls/) which occurs.

(Be sure to turn off throttling and enable cache before continuing.)

Replace the lone `img` tag in the HTML with `<figure>` and `<figcaption>` tags:

```html
<figure>
  <img
    src="img/pesto.jpg"
    itemprop="image"
    alt="a bold of pesto with a wooden spoon in it"
  />
  <figcaption>
    Classic, simple basil pesto recipe with fresh basil leaves, pine nuts,
    garlic, Romano or Parmesan cheese, extra virgin olive oil, and salt and
    pepper.
  </figcaption>
</figure>
```

The `<figure>` tag allows an optional `<figcaption>` element.

## Aside - Image Optimization

We want to display identical image content across devices but the dimensions should be larger or smaller depending on the device - e.g. a smaller image for mobile users. While the standard `<img>` element points the browser to a single source file, two attributes — `srcset` and `sizes` — provide additional source images along with hints to help the browser pick the right one.

[srcset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/srcset) defines a set of images that allow the browser to choose which image size to use.

Examine image use in a typical [NY Times article](https://www.nytimes.com/2023/06/04/nyregion/brooklyn-brownsville-no-police.html):

```html
<img
  alt="A man in a purple hoodie talks to a group of teenagers. "
  class="css-jb88yk"
  src="https://static01.nyt.com/images/2023/06/01/multimedia/00ny-no-cops90-vkgl/00ny-no-cops90-vkgl-articleLarge.jpg?quality=75&amp;auto=webp&amp;disable=upscale"
  srcset="
    https://static01.nyt.com/images/2023/06/01/multimedia/00ny-no-cops90-vkgl/00ny-no-cops90-vkgl-articleLarge.jpg?quality=75&amp;auto=webp  600w,
    https://static01.nyt.com/images/2023/06/01/multimedia/00ny-no-cops90-vkgl/00ny-no-cops90-vkgl-jumbo.jpg?quality=75&amp;auto=webp        1024w,
    https://static01.nyt.com/images/2023/06/01/multimedia/00ny-no-cops90-vkgl/00ny-no-cops90-vkgl-superJumbo.jpg?quality=75&amp;auto=webp   2048w
  "
  sizes="100vw"
  decoding="async"
  width="600"
  height="400"
/>
```

Note the width and height attributes to prevent layout shift.

Replace the `img` tag in `index.html` with:

```html
<img
  alt="a bowl of pesto with a wooden spoon in it"
  src="img/pesto.jpg"
  srcset="
    img/pesto/pesto_iodywc_c_scale,w_380.jpg   600w,
    img/pesto/pesto_iodywc_c_scale,w_780.jpg  1024w,
    img/pesto/pesto_iodywc_c_scale,w_1380.jpg 2048w
  "
  width="100%"
  height="auto"
/>
```

Hard reload the page at various widths and look in the Sources panel of the developer tools to see the image that was chosen by the browser.

---

Note: you might also see a [picture tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture) used on occasion:

```html
<picture>
  <source
    srcset="img/pesto/pesto_iodywc_c_scale,w_1380.jpg"
    media="(min-width: 1200px)"
  />
  <source
    srcset="img/pesto/pesto_iodywc_c_scale,w_780.jpg"
    media="(min-width: 800px)"
  />
  <img itemprop="image" src="img/pesto.jpg" />
</picture>
```

---

The `<picture>` tag is used for cropping or modifying images for different media conditions _or_ offering different image formats when certain formats are not supported by all browsers. See the [example](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture) on MDN.

The browser ignores everything after the first matching condition, so be careful how you order the media conditions.

Using either technique can save a lot of bandwidth. Older browsers that don't support these features will just ignore them and load the image referenced in the `src` attribute.

You typically automate the process using software such as [Sharp](https://www.npmjs.com/package/sharp) to create multiple image sizes and formats. The HTML is often [automatically generated](https://www.npmjs.com/package/gatsby-plugin-image) as well.

To experiment with these techniques I uploaded `pesto.jpg` to [responsivebreakpoints.com](https://www.responsivebreakpoints.com/), downloaded the zip file and place the unzipped folder in the `img` directory.

There are also specialized image only hosting services such as [Cloudinary](https://cloudinary.com/) which perform image processing.

Demo: samples of [Cloudinary](https://cloudinary.com/) image processing:

```html
<img
  src="https://res.cloudinary.com/deedee/image/upload/w_200,h_200,c_crop/v1623521871/samples/pesto.jpg"
  alt="basil"
/>

<img
  src="https://res.cloudinary.com/deedee/image/upload/w_200,e_grayscale/v1623521871/samples/pesto.jpg"
  alt="basil"
/>
```

The techniques above are used primarily on high traffic websites. For smaller sites just run your images through a processor such as [imageOptim](https://imageoptim.com/mac) before deploying them on your site.

## Flex Layout

The two column view applies only to widescreen.

We will make the article and aside run side by side by applying flex to their parent container within a mobile first breakpoint:

```css
@media (width > 640px) {
  .content {
    display: flex;
  }
}
```

Note: we _cannot_ use a CSS variable as a breakpoint, i.e.:

```css
@media (min-width: var(--breakpoint)) {
  /* ... */
}
```

A media query is not an element selector so it does not inherit.

We can use the flex property on the flex children to manipulate the columns:

```css
@media (width > 640px) {
  .content {
    display: flex;
  }
  article {
    flex: 1 0 60%;
  }
}
```

The [flex property](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) is used on flex children only. We are using a shortcut here which includes `flex-grow, flex-shrink, and flex-basis`. The default is `0 1 auto`.

Here is the long form:

```css
article {
  flex-grow: 1;
  flex-shrink: 0;
  flex-basis: 60%;
}
```

Use a background color and box-shadow to color the aside:

```css
@media (width > 640px) {
  /* ommited for brevity */
  aside {
    background: var(--light-green);
    box-shadow: -4px 0px 4px #ddd;
  }
}
```

Add a variable for light green:

```css
--light-green: #fafdeb;
```

Add some padding to the two columns for both large and small screens:

```css
article,
aside {
  padding: 1rem;
}
```

Format the footer;

```css
footer {
  background-color: var(--basil-green);
  padding: 1rem;
  border-radius: 0 0 4px 4px;
  margin-bottom: 2rem;
}
```

## The Branding Header

Add the green background to the branding div.

```css
header {
  height: 120px;
  background: var(--basil-green);
  border-radius: 8px 8px 0px 0px;
}
```

Note: this is one of the rare occasions that we use the height property. We can use it here because the header does not contain dynamic content.

### Custom Fonts

Add a custom font (top of the css file). Copy the `@font-face` CSS from `font/stylesheet.css` into the top of `styles.css` and correct the file paths:

```css
@font-face {
  font-family: "futura_stdlight";
  src:
    url("font/futurastd-light-webfont-webfont.woff2") format("woff2"),
    url("font/futurastd-light-webfont-webfont.woff") format("woff");
  font-weight: normal;
  font-style: normal;
}
```

Note - To convert fonts to web formats see [Font Squirrel](https://www.fontsquirrel.com/tools/webfont-generator).

```css
header h1 {
  background: url(img/basil.png) no-repeat;
  font-family: futura_stdlight, sans-serif;
  font-weight: normal;
  color: #fff;
  font-size: 5rem;
}
```

Note: `font-weight: normal;` is nice to have here because by default header tags are bold and we do not have a bold version of the futura font available.

Try: temporarily remove `no-repeat` from the `background` property and setting the height of the `<h1>` to 600px.

The background image is 272px by 170px. Recall the [final design goal](other/FINAL.png).

Since background images fill the background of their container we can begin by manipulating padding:

```css
header h1 {
  /* omitted */
  padding-left: 260px;
  padding-top: 90px;
}
```

(We cannot see the text because it is white and we have added padding.)

Use transform to tweak the positioning:

```css
header h1 {
  /* omitted */
  transform: translate(-100px, -80px);
}
```

Note: [transforms](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transforms) are a useful property, especially when it comes to creating animations.

## CSS Nesting

In order to simplify your CSS and maintain order you can nest one CSS rule inside another:

```css
header {
  height: 120px;
  background: var(--basil-green);
  border-radius: 8px 8px 0px 0px;
  h1 {
    background: url(img/basil.png) no-repeat;
    font-family: futura_stdlight, sans-serif;
    font-weight: normal;
    color: #fff;
    font-size: 5rem;
    padding-left: 260px;
    padding-top: 90px;
    transform: translate(-100px, -80px);
  }
}
```

Note the beta link in the header:

```html
<header>
  <h1>Basilica!</h1>
  <a class="beta" href="#">Beta</a>
</header>
```

Absolutely position the beta element:

```css
header a.beta {
  background: url("img/burst.svg") no-repeat;
  color: #fff;
  font-size: 1.5rem;
  position: absolute;
  top: -20px;
  right: 10px;
  width: 85px;
  height: 85px;
  line-height: 85px;
  text-align: center;
  text-transform: uppercase;
}
```

We can use flex in order to align the text vertically and horizontally:

```css
header a.beta {
  background: url("img/burst.svg") no-repeat;
  color: #fff;
  font-size: 1.5rem;
  position: absolute;
  top: -20px;
  right: 10px;
  width: 85px;
  height: 85px;
  /* line-height: 85px; */
  /* text-align: center; */
  text-transform: uppercase;
  display: flex;
  justify-content: center;
  align-items: center;
}
```

Note:

- the use of `img/burst.svg` for the background image. Examine it in the editor.
- the use of position absolute. We will give this element a positioning context by applying position absolute to its containing element:

```css
header {
  /* omitted */
  position: relative;
}
```

Add a hover, transform and animate:

```css
header a.beta {
  /* omitted */
  transform: rotate(20deg);
  transition: all 1s ease;
}
```

Create a hover state for the burst:

```css
header a.beta:hover {
  transform: rotate(0deg) scale(1.2);
}
```

Finally, nest the beta element inside the header CSS:

```css
header {
  height: 120px;
  background: var(--basil-green);
  border-radius: 8px 8px 0px 0px;
  position: relative;
  h1 {
    background: url(img/basil.png) no-repeat;
    font-family: futura_stdlight, sans-serif;
    font-weight: normal;
    color: #fff;
    font-size: 5rem;
    padding-left: 260px;
    padding-top: 90px;
    transform: translate(-100px, -80px);
  }
  a.beta {
    background: url("img/burst.svg") no-repeat;
    color: #fff;
    font-size: 1.5rem;
    position: absolute;
    top: -20px;
    right: 10px;
    width: 85px;
    height: 85px;
    text-transform: uppercase;
    display: flex;
    justify-content: center;
    align-items: center;
    transform: rotate(20deg);
    transition: all 1s ease;
    &:hover {
      transform: rotate(0deg) scale(1.2);
    }
  }
}
```

Note the use of the ampersand (&) for the nested hover.

### Header: Responsive Design

Examine the page for problems in a narrow browser.

We will attempt a mobile first design strategy. Edit the css to display for small screen first:

```css
header {
  /* omitted */
  h1 {
    background: url(img/basil.png) no-repeat;
    font-family: futura_stdlight, sans-serif;
    font-weight: normal;
    color: #fff;
    font-size: 5rem;
  }
```

And add features for the large screen within the media query:

```css
header {
  /* omitted */
  h1 {
    background: url(img/basil.png) no-repeat;
    font-family: futura_stdlight, sans-serif;
    font-weight: normal;
    color: #fff;
    font-size: 5rem;
    @media (width > 640px) {
      padding-left: 240px;
      padding-top: 90px;
      transform: translate(-100px, -80px);
      background-position: top left;
    }
  }
}
```

Additional tweaks for the small screen:

- Remove the body margin top:

```css
body {
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  line-height: 1.5;
  color: var(--dark-gray);
  max-width: var(--max-width);
  /* margin: 0 auto;
  margin-top: 24px; */
}
```

and add it back for the wide screen:

```css
@media (min-width: 640px) {
  margin: 0 auto;
  margin-top: 24px;
}
```

- Remove the rounded corners on small screen:

```css
header {
  /* omitted */
  /* border-radius: 8px 8px 0px 0px; */
}
```

add them back to wide screens:

```css
header {
  height: 120px;
  background: var(--basil-green);
  /* border-radius: 8px 8px 0px 0px; */
  position: relative;
  @media (width > 640px) {
    border-radius: 8px 8px 0px 0px;
  }
  /* omitted */
```

Always remember: there is no hover in touch screen devices. Use the Device Toggle in the developer tools set to a mobile viewport and tap on the burst.

## The Navigation

Add the code below:

```css
nav {
  background: var(--light-gray);
  border-top: 0.5rem solid var(--light-orange);
  padding: 0.5rem;
  display: flex;
  align-items: center;

  ul {
    display: flex;
    gap: 1rem;
  }

  li {
    list-style: none;
    /* margin-right: 0.5rem; */
  }
  p {
    margin-right: auto;
  }
}
```

Note:

- the light gray needs to be lighter: `--light-gray: #e4e1d1;`
- we have two flex children, the `<ul>` and the lone `<p>` tag.
- the margin-right property on the paragraph and the effect it has on the positioning on the navigation links.

Remove it and add `justify-content` to the flex parent:

```css
nav {
  justify-content: space-between;
  flex-wrap: wrap;

  /* p {
  margin-right: auto; 
} */
}
```

Note: the flex-wrap property allows the paragraph to stack on small screens.

### Nav Links and Gradients

```css
nav {
  /* omitted */
  a {
    text-align: center;
    font-size: 1.5rem;
    padding: 8px;
    color: #fff;
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.5);
    border-radius: 6px;
  }
}
```

The [gradients](http://www.colorzilla.com/gradient-editor/) for the buttons:

```css
.nav-storeit a {
  background-image: linear-gradient(to bottom, #fcde41 1%, #dfa910 100%);
}

.nav-storeit a:hover {
  background-image: linear-gradient(to bottom, #dfa910 0%, #fcde41 100%);
}

.nav-pickit a {
  background-image: linear-gradient(to bottom, #abc841 0%, #6b861e 100%);
}

.nav-pickit a:hover {
  background-image: linear-gradient(to bottom, #6b861e 1%, #abc841 100%);
}

.nav-cookit a {
  background-image: linear-gradient(to bottom, #6f89c7 0%, #344e8b 100%);
}

.nav-cookit a:hover {
  background-image: linear-gradient(to bottom, #344e8b 1%, #6f89c7 100%);
}
```

The red color we've chosen for hovers is not visually pleasant here. We will use the CSS pseudo-class [`:not`](https://developer.mozilla.org/en-US/docs/Web/CSS/:not) to exclude the links in the nav bar:

```css
nav {
  /* omitted */
  a {
    /* omitted */
    &:hover {
      color: white;
    }
  }
}
```

Make all the buttons the same width. Try with and without the `inline-block`.

```css
nav {
  /* omitted */
  a {
    /* omitted */
    min-width: 120px;
    display: inline-block;
  }
}
```

Note: this is a setting which will likely need to be changed to accomodate small screens.

## CSS Grid

CSS Tricks offers a [guide to CSS grid](https://css-tricks.com/snippets/css/complete-guide-grid/).

Flexbox operates in a [single dimension](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) using the `flex-direction` property: x or y. CSS Grid operates on both the x _and_ y axis.

Our current use of Flexbox to style the content columns operates in a single (horizontal or x) dimension so flex is really all we need for this layout.

Nevertheless, we will use CSS Grid for the layout in order to introduce some of its features in a simple use case.

Remove the flex statements and use a grid display, define columns, and set the start and end points for the grid children:

```css
@media (width > 640px) {
  .content {
    /* display: flex; */
    display: grid;
    grid-template-columns: 20% 20% 20% 20% 20%;
  }
  article {
    grid-column-start: 1;
    grid-column-end: span 3;
    /* flex: 1 0 60%; */
  }
  aside {
    grid-column-start: 4;
    grid-column-end: span 2;
    background: var(--light-green);
    box-shadow: -4px 0px 4px #ddd;
  }
}
```

`grid-template-columns` can also be expressed as a series of fractions:

`grid-template-columns: 1fr 1fr 1fr 1fr 1fr;`

Or using shorthand:

`grid-template-columns: repeat(5, 1fr);`

By moving the `display: grid` setting to the body selector, we can use [grid areas](https://developer.mozilla.org/en-US/docs/Web/CSS/grid-template-areas) to define our layout:

```css
@media (width > 600px) {
  body {
    display: grid;
    grid-template-areas:
      "header"
      "nav"
      "content"
      "footer";
  }
  header {
    grid-area: header;
  }
  nav {
    grid-area: nav;
  }
  .content {
    grid-area: content;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-column-gap: 1rem;
  }
  article {
    grid-column: span 3;
  }
  aside {
    grid-column: span 2;
    background: var(--light-green);
    box-shadow: -4px 0px 4px #ddd;
  }
  footer {
    grid-area: footer;
  }
}
```

Demo: with grid areas it becomes easy to reassign different sections of the page.

```css
header {
  grid-area: footer;
}
```

This can be very useful in conjunction with breakpoints for responsively laying out complex pages.

## JavaScript

Let's ease back into JavaScript with a demonstration and a simple DOM manipulation.

### Aside: Demo Arrays in Node

Review Node:

```sh
$ mkdir node
$ cd node
$ touch basilnode.js
$ npm init -y
$ npm install random-number
```

[Random Number](https://www.npmjs.com/package/random-number) on npmjs.com.

In `basilnode.js`:

```js
const randomNumber = require("random-number");

const randomIndex = randomNumber({
  min: 0,
  max: 4,
  integer: true,
});

console.log(randomIndex);
console.log(typeof randomNumber);
console.log(typeof randomIndex);
```

At the command line:

```sh
$ node basilnode.js
```

Add some additional variables - arrays:

```js
const randomNumber = require("random-number");

const basilChef = ["mama", "papa", "baby"];

function randomItem(array) {
  const randomIndex = randomNumber({
    min: 0,
    max: array.length - 1,
    integer: true,
  });
  return array[randomIndex];
}

console.log(basilChef);
console.log(basilChef[0]);
console.log(basilChef.length);
console.log(randomItem(basilChef));
```

```sh
$ node basilnode.js
```

Call the randomItem function from within another function and use a [template string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) to construct a bit of HTML::

```js
const randomNumber = require("random-number");

const basilChef = ["mama", "papa", "baby"];
const basilTexture = ["greasy", "frozen", "spicy"];

function randomItem(array) {
  const randomIndex = randomNumber({
    min: 0,
    max: array.length - 1,
    integer: true,
  });
  return array[randomIndex];
}

function makeBasil() {
  return `<h2>${randomItem(basilChef)}'s ${randomItem(
    basilTexture
  )} basil</h2>`;
}

console.log(makeBasil());
```

Here is the same random number generation above without relying on the installed node module:

```js
function random() {
  const max = 3;
  const randomIndex = Math.floor(Math.random() * max);
  return randomIndex;
}

const basilChef = ["mama", "papa", "baby"];
const basilTexture = ["greasy", "frozen", "spicy"];

function randomItem(array) {
  const randomIndex = random();
  return array[randomIndex];
}

function makeBasil() {
  return `<h2>${randomItem(basilChef)}'s ${randomItem(
    basilTexture
  )} basil</h2>`;
}

console.log(makeBasil());
```

### Add a Script

```html
<script src="js/index.js"></script>
```

We'll do something similar to the node demo above in our app - replacing the recipe title with a random one.

In `index.js`:

Evolve a function that uses JavaScript's built-in [Math methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) to return a random number between zero and two:

```js
function random() {
  const max = 3;
  // const randomIndex = Math.random();
  // const randomIndex = Math.random() * max;
  const randomIndex = Math.floor(Math.random() * max);
  return randomIndex;
}

console.log(random());
```

Now call our random function passing in an array.

```js
const basilChefs = ["mama", "papa", "baby"];

function random(array) {
  const max = array.length;
  const randomIndex = Math.floor(Math.random() * max);
  return array[randomIndex];
}

const recipeName = random(basilChefs);
console.log(recipeName);
```

We used the random number to select a name from the array and return it to the calling function.

Add another variable `basilTexture` and massage the output to product a string:

```js
const basilChefs = ["mama", "papa", "baby"];
const basilTexture = ["greasy", "frozen", "spicy"];

function random(array) {
  const max = array.length;
  const randomIndex = Math.floor(Math.random() * max);
  return array[randomIndex];
}

const recipeName =
  "My " + random(basilChefs) + "'s " + random(basilTexture) + " pesto";
console.log(recipeName);
```

Let's use the return value in our layout:

```js
const titleElement = document.querySelector("h2");
```

Test `titleElement` in the console.

```js
const titleElement = document.querySelector("h2");

const basilChefs = ["mama", "papa", "baby"];
const basilTexture = ["greasy", "frozen", "spicy"];

function random(array) {
  const max = array.length;
  const randomIndex = Math.floor(Math.random() * max);
  return array[randomIndex];
}

const recipeName = `${random(basilChefs)}'s ${random(basilTexture)} pesto`;

titleElement.innerText = recipeName;
```

and add CSS:

```css
article h2 {
  font-size: 2rem;
  text-transform: capitalize;
}
```

## JavaScript Popover

Create a div on the bottom of the html page (but before the script tag).

```html
<div class="modal">
  <h3>Hi! I'm a Modal Window (ʘ‿ʘ)╯</h3>
  <p>Information about the beta program.</p>
</div>
```

Add styling:

```css
.modal {
  box-sizing: border-box;
  max-width: 400px;
  padding: 2rem;
  border-radius: 5px;
  min-height: 200px;
  border: 2px solid var(--orange);
  background: white;
  position: fixed;
  top: calc(50vh - 100px);
  left: calc(50vw - 200px);
  /* display: none; */
}
```

Uncomment `display: none` and add a `open` rule:

```css
.open {
  display: block;
}
```

Test by adding the `open` class to the modal using the dev tool's inspector.

Code the `.beta` button to show the window.

Create a variable for the beta button, attach an event listener to it, and create a function to handle the event.

```js
const modal = document.querySelector(".modal");
const betaButton = document.querySelector(".beta");

function showPopover(event) {
  modal.classList.toggle("open");
  event.preventDefault();
}

betaButton.addEventListener("click", showPopover);
```

Refactor to use event delegation:

```js
const modal = document.querySelector(".modal");
// const betaButton = document.querySelector('.beta');

function showPopover(event) {
  console.log(event.target);
  if (!event.target.matches(".beta")) return;
  modal.classList.toggle("open");
  event.preventDefault();
}

// betaButton.addEventListener('click', showPopover);
document.addEventListener("click", showPopover);
```

## DOM Scripting Methods Used

- Use [querySelector](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector) to find the first matching element on a page `const modal = document.querySelector('.modal');`
- Use [querySelectorAll()](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/querySelectorAll) to find all matching elements on a page
- Use [addEventListener('event', function)](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener), to listen for events on an element. You can find a full list of available events on the [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/Events)
- Use [Functions](https://developer.mozilla.org/en-US/docs/Glossary/Function) to store and execute your commands
- Use [classList](https://plainjs.com/javascript/attributes/adding-removing-and-testing-for-classes-9/) to add, remove, toggle, list and test for classes

### Matches

The `matches()` method lets you check if an element would be selected by a particular selector. It returns true if the element is a match, and false when it’s not. It can be an alternative to using `element.classList.contains('.someclass')`.

```js
// Match by an ID
if (elem.matches("#first-button")) {
  // Do something...
}

// Match by a class
if (elem.matches(".button-submit")) {
  // Do something...
}

// Match by one of several selectors
// Returns true when element contains at least one of the selectors
if (elem.matches(".click-me, .button-submit")) {
  // Do something...
}
```

### Add Another Close Method

Add html to the betainfo:

```html
<div class="modal">
  <h3>Hi! I'm a Modal Window (ʘ‿ʘ)╯</h3>
  <p>Information about the beta program.</p>
  <!-- NEW -->
  <a class="closer" href="#0">✖︎</a>
</div>
```

Style it:

```css
.closer {
  position: absolute;
  top: -10px;
  right: -10px;
  width: 1.5rem;
  height: 1.5rem;
  background: #fff;
  color: var(--orange);
  border: 4px solid var(--orange);
  border-radius: 50%;
  text-align: center;
  line-height: 1.5rem;
  cursor: pointer;
}
```

Extend the showPopover function to include the new element script.

```js
const modal = document.querySelector(".modal");

function showPopover(event) {
  if (!event.target.matches(".beta, .closer")) return;
  modal.classList.toggle("open");
  event.preventDefault();
}

document.addEventListener("click", showPopover);
```

Note: you cannot animate between `display: none` and `display: block`.

### Adding Animation to the Modal

Add a wrapping div - `modal-outer` - around the modal:

```html
<div class="modal-outer">
  <div class="modal">
    <h3>Hi! I'm a Modal Window (ʘ‿ʘ)╯</h3>
    <p>Information about the beta program.</p>
    <a class="closer" href="#0">✖︎</a>
  </div>
</div>
```

Style it:

```css
.modal-outer {
  display: grid;
  justify-content: center;
  align-items: center;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  height: 100vh;
  width: 100vw;
  top: 0;
  left: 0;
  /* Hide this until we need it */
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.2s;
}

.modal-outer.open {
  opacity: 1;
  pointer-events: all;
}
```

Try: changing the opacity and pointer-events properties to 1 and all.

Edit the script to select the outer div and apply the `open` class to it:

```js
const modal = document.querySelector(".modal");
const modalOuter = document.querySelector(".modal-outer");

function showPopover(event) {
  if (!event.target.matches(".beta, .closer")) return;
  modalOuter.classList.toggle("open");
  event.preventDefault();
}

document.addEventListener("click", showPopover);
```

Now the modal wrapper will show when the button is clicked - but the modal will not. (Toggle the opacity on the outer div to see the modal.)

Edit styles for the interior modal:

```css
.modal {
  box-sizing: border-box;
  max-width: 400px;
  padding: 2rem;
  border-radius: 5px;
  min-height: 200px;
  border: 2px solid var(--orange);
  background: white;
  transform: translateY(200%);
  transition: transform 1s;
}
```

Note: we are removing the following properties from the inner modal:

```css
position: fixed;
top: 30%;
left: calc(50% - 150px);
display: none;
```

Remove the generic open CSS rule:

```css
/* .open {
  display: block;
} */
```

and replace it with a transform that selects the modal via the outer modal's class:

```css
.modal-outer.open .modal {
  transform: translateY(0);
}
```

Note that we are no longer using '`display: none` to hide the modal. The inner modal is becoming visible because its container, modal outer, is transitioning opacity.

Edit the script to allow clicking on the overlay to close the modal.

```js
// const modal = document.querySelector('.modal')
const modalOuter = document.querySelector(".modal-outer");

function showPopover(event) {
  if (event.target.matches(".beta")) {
    modalOuter.classList.add("open");
  } else if (event.target.matches(".closer, .modal-outer")) {
    modalOuter.classList.remove("open");
  } else return;
  event.preventDefault();
}

document.addEventListener("click", showPopover);
```

## A Dynamic Popover

We will use the popover for different purposes depending on which element is clicked.

```js
const modalOuter = document.querySelector(".modal-outer");
const modalInner = document.querySelector(".modal");

var betaContent = `
<h3>Oooops!</h3>
<p>Wow! Nothing works!<p>
`;

function showPopover(event) {
  if (event.target.matches(".beta")) {
    modalInner.innerHTML = betaContent;
    modalOuter.classList.add("open");
  } else if (event.target.matches(".closer, .modal-outer")) {
    modalOuter.classList.remove("open");
  } else return;
  event.preventDefault();
}

document.addEventListener("click", showPopover);
```

Let's use our new popover to display a different message when the user clicks on any of the three nav buttons.

```js
const modalOuter = document.querySelector(".modal-outer");
const modalInner = document.querySelector(".modal");

var betaContent = `
<h3>Oooops!</h3>
<p>Wow! Nothing works!<p>
`;
var buttonContent = `
<h2>Coming Soon</h2>
<p>This feature coming soon.<p>
<a class="closer" href="#0">✖︎</a>
`;

function showPopover(event) {
  if (event.target.matches(".beta")) {
    modalInner.innerHTML = betaContent;
    modalOuter.classList.add("open");
  } else if (event.target.closest("nav ul")) {
    modalInner.innerHTML = buttonContent;
    modalOuter.classList.add("open");
  } else if (event.target.matches(".closer, .modal-outer")) {
    modalOuter.classList.remove("open");
  } else return;
  event.preventDefault();
}

document.addEventListener("click", showPopover);
```

Note the use of [closest](https://gomakethings.com/checking-event-target-selectors-with-event-bubbling-in-vanilla-javascript/) above. The `closest()` method looks for the closest matching parent to an element that has a selector that you pass in.

## END of Exercise

## Notes

Template literals allow embedded expressions. You can use multi-line strings and string interpolation features with them. They were called "template strings" in prior editions of the ES2015 specification.

```js
let recipe = `
<figure>
  <picture>
    <img src="img/pesto.jpg" alt="Italian pesto" />
  </picture>

  <figcaption>
    Classic, simple basil pesto recipe with fresh basil leaves, pine
    nuts, garlic, Romano or Parmesan cheese, extra virgin olive oil, and
    salt and pepper.
  </figcaption>
</figure>

<h2 itemprop="name">Pesto</h2>

<p itemprop="description">
  A sauce of crushed basil leaves, pine nuts, garlic, Parmesan cheese,
  and olive oil, typically served with pasta.
</p>

<h3>Directions</h3>

<ol itemprop="recipeInstructions">
  <li>
    Combine the basil, garlic, and pine nuts in a food processor and
    pulse until coarsely chopped. Add 1/2 cup of the oil and process
    until fully incorporated and smooth. Season with salt and pepper.
  </li>
  <li>
    If using immediately, add all the remaining oil and pulse until
    smooth. Transfer the pesto to a large serving bowl and mix in the
    cheese.
  </li>
  <li>
    If freezing, transfer to an air-tight container and drizzle
    remaining oil over the top. Freeze for up to 3 months. Thaw and stir
    in cheese.
  </li>
</ol>

<h3>Ingredients</h3>
<ul>
  <li itemprop="recipeIngredient">2 cups packed fresh basil leaves</li>
  <li itemprop="recipeIngredient">2 cloves garlic</li>
  <li itemprop="recipeIngredient">1/4 cup pine nuts</li>
  <li itemprop="recipeIngredient">2/3 cup extra-virgin olive oil</li>
  <li itemprop="recipeIngredient">
    Kosher salt and freshly ground black pepper, to taste
  </li>
  <li itemprop="recipeIngredient">
    1/2 cup freshly grated Pecorino cheese
  </li>
  <li itemprop="recipeIngredient">
    1 <abbr title="Pounds">lb</abbr> plain pasta
  </li>
</ul>
`;

const article = document.querySelector("article");
article.innerHTML = recipe;
```

## Expressions

Any unit of code that can be evaluated to a value is an expression. Since expressions produce values, they can appear anywhere in a program where JavaScript expects a value.

```js
10 + 13;
"hello" + "world";
```

## Statements

A statement is an instruction to perform a specific action - creating a variable or a function, looping through an array of elements, and evaluating code based on a specific condition.

```js
var total = 0;

function greet(message) {
  console.log(message);
}
```

```js
let dirs = "";

function createDirections() {
  for (let i = 0; i < currRecipe.directions.length; i++) {
    dirs += "<li>" + currRecipe.directions[i] + "</li>";
  }
}

createDirections();
```

```js
${currRecipe.directions.map(dir => {
  `<li>${dir}</li>`;
})}
```

```js
${currRecipe.directions.map(dir => `<li>${dir}</li>`)}
```

```js
${currRecipe.directions.map(dir => `<li>${dir}</li>`).join('')}
```

ONE

```js
const recipeTitle = recipesData[0].name;
console.log(recipeTitle);
const figure = document.querySelector("h2");
console.log(figure);
figure.innerText = recipeTitle;
```

`<div id="app"></div>`

TWO

```js
const recipe = recipesData[0];
const recipeOne = '<h2 itemprop="name">' + recipe.name + "</h2>";
const app = document.querySelector("#app");
app.innerHTML = recipeOne;
```

`<div id="app"></div>`

THREE

```js
const recipe = recipesData[0];
const recipeOne =
  "<h2>" +
  recipe.name +
  "</h2>" +
  "<figure>" +
  "<picture>" +
  "<img src=img/" +
  recipe.photo +
  ' alt="' +
  recipe.name +
  '" />' +
  "</picture>" +
  "<figcaption>" +
  recipe.description +
  "</figcaption>" +
  "</figure>";

const app = document.querySelector("#app");
app.innerHTML = recipeOne;
```

FOUR

```js
const recipe = recipesData[0];
const recipeOne = `<h2>${recipe.name}</h2>
  <figure >
  <picture>
    <img src="img/${recipe.photo}" alt="${recipe.name}" />
  </picture>
    <figcaption>${recipe.description}</figcaption>
  </figure>`;

console.log(recipeOne);

const app = document.querySelector("#app");
app.innerHTML = recipeOne;
```

Concurrently & npm run all

If you’re not using (or need to support Windows), you can simply add:

```
"start": "npm run server & npm run sass,
```

However, if you do need to support Windows, I recommend installing npm-run-all as a dev dependency with:

```
npm install npm-run-all --save-dev
```

Then you can use the following command instead:

```
"start": "npm-run-all --parallel server sass",

```

With concurrently:

```
"start": "concurrently \"npm run sass\" \"npm run server \" "
"start": "npm run server & npm run sass"
```

```js
  "scripts": {
    "server": "browser-sync app -w ",
    "sass": "sass  scss/styles.scss app/css/styles.css --watch --source-map",
    "start": "concurrently \"npm run sass\" \"npm run server\" "
  },
```

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules
