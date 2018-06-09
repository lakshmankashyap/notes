# Performance

## What happens when browsing ?

1.  Make a request to the server
2.  The server returns the HTML file
3.  The browser reads the file and loads the differents files (CSS files `link` tag, images, `scripts`)

## Keys to performance

- Improve the **client side**
  - Critical render path
  - Optimized code
  - PWA
- Improve the transfer of files and requests
  - Minimize files
  - Minimize Delivery
- Improve the backend
  - CDNs
  - Caching
  - Load balancing
  - GZIP
  - DB Scaling

## Network performance

### Honey I shrunk the files

We can minimize texts :

- Use `uglify.js` : removes the whitespace and so some bytes!

We can also minimize images :

- JPG : doesn't really allow transparency images. It's good for complex images with lots of colors.
- GIF : Looks grainy, limited colors : good for small animations.
- PNG : few sets of colors, good for logos.
- SVG : vector graphics, tend to be very simplistic.

Resources :

- https://99designs.com/blog/tips/image-file-types/
- https://pageweight.imgix.com/
- https://www.sitepoint.com/gif-png-jpg-which-one-to-use/

**Facts to remember**

- Transparency : PNG
- Animation : GIF
- Colourful : JPG
- Simple icons, logos, ... : SVG
- Reduce PNG with [TinyPNG](https://tinypng.com)
- Reduce JPEG with [JPEG Optimizer](http://jpeg-optimizer.com/)
- Choose simple illustrations over detailed photographs
- Lower JPEG image quality (30-60%)
- Resize image based on size it will be displayed
- Display different size images for different backgrounds : use mediaqueries for that (`@media screen and (min-width: ...px)`)
- Use CDNs like [imigx](https://www.imgix.com/)
- Remove metadata with a tool like [verexif](https://www.verexif.com/en/)

### The traveling delivery man

We need to limit the number of files we want to deliver

- Get rid of big frameworks like Bootstrap if not really necessary !
- Merge CSS files / JS files
