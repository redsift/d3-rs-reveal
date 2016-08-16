# d3-rs-reveal

[![Circle CI](https://img.shields.io/circleci/project/redsift/d3-rs-reveal.svg?style=flat-square)](https://circleci.com/gh/redsift/d3-rs-reveal)
[![npm](https://img.shields.io/npm/v/@redsift/d3-rs-reveal.svg?style=flat-square)](https://www.npmjs.com/package/@redsift/d3-rs-reveal)
[![MIT](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](https://raw.githubusercontent.com/redsift/d3-rs-reveal/master/LICENSE)

`d3-rs-reveal` generate a SVG based transition to reveal an image. Typically large images take time to download and present leaving the user with an unsatisfying experience. This module used animated SVG filters and preview images to blend the transition between the low resolution preview and the full resolution asset when available.

## Example

[View @redsift/d3-rs-reveal on Codepen](http://codepen.io/rahulpowar/pen/mEoVyz?editors=0010)

You may use your browser's throttling tools and cache to simulate different load and reload scenarios.

## Usage

### Browser

    <script src="//static.redsift.io/reusable/d3-rs-reveal/latest/d3-rs-reveal.umd-es2015.min.js"></script>
    <script>
        var reveal = d3_rs_reveal.html()
                    .placeholder(placeholder)
                    .imgWidth(imgWidth)
                    .imgHeight(imgHeight)
                    .img(img);
        
        d3.select('body').call(reveal);
    </script>

### ES6

    import { reveal } from "@redsift/d3-rs-reveal";
    let eml = reveal.html();
    ...

### Require

    var reveal = require("@redsift/d3-rs-reveal");
    var eml = reveal.html();
    ...

## Creating images

The supplied preview filter was build to hide heavy compression artefacts so the preview can be exported with extreme levels of compression. At these levels, it becomes practical to base64 and inline the image data.

```bash
$ convert photo-src.jpg -resize 256 -blur 1.1 -quality 10 -profile sRGB_v4_ICC_preference.icc -strip photo-dest_0x.jpg
$ base64 photo-dest_0x.jpg
```

Note that the color profile is converted to sRGB before being stripped from the image. A full discussion on image processing is beyond the scope of this module. [gulp-compressedimages](https://github.com/redsift/gulp-compressedimages) provides a tuned pipeline for generating these images in common sizes.

## Checking supported features

Formats such as WEBP can reduce the size of the assets while maintaining quality. Unfortunately testing for browser support for the codec required some work. A helper is exposed on this module `checkSupported`. It returns a promise that can be supplied to the `img` parameter and transformed to the URL of the desired asset. 

```js
import { reveal, checkSupported } from "@redsift/d3-rs-reveal";
let imageReveal = reveal()
                    .placeholder(placeholder)
                    .imgWidth(imgWidth)
                    .imgHeight(imgHeight)
                    .img(checkSupported.then(s => `./hero${s.retina ? '_2x' : ''}.${s.webp ? 'webp' : 'jpg'}`))
```

### Parameters

Property|Description|Transition|Preview
----|-----------|----------|-------
`classed`|*String* SVG custom class|N
