# Why We Should be Using SVG

I recently stumbled upon [Avdi Grimm's](https://twitter.com/avdi) awesome idea [PairProgramWith.Me](http://www.pairprogramwith.me). The site comes with a crisp, simple badge by [David Browning](http://twoguys.us), that's the perfect type of image to be an SVG: simple, computer-generated; it was very likely made in Illustrator anyway, so why does the site come with a .png version instead of an .svg?

The likely answer is browser support. PNG's had full support since IE6 officially died (it did, right?), and the first SVG specification from the W3C became a recommendation in August 2011--kind of young as far as the web is concerned. PNG's been used fairly widely on the web lately, and for good reason--it supports a similar color range to .jpg as well as transparency (which .jpg doesn't, duh), and file sizes are usually smaller. Add in browser support, and you've got yourself a rather pragmatic option. 

But here's why I think .svg is the way to deploy image files as of 2013:

1) Vector is the format designers have traditionally chosen for print design. It's made to be scaled up--ready for it?--infinitely. We've already begun transitioning to responsive design as credo, and svg should be part of your design process. You don't know who will be viewing your image at what size on what screen when. And it doesn't cost you a thing to have a very largely displayed svg. Which leads us to:

2) It's tiny. A 300 x 300px png file will be smaller by an order of a KB or two, but an svg is an svg is an svg. SVG is just math: something computers are very good at doing. At any size, it's not only just as crisp, it's also just as expensive as it is at any other size. 

3) Browser support. IE8 and down is a no go, so if that's your bread and butter, SVG is not the path for you. Marketshare on IE8 was still considerable as of April 2013--around 10%--but we're quickly headed for a world of widespread SVG acceptance. 

#### The #1 SVG Gotcha:

When exporting your svg image, make sure you select the option "Convert to Outlines." Those familiar with the print world will implicitly understand this one: this option will ensure viewers see the typefaces you intended them to see. 

If you don't convert to outlines, some browsers (Firefox) will attempt to display the font using the user's system fonts. If the user doesn't have the typeface, the browser will fall back on something without your having a say in the matter. Converting to outlines will ask svg to convert the typeface to geometric shapes just like the rest of the image, thus preserving your image across browsers and providing a consistent experience. 

![An svg version of Browning's awesome Pair with me button](http://brettshollenberger.herokuapp.com/assets/pair-original-colors.svg)