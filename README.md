Parallax.js
===========

Simple parallax scrolling effect inspired by [Spotify.com](http://spotify.com/) implemented as a jQuery plugin
[http://pixelcog.com/parallax.js/](http://pixelcog.com/parallax.js/)

## Installation

Download and include `parallax.min.js` in your document after including jQuery.

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
<script src="/path/to/parallax.min.js"></script>
```

If you will be using the responsive feature of `parallax.js` and do not have [Modernizr.mq](http://modernizr.com/docs/#mq) already included in the document, use the `parallax.mq.js` or `parallax.mq.min.js` version.


## Usage

### Via data attributes

To easily add a parallax effect behind an element, add `data-parallax="scroll"` to the element you want to use, and specify an image with `data-image-src="/path/to/image.jpg"`.

```html
<div class="parallax-window" data-parallax="scroll" data-image-src="/path/to/image.jpg"></div>
```

#### Responsive images

To add different resolutions of the same images (similar to the HTML5 tag [picture](http://www.html5rocks.com/en/tutorials/responsive/picture-element/)), add ```span``` child elements and specify an image with ```data-image-src="/path/to/image.jpg"``` and its corresponding media query with ```data-media-query="<media query>"```. Media queries are tested with [Modernizr.mq](http://modernizr.com/docs/#mq). The media queries work like in CSS. (This feature is not supported in IE 8 and other old browser that do not support media queries.)

```html
<div class="parallax-window" data-parallax="scroll" data-responsive="true" data-image-src="http://placehold.it/320x320&text=default">
  <span data-image-src="http://placehold.it/767x767/" data-media-query="(min-width: 320px)"></span>
  <span data-image-src="http://placehold.it/1024x1024/" data-media-query="(min-width: 768px)"></span>
  <span data-image-src="http://placehold.it/1200x1200/" data-media-query="(min-width: 1024px)"></span>
  <span data-image-src="http://placehold.it/1400x1400/" data-media-query="(min-width: 1200px)"></span>
</div>
```

The '767x767' image will load for widths 320px - 766px because the media query `(min-width: 320px)` matches only for that range until the device-width of 767px and `(min-width: 767px)` becomes true and matches. The '1024x1024' image will be visible until a device-width of 1024px, when `(min-width: 1024px)` matches best and the '1200x1200' image will be used. More explicit media queries can be used too like `(min-width: 320px) and (max-width: 767px)`. In order to set a fallback/default image if no media query matches, set a `data-image-src="/path/to/image.jpg"` on the parallax element `div`. If a fallback/default image is not specified, a 1x1 transparent gif will be used. Or, if the following were to be added as the first span element, before the 320px one, it would show up from 0px - 319px:

```html
<span data-image-src="http://placehold.it/320x320/" data-media-query="(max-width: 320px)"></span>
```

### Via JavaScript

To call the parallax plugin manually, simply select your target element with jQuery and do the following:

```javascript
$('.parallax-window').parallax({imageSrc: '/path/to/image.jpg'});
```

### Notes

What parallax.js will do is create a fixed-position element for each parallax image at the start of the document's body. This mirror element will sit behind the other elements and match the position and dimensions of its target object.

Due to the nature of this implementation, you must ensure that these parallax objects and any layers below them are transparent so that you can see the parallax effect underneath.  Also, if there is no other content in this element, you will need to ensure that it has some fixed dimensions otherwise you won't see anything.

```css
.parallax-window {
  min-height: 400px;
  background: transparent;
}
```

Also, keep in mind that once initialized, the parallax plugin presumes a fixed page layout unless it encounters a `scroll` or `resize` event.  If you have a dynamic page in which another javascript method may alter the DOM, you must manually refresh the parallax effect with the following commands:

```javascript
jQuery(window).trigger('resize.px.parallax').trigger('scroll.px.parallax');
```

## Options

Options can be passed in via data attributes of JavaScript.  For data attributes, append the option name to `data-`, as in `data-image-src=""`.

Note that when specifying these options as html data-attributes, you should convert "camelCased" variable names into "dash-separated" lower-case names (e.g. `zIndex` would be `data-z-index=""`).

<table class="table table-bordered table-striped">
	<thead>
		<tr>
			<th style="width: 100px;">Name</th>
			<th style="width: 100px;">type</th>
			<th style="width: 50px;">default</th>
			<th>description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>imageSrc</td>
			<td>path</td>
			<td>null</td>
			<td>You must provide a path to the image you wish to apply to the parallax effect.</td>
		</tr>
		<tr>
			<td>naturalWidth</td>
			<td>number</td>
			<td>auto</td>
			<td rowspan="2">You can provide the natural width and natural height of an image to speed up loading and reduce error when determining the correct aspect ratio of the image.</td>
		</tr>
		<tr>
			<td>naturalHeight</td>
			<td>number</td>
			<td>auto</td>
		</tr>
		<tr>
			<td>position</td>
			<td>xPos yPos</td>
			<td>center center</td>
			<td rowspan="3">This is analogous to the background-position css property. Specify coordinates as top, bottom, right, left, center, or pixel values (e.g. -10px 0px). The parallax image will be positioned as close to these values as possible while still covering the target element.</td>
		</tr>
		<tr>
			<td>positionX</td>
			<td>xPos</td>
			<td>center</td>
		</tr>
		<tr>
			<td>positionY</td>
			<td>yPos</td>
			<td>center</td>
		</tr>
		<tr>
			<td>speed</td>
			<td>float</td>
			<td>0.2</td>
			<td>The speed at which the parallax effect runs. 0.0 means the image will appear fixed in place, and 1.0 the image will flow at the same speed as the page content.</td>
		</tr>
		<tr>
			<td>zIndex</td>
			<td>number</td>
			<td>-100</td>
			<td>The z-index value of the fixed-position elements.  By default these will be behind everything else on the page.</td>
		</tr>
		<tr>
			<td>bleed</td>
			<td>number</td>
			<td>0</td>
			<td>You can optionally set the parallax mirror element to extend a few pixels above and below the mirrored element.  This can hide slow or stuttering scroll events in certain browsers.</td>
		</tr>
		<tr>
			<td>responsive</td>
			<td>boolean</td>
			<td>false</td>
			<td>If true, this option will look for nested <code>&lt;span&gt;&lt;/span&gt;</code> elements with responsive information. Each nested span should specify an image with <code>data-image-src="/path/to/image.jpg"</code> and its corresponding media query with <code>data-media-query="(max-width: 768px)"</code> – the last matching media query wins. Media queries are tested with <a href="http://modernizr.com/docs/#mq" target="_blank">Modernizr.mq</a>. If set to true and no span children elements are found, and an image is specified on the parallax element <code>&lt;div&gt;&lt;/div&gt;</code> with <code>data-image-src="/path/to/image.jpg"</code> will be used and it will not be responsive. The parallax mirror aspect ratio will be updated as the images are updated.</td>
		</tr>
		<tr>
			<td>iosFix</td>
			<td>boolean</td>
			<td>true</td>
			<td>iOS devices are incompatable with this plugin. If true, this option will set the parallax image as a static, centered background image whenever it detects an iOS user agent. Disable this if you wish to implement your own graceful degradation.</td>
		</tr>
		<tr>
			<td>androidFix</td>
			<td>boolean</td>
			<td>true</td>
			<td>If true, this option will set the parallax image as a static, centered background image whenever it detects an Android user agent. Disable this if you wish to enable the parallax scrolling effect on Android devices.</td>
		</tr>
		<tr>
			<td>overScrollFix</td>
			<td>boolean</td>
			<td>false</td>
			<td>(Experimental) If true, will freeze the parallax effect when "over scrolling" in browsers like Safari to prevent unexpected gaps caused by negative scroll positions.</td>
		</tr>
	</tbody>
</table>

## Contributing

To get started developing parallax.js [Grunt](http://gruntjs.com/getting-started) and its dependencies need to be installed.

Once Grunt is installed, within the project's root directory, install project dependencies with `npm install`.

From there, run `grunt watch` and develop the `lib/parallax.js`. This will generate `lib/parallax.mq.js`.

If you have a pull request you would like to submit, please ensure that you update the minified versions of the library along with your code changes. Run `grunt` to perform code concatenation and compression, essentially updating the minified versions of the parallax.js.


LICENSE
=======

The MIT License (MIT)

Copyright (c) 2015 PixelCog Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
