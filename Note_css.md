

# Basic

### Priority

![](https://tva1.sinaimg.cn/large/008i3skNly1grd8hqfiu9j30cg07jab0.jpg)

### Combinator

![](https://tva1.sinaimg.cn/large/008i3skNly1grd9atcs78j60n909vact02.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1grd9bj6bw5j30ne08tgnu.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1grd9cie7wpj30ni08c76j.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNly1grd9cyizi0j30nb08ggnw.jpg)

### Shorthand

![](https://tva1.sinaimg.cn/large/008i3skNly1gre1gogysmj30mn0d5got.jpg)

### Height & Width

​		`div` and `h1` elements are block level elements, always take the full available width by default.

​		unlike inline element like anchor tag `<a>`

##### box-sizing ( margin is never included)

- border-box: includes padding and border

- content-box: if we set a width and height, we set these just for content, not for the entire box.

  ```css
  // to not be overwritten by block level element
  * {
      box-sizing: border-box;
  }
  ```

![](https://tva1.sinaimg.cn/large/008i3skNly1grxt49igjej307h0550sw.jpg)

### Display Property

- change the element level

  ​	ex: for `<a>`, it is inline element, but after `display:block;` it can be display as a block level element.

  ​	but this option is not so useful for us.

- show or hide element:  `display: none;`

  ​	can be used to create modals which only show up on certain action.

- mix the feature of inline and block: `display:inline-block;`

  ​	for inline elements, there is no effects on margin top or bottom, padding. but block level element yes. 

  ​	So `inline-block` makes it possible that elements can go next to each other but they can still behave like block level element to set margin top/bottom, padding.

`display:none` removes the element from the document flow, but it's still part of the DOM.

`visibility:hidden` : the element is only invisible, it's not removed from the document flow and of course also not from the DOM.

### Applying Display Property

To put two `inline-block` elements in one line and the second element is on the right.

```html
<div>
...
</div>
<nav class="main-nav">
...
</nav>
```

```css
div {
    display: inline-block;
}

.main-nav {
    display: inline-block;
    text-align: right;
    width: calc(100% - 49px); /* 49px is width of div */
}
```

​		but we got a problem: two elements are in different lines, cause the whitespace between `div` and `nav` is considered as a character and is added as an extra inline element. We need to minus more as the width of `div`.

​		`width: calc(100% - 54px);`

### Block level

**Block-level elements** are rendered as a block and hence take up all the available horizontal space. You can set margin-top and margin-bottom and two block-level elements will render in two different lines.

Some examples are: `<div>` , `<section>` , `<article>` , `<nav>` but also `<h1>` , `<h2>` etc and `<p>` .

### Inline elements

**Inline elements** on the other hand only take up the space they require to fit their content in. Hence two inline-elements will fit into the same line (as long as the combined content doesn't take up the entire space in which case a line break would be added).

They also use the box-model you learned about but `margin-top` and `margin-bottom` have no effect on the element. `padding-top` and `padding-bottom` also have a different effect.

Additionally, setting a `width` or `height` on an inline element also has no effect. The width and height is auto to take as much space as required by the content.

Some example elements are: `<a>` , `<span>` , `<img>` 

### Pseudo

![](https://tva1.sinaimg.cn/large/008i3skNly1griwlisadtj30mg09h758.jpg)

```css
/* Selects any element that is NOT an active anchor */
a:not(.active) {
	color: blue;
}
```

![](https://tva1.sinaimg.cn/large/008i3skNly1griwu0vmeyj314o0gywh5.jpg)

```css
.main-nav__item a::after {
   content: " (link)";
   color: red;
}
```

<img src="https://tva1.sinaimg.cn/large/008i3skNly1griwusev6yj30e5014aa2.jpg" />



### Initial css

```css
* {
    box-sizing: border-box;
}

body {
    font-family: 'Montserrat', sans-serif;
    margin: 0;
}
```

# Practice

### box-shadow

`h-offset` : Required. The horizontal offset of the shadow. A positive value puts the shadow on the right side of the box, a negative value puts the shadow on the left side of the box

`v-offset` : Required. The vertical offset of the shadow. A positive value puts the shadow below the box, a negative value puts the shadow above the box

`blur` : 模糊度

`spread` : size of the shadow

### Position Property

- `fixed` : the position of the element only depends on the viewport.
- `absolute` : the closest ancestor which has the position property different from `static`  applied is the positioning context for the element, if not, the HTML element is the positioning context.
- `relative` :the positioning context is the element itself, and is not taken out of the document flow.
- `sticky` : combination of `relative` and `fixed`. work with position directly property: `top` etc.

### z-index

Adding z-index to elements which don't have any position property applied or any position property with a value different from `static`, the z-index doesn't have any impact.

One exception from this behaviour is flex-box: Applying the `z-index` to flex-items will change the order of these items even if no `position` property was applied.

### overflow

if we don't want to place the child element outside of the parent element, we could add `overflow:hidden;` to the parent. but if the parent is the body element, this will not work, we should just add `overflow: auto/hidden` to the html element.

# Background & Image

### background-size

- `cover` : ensure the image always fills the entire container, it will even zoom in if the image is smaller than the container.
- `contain` : ensure the entire image but not the entire container.

### background-position

- value with `px` : change the position of the entire image.
- percentage value: modify the excess space of the image.
- `center` : 50% 50%.
- `left top` : 0% 0%, the left edge of the image is positioned on the left edge of the container etc.
- `left 10% bottom 20%` : crop 10% to the left, crop 20% to the bottom.

![](https://tva1.sinaimg.cn/large/008i3skNly1grryxg7iidj30ry0dl0zf.jpg)

### shorthand

background: image position/size repeat origin clip 

```css
background: url("freedom.jpg") left 10% bottom 20%/cover no-repeat border-box;
```

when origin and clip is the same, we could just set one value.

### Image Layout

![](https://tva1.sinaimg.cn/large/008i3skNly1grs0ttkcsxj30hn0bwgxx.jpg)

If you've got an image in a surrounding container and the image of course is an inline element, you can

get rid of that whitespace that is introduced to that inline element behavior by adding `vertical-align: top ` , or `display: block` .

### Linear-gradient

```css
background-image: linear-gradient(to left bottom, red, blue);
```

![](https://tva1.sinaimg.cn/large/008i3skNly1grs13srsj2j30lj0enwks.jpg)

### Radial-gradient

```css
    background-image: radial-gradient(circle at top left, red, blue, green);

```

![](https://tva1.sinaimg.cn/large/008i3skNly1grs1byrkg1j30li0eojyb.jpg)

### Stacking multiple backgrounds

```css
    background: linear-gradient(to top, rgba(80, 60, 18, 0.6) 10%, transparent), url("images/freedom.jpg") left 10% bottom 20%/cover no-repeat border-box, #ff1b68;

```

# Size & Units

![](https://tva1.sinaimg.cn/large/008i3skNly1grt375xbxaj30pb0cb41n.jpg)

- An element which has the **position fixed** declaration applied and a **percentage** unit, the containing block is the viewport.
- An element which has the **position absolute** declaration applied and a **percentage** unit, the containing block is its **ancestor's content plus padding**.
- An element which has the **position static/relative** declaration applied and a **percentage** unit, the containing block is its **ancestor's content ** which is block level element.

![](https://tva1.sinaimg.cn/large/008i3skNly1grtb27pptbj30q20butb1.jpg)

### Hiding Scrollbars on windows machines

Using `vw` on Windows does include the scrollbars: `vw:100` is equal to 100% of the viewport width + scrollbars. On the Mac this is not an issue.

**Solutions:**

​		-- Use `width: 100% ` instead of `vw: 100`

​		-- Add `overflow-x: hidden;` to the `body` selector to hide the horizontal scrollbar (or `overflow-y: hidden;` to hide the vertical scrollbar)

​		-- 

```css
body::-webkit-scrollbar {
	width: 0;
}
```

Summary:

https://web.archive.org/web/20180505112131/https://blogs.msdn.microsoft.com/kurlak/2013/11/03/hiding-vertical-scrollbars-with-pure-css-in-chrome-ie-6-firefox-opera-and-safari/

### Center Element

`margin: auto;` only works for block level elements with an explicitly assigned width.

![](https://tva1.sinaimg.cn/large/008i3skNly1gruh3f989zj30qx0enguw.jpg)

# JavaScript & CSS

### Select Element

We can select elements with JS by tag, by Id, by attribute or by class.

```js
//select the first element with class backdrop
var backdrop = document.querySelector('.backdrop');
//return an array with class backdrop
var backdrop = document.querySelectorAll('.backdrop');

// return an object notation
console.dir(backdrop)
```

### Manipulate Element

```js
function closeModal() {
    modal.style.display = 'none';
    backdrop.style.display = 'none';
}

button_no.addEventListener('click', closeModal);

backdrop.addEventListener('click', function() {
    mobileNav.style.display = 'none';
    closeModal();
});
```

### Manipulate Classes

```js
mobileNav.classList.add('open');
mobileNav.classList.remove('open');
//add or remove automatically
mobileNav.classList.toggle('open');
```

### Property Notation

two ways to get property notation:

​	-- `backdrop.style.backgroundImage`

​	-- `backdrop.style['background-image']`

# Responsive

(iphone 8)

hardware:  width: 750px, height: 1334px

CSS: width: 375px, height: 667 px

pixel ratio: 2

To tell the browser that it should apply this pixel ratio to make sure that it translate the actual hardware pixels into CSS pixels, we simple have to add this viewport `meta` tag.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### mobile first work

​		media queries

```css
@media (min-width: 640px) and (orientation: landscape)  {
  #product-overview h1 {
    font-size: 3rem;
  }
}
```

landscape: 横屏

portrait： 竖屏

### Breaking Points

mobile: ~ 767px

tablet: 768 ~ 1000px

desktop: 1000 ~ px



# Forms

### Advanced Attribute Selectors

![](https://tva1.sinaimg.cn/large/008i3skNly1gryvd9k4ylj30m80cwn3x.jpg)

```css
.signup-form input[id*="terms"],
.signup-form input[id*="terms"] + label{
    display: inline-block;
    width: auto;
}
```

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gryvtint2uj30ja054t9j.jpg" style="zoom:50%;" />

### outline

Always comes after the border, we can have both. Outline is added outside of the border. It's not counting towards the box size, neither doing anything on the box shadow.

### restyle focus status

```css
.signup-form input:not([type="checkbox"]):focus,
.signup-form select:focus {
    outline: none;
    background: #d8f3df;
    border-color: #2ddf5c;
}
```

### style checkbox

```css
.signup-form input[type="checkbox"] {
    border: 1px solid #ccc;
    background: white;
    width: 1rem;
    height: 1rem;
    appearance: none; /* to make sure overwrite checkbox */
}

.signup-form input[type="checkbox"]:checked {
    background: #2ddf5c;
    border: 1px solid #0e4f1f;
}
```

### invalid

```css
.signup-form input.invalid,
.signup-form select.invalid,
.signup-form :invalid {
    border-color: red !important;
    background: #faacac;
}
```

### disabled button

```css
.button[disabled] {
  cursor: not-allowed;
  border: #a1a1a1;
  background: #ccc;
  color: #a1a1a1;
}
```

# Text & Font

### Importing Google Fonts

In `css` file

```css
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100&display=swap');
```

### Importing Custom Fonts (download file .ttf)

```css
@font-face {
  font-family: "Anonymous";
  src: url("anonymous.ttf") format("truetype");
  /*
  src: url("anonymous.woff2") format("woff2");
  src: url("anonymous.woff") format("woff");
  */
}
```

### font-variant

small-caps --> all capitalize

### text-shadow

`text-shadow` : 2px  2px 7px rgb(221, 128, 128);

### shorthand

font:  font-style  font-variant  font-weight  font-size* /line-height  font-family*   



# Flexbox

![](https://tva1.sinaimg.cn/large/008i3skNly1gs5sj3icyrj30q60e4q5w.jpg)

### Container

#### display

​		all elements not defining height will adapt to the maximum height defined.

- `flex` : items are displayed in one row, their widths depends on the parent's width.
- `inline-flex` : the parent element can't change items' width if they are predefined.

#### flex-direction

![](https://tva1.sinaimg.cn/large/008i3skNly1gs5tz9n2x8j30n50dm0uw.jpg)

#### flex-wrap

- `nowrap`

  ![](https://tva1.sinaimg.cn/large/008i3skNly1gs5thxbsmcj60gk06474j02.jpg)

- `wrap`

  the maximum height only works for its row.

  ![](https://tva1.sinaimg.cn/large/008i3skNly1gs5tj5nkioj30go06iq35.jpg)

- `wrap-reverse`

  ![](https://tva1.sinaimg.cn/large/008i3skNly1gs5tki0lhuj30j5077glw.jpg)

#### shorthand

​	flex-flow: flex-direction flex-wrap;

#### align-items

- `stretch` : 可伸缩的

- `center` : center flex items along the **cross axis**.

  ![](https://tva1.sinaimg.cn/large/008i3skNly1gs5u9ytzopj60nn04uweo02.jpg)

- `flex-start` (along the cross axis, same as below)

- `flex-end`

- `baseline` : 基线，文字处于同一水平线

  ![](https://tva1.sinaimg.cn/large/008i3skNly1gs5uoewydaj30lu07oq38.jpg)

#### justify-content

- `center` : center flex items along the **main axis**.

  ![](https://tva1.sinaimg.cn/large/008i3skNly1gs5uff89naj30nl0az74l.jpg)

- `flex-start`

- `flex-end`

#### align-content

​	combination of `align-items` and `justify-content` , align items along the cross axis. As soon as we have our second line available, the `align-content` will come into play.

```css
 /* without align-content */
 align-items: center;
 justify-content: center;
```

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gs5v3hpvcij30gq0b3mxf.jpg" style="zoom:50%;" />

- `center`

  <img src="https://tva1.sinaimg.cn/large/008i3skNly1gs5v4bnhjbj30gm0b1jrn.jpg" style="zoom:50%;" />

- `flex-start`

  <img src="https://tva1.sinaimg.cn/large/008i3skNly1gs5v53gmw5j30go0b2q37.jpg" style="zoom:50%;" />

- `space-between`

  <img src="https://tva1.sinaimg.cn/large/008i3skNly1gs5v66r3tnj30gp0b1dg3.jpg" style="zoom:50%;" />

  

### Items

#### order

its value can be -1, 0, 1, defines the order of elements displaying

#### align-self

align a single item along the cross axis.

![](https://tva1.sinaimg.cn/large/008i3skNly1gs62jlvyn9j30py0b474m.jpg)

#### flex-grow

`flex-grow: 1;` make it possible to increase the width when the parent element increase width.

```css
.item-1 {
	flex-grow: 1;
}
.item-2 {
	flex-grow: 4;
}
```

if flex container increase width for 500px, thus item-1 increases 100px and item-2 increases 400px;

if `flex-wrap` is wrap applied, when the element is wrapped into the second line,

this element occupies the entire space of this second line.

![](https://tva1.sinaimg.cn/large/008i3skNly1gs663mn3a4j60mv0azglz02.jpg)

#### flex-shrink

`flex-shrink: 1;(defaul value)` make it possible to decrease the width when the parent element decrease width.

same logic as `flex-grow` .

#### flex-basis

if we define `flex-basis` to some value, it will overwrite the length along the main axis. For example, 

```css
/* container */
flex-direction: row;
/* item */
flex-shrink: 0; /* important */
width: 350px;
flex-basis: 300px;
```

when we modify the width of container, this element's width remains at 300px.

#### shorthand

flex: flex-grow  flex-shrink  flex-basis;

default: 

 `flex:0 1 auto; `

# CSS Grid

`display: grid;`

A grid element will create new rows dynamically for its direct child elements.

### Defining Columns & Rows

- `grid-template-columns: 300px 200px 20%;` gives 3 columns.

  `grid-template-columns: 300px 4fr 20% 1fr;`

- the unit `fraction(fr)` occupy the remaining space which is 

  100% - 300px - 20% pixels.

  if container's width is 1000px, each column is 300px, 400px,  200px, 100px.

- `grid-template-rows: 5rem 2.5rem;`

### Position Child elements by line number

![](https://tva1.sinaimg.cn/large/008i3skNly1gs70dtna00j30f405r3z8.jpg)

```css
grid-column-start: 3;
grid-column-end: 5;
```

![](https://tva1.sinaimg.cn/large/008i3skNly1gs70g527ljj30f2064q3p.jpg)

### repeat

when we want to create several columns with same width,

`grid-template-columns: repeat(4, 25%);`

### minmax

`grid-template-columns: 5rem minmax(10px, 200px);`

`minmax(10px, auto)` means it won't be smaller than 10px, but will fill up as it can.

### Advanced Element Positioning

```css
grid-column-start: 3;
grid-column-end: span 2; /* span 2 cells*/

/* equals to */

grid-column-start: 3;
grid-column-end: 5;
```

```css
/* occupy the entire row */
grid-column-start: 1;
grid-column-end: -1; /* we can also set -2 to reach the last-second column*/
```

grid elements can overlap if we explicitly set the column and row. Initially, grid will always try to get rid of overlapping.

![](https://tva1.sinaimg.cn/large/008i3skNly1gs714h6n83j30et0apgmn.jpg)

### Named Line

`grid-template-columns: [row-1-start] 5rem [row-1-end row-2-start] minmax(10px, 200px) [row-2-end];`

!! we can't set `span` as name.

`grid-column-start: row-1-start;`

### gaps (in container)

`grid-column-gap: 20px;`

`grid-row-gap: 10px;`

### shorthand

`grid-column: grid-column-start / grid-column-end;`

`grid-row: grid-row-start / grid-row-end;`

`grid-area: grid-row-start / grid-column-start / grid-row-end / grid-column-end;`

`grid-gap: grid-row-gap  grid-column-gap;`

### Named Template Area

We have 4 columns ans 3 rows.

```css
/* in container */
grid-template-areas: "header header header header"
										 "side . main main"
										 "footer footer footer footer";
/* dot means keep empty*/
/* element */
grid-area: header;
```

### fit-content

​		if there are differences of width or height between pc version and mobile version, we can use function `fit-content` to adapt the content.

```css
grid-template-rows: 3.5rem auto fit-content(8rem);
```

### Positioning Grid Element (in container)

`justify-items` : align items horizontally 

`align-items` : align items vertically

- `stretch`
- `start`
- `end`
- `center`

### Postioning the Entire Grid

when grid is smaller than viewport, we can change its position by `justify-content`(x-axis) and `align-content` (y-axis).

### Positioning Elements Individually

`justify-self`

`align-self`

### Autoflow

`grid-auto-rows` : defines the height of rows which are created dynamically.

```css
grid-auto-rows: minmax(8rem, auto);
```

`grid-auto-columns` has same logic.

`grid-auto-flow` : defines the direction of creating new elements. (`row` or `column`)

### Auto-fill

```css
grid-template-columns: repeat(auto-fill, 10rem);
grid-auto-flow: row;
```

this means new element will be created along columns until the entire row is filled, and then grid create a new row.

![](https://tva1.sinaimg.cn/large/008i3skNly1gs783sv2yoj30ln0cntbl.jpg)

### Auto-fit

```css
grid-template-columns: repeat(auto-fit, 10rem);
```

if we don't have enough items for an entire row, `auto-fit` centers items automatically.

![](https://tva1.sinaimg.cn/large/008i3skNly1gs787jizcij30nw0afgni.jpg)

### Creating A Dense Grid

![](https://tva1.sinaimg.cn/large/008i3skNly1gs78cl6jfvj30k60i0gol.jpg)

we may come across this situation, and we want other elements could jump into the empty case.

` grid-auto-flow: row dense;`

![](https://tva1.sinaimg.cn/large/008i3skNgy1gs78eb6xz6j30jg0hv77b.jpg)

### Summary

![](https://tva1.sinaimg.cn/large/008i3skNly1gs79tjioxkj30nw0dagu9.jpg)

# Transformation

### Rotating Elements

```css
transform: rotateZ(45deg);
```

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gs7a4zdwk4j30he06ajul.jpg" style="zoom:50%;" />

### Setting transform-origin

```css
transform-origin: left top;
```

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gs7a5wt8vcj30gl069jts.jpg" style="zoom:50%;" />

### Using Rotate with Translate

```css
transform: rotateZ(45deg) translateX(10rem);
```

`translateX()` makes the element move on the x-axis, and same logic as `translateY()`.

but here, we rotated the element, the x-axis isn't a normal horizontal line.

![](https://tva1.sinaimg.cn/large/008i3skNly1gs7acfq8dbj306s05awg8.jpg)

work with `overflow:hidden;` we can get

![](https://tva1.sinaimg.cn/large/008i3skNly1gs7aibxf9ij307s05o0u2.jpg)

### skew

skews 拉扯 elements on the x-axis or y-axis.

`skewX(20deg)`

![](https://tva1.sinaimg.cn/large/008i3skNly1gs7amiqpjhj30g109ygse.jpg)

### scale

缩放

# Transition

Transitions are a kind of built-in animation or transition between 2 status.

```css
/* from */
.modal {
  opacity: 0;
  transform: translateY(-3rem);
  transition: opacity 200ms ease-in 1s, transform 500ms ease-out; 
}

/* to */
.open {
  opacity: 1 !important;
	transform: translateY(0) !important;
}
```

- `opacity 500ms` : 0.5s 过渡时间

  `ease-in` : starts slower and end faster.

  `1s` : start after 1s

- `ease-out` : starts faster and end slower.

- `linear` : keep same speed.

  

If there is changement of `display` and transition at same step, CSS can't kick them both off . CSS need a delay between `display` and `transition` . we can make it with JS.

```js
 backdrop.style.display = "block";
 setTimeout(function() {
    backdrop.classList.add("open");
 }, 10);

// to close
backdrop.classList.remove("open");
setTimeout(function() {
  backdrop.style.display = "none";
}, 200);
```

# Animation

![](https://tva1.sinaimg.cn/large/008i3skNly1gs85nen1oyj30jb0amq3z.jpg)

```css
@keyframes wiggle {
  from {
    transform: rotateZ(0);
  }
  to {
    transform: rotateZ(10deg);
  }
}

.main-nav__item--cta {
  animation: wiggle 200ms 3s 8 alternate-reverse;
  /* animation: wiggle 200ms 3s 8 forwards; */
}
```

- `3s` : start after 3 seconds
- `8` : play the animation 8 times after it starts
- animation-direction
  - `normal` :  The animation is played as normal (forwards). This is default
  - `reverse` : The animation is played in reverse direction (backwards)
  - `alternate` : The animation is played forwards first, then backwards
  - `alternate-reverse` : The animation is played backwards first, then forwards

- `animation-fill-mode` : 
  - `forwards` : end with the final property in the last keyframe (to)
  - `backwards` : start with the first property in the first keyframe(from)
  - `both` : start with the first keyframe and end with the last keyframe.

animation: name duration delay timing-function iteration direction fill-mode play-state;

```css
animation: wiggle 200ms 1s ease-out 8 alternate forwards running;
```

### multiple keyframes

```css
@keyframes wiggle {
  0% {
    transform: rotateZ(0);
  }
  50% {
    transform: rotateZ(-10deg);
  }
  100% {
    transform: rotateZ(10deg);
  }
}
```

### Animation Event Listener 

```js

ctaButton.addEventListener('animationstart', function(event) {
  console.log('Animation start', event);
});

ctaButton.addEventListener('animationend', function(event) {
  console.log('Animation end', event);
});

ctaButton.addEventListener('animationiteration', function(event) {
  console.log('Animation iterates', event);
});
```

# CSS variables

```css
:root {
	--my-color: #fa923f;
}
.element-1 {
  color: var(--my-color);
}
.element-2 {
  color: var(--my-color, #fa923f);
}
```

