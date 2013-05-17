Squeezr
=======
is basically just another approach to improve browsing experience across the ever-growing
multitude of web enabled devices. It can help you preserve your visitor's bandwidth by
***shrinking your website's images and CSS files*** to the device-specific limitations.

Squeezr can easily be applied to any website – be it a collection of plain HTML files or rendered
by a full-blown CMS – and does not require any remarkable change to your source code. As a means of
[responsive web design](http://www.abookapart.com/products/responsive-web-design) it is to be used
in combination with [fluid image](http://unstoppablerobotninja.com/entry/fluid-images/) techniques.

Primary objectives
------------------

*	Squeezer aims to be a ***modern solution*** and doesn't care much about being specifically compatible
	with outdated server environments (see [requirements](#requirements)).
*	It was the overall goal to leverage the ***best possible performance*** in every
	[stage of processing](#functionality). Considerable effort has been made to deliberately choose
	and combine the techniques involved in Squeezr.
	
<a id="functionality"></a>Functionality
---------------------------------------

Currently Squeezr consists of two "engines" that can be used independently of each other:

*	The **[image engine](#images)** automatically resizes images so that they don't exceed your
	visitor's screen size. The resulting image variants get cached to disk for optimized follow-up
	request performance.
*	Similarily, the **[CSS engine](#css)** creates and caches device-specific CSS file variants by
	stripping out irrelevant CSS3 media query sections on the server side. Optionally CSS minification
	can be applied as well, potentially reducing file size even further.

For both engines the processing stages are:

1.	In the client browser, a tiny ***JavaScript*** determines the relevant parameters (screen size,
	pixel density, etc.) and sets a couple of corresponding cookie values to be sent to the
	server along with every following (image or CSS) request. Embedding this little JavaScript into
	the head of your website's pages is the only markup change required for Squeezr to start working.
2.	Based on the cookie values the ***Apache webserver*** redirects any incoming image or CSS
	request to an appropriate cache file. If the cookie values don't exist for some reason
	(e.g. because of the client not supporting JavaScript or cookies), no redirect will occur and
	the originally requested file will be delivered.
3.	In case the requested cache file doesn't exist yet, the ***PHP component*** of Squeezr becomes
	active and creates and caches an appropriate file version, which is finally also delivered to the
	client.


<a id="images"></a>Image engine
-------------------------------
The image engine was heavily inspired and influenced by
[Matt Wilcox' Adaptive Images](http://adaptive-images.com/). However, his approach seemed to have
some drawbacks which I wanted to overcome with Squeezr:

*	In general I wanted to avoid that ***every*** image request gets processed by a PHP script,
	even those ones asking for images that had been cached before. IMHO this smells like an
	unnecessary performance overhead. Why not let Apache do the work instead of delegating
	everything to PHP in such cases?
*	If an image doesn't need to be resized (as it is already small enough), Adaptive Images
	just returns the originally requested file (which is ok so far). However,
	the HTTP headers necessary for leveraging the client browser's cache on subsequent
	re-requests of the same file do not get sent along, which results in the file being loaded
	from the server over and over again as if it had never been processed before.
*	Adaptive Images includes means to partially invalidate the image file cache in case the
	original files are changing. If this feature is enabled, the original file is checked for
	modification ***each time it is requested***. IMHO this rather expensive check must not be
	part of the request process. In an ideal world garbage collection like this only needs to
	be done when the original file really changes. Instead of putting the visitor in charge of
	cleaning the cache I'd rather see the website administrator responsible for this.
	

<a id="css"></a>CSS engine
-------------------------------


Installation
------------


Configuration
-------------


<a id="requirements"></a>Requirements
-------------------------------------

*	Client side
	*	JavaScript support
	*	Cookie support
*	Server side
	*	Apache 2.2+ with mod_rewrite
	*	PHP 5.3+
	*	GD (Image engine only; mostly standard with PHP)

Legal
-----
Squeezr by Joschi Kuphal is licensed under a [Creative Commons Attribution 3.0 Unported
License](http://creativecommons.org/licenses/by/3.0/).