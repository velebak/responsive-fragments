# Responsive Fragments

Using HTML5 `data-attributes` with a device category 'keyword', containing a URL, on (empty) `div` -tags, to have jQuery (Ajax) load in (append) that URL as additional content when CSS changes the `body:after` CSS3 generated `content` value using a MediaQuery.

*This way of loading in additional content is inspired by **Jeremy Keith's Adactio blog article**: [Conditional CSS](http://adactio.com/journal/5429/) (april 27, 2012)*

This is great for creating HTML **prototypes** or even straight in production code.

### Note about CSS MediaQuery breakpoints and (now) populair devices

You really **shouldn't lock your CSS MediaQery breakpoints** to (now) populair (mobile / tablet) width's of devices. Because that is going to change very quickly. Instead you view your web page at different viewport width's and when things start to look weird, at a CSS3 MediaQuery breakpoint and adjust the layout.

## Device categories

But ... you could (doesn't mean you should) create **4 breakpoint 'types'** related to a **device category** for your web page layout. These imaginary device categories can also be used in communication with your client and or project team.

* You start mobile-first, which is using no breakpoint at all, but let's call it a `device-smartphone` category
* Then a `device-tablet` category width comes along when the viewport gets wider
* Then comes the `device-desktop` category width
* And when your lucky, `device-wide` category width is the last one

Again, when you set the CSS3 MediaQuery breakpoints for these device categories (in `px` or preferred in `em`), don't use now populair device width's! The web is fluid, not fixed.

## HTML fragments

In the HTML you create these (empty) `div` -tags with a HTML5 `data-attribute` containing the URL which contains the (small) HTML fragment that you would like to load in when say the viewport is in the `device-tablet` category.

### Tablet device category

`<div data-device-tablet="/fragments/sidebar.html"></div>`

### Desktop device category

`<div data-device-desktop="/fragments/heavy-social-media.html"></div>`

### Wide devices category

`<div data-device-wide="/fragments/big-video.html"></div>`

## CSS sets a hidden device category 'keyword' on the body:after

In the [Adactio article](http://adactio.com/journal/5429/) they set a 'device category keyword' on the `body:after` CSS generated `content` using ofcourse ... CSS3 MediaQueries.

When the viewport width changes, this keyword changes. You start **mobile-first**, but for the record, add the correct device 'keyword' for `device-smartphone`, but it is not going to be used by Javascript.

	@media (max-width: 320px) {
		body:after {
			content: 'device-smartphone';
			display: none;
		}
	}
	
	@media (min-width: 321px) and (max-width: 780px) {
		body:after {
			content: 'device-tablet';
			display: none;
		}
	}
		
	@media (min-width: 781px) and (max-width: 1024px) {
		 body:after {
			content: "device-desktop";
			display: none;
	}	
	
	@media (min-width: 1025px) {
		body:after {
			content: "device-wide";
			display: none;
		}
	}

The above breakpoints are used as an example. Don't set CSS3 MediaQuery breakpoints based on (now) populair (mobile / tablet) devices.

## A Javascript listner picks up the CSS keyword changes

The Adactio article explains how to use Javascript to 'read' this keyword from the `body:after` CSS generated `content`. A jQuery `$(window).resize()` function is attached to listen to changes of the viewport keyword.

When the viewport gets wider, CSS changes the device-category *keyword*, Javascript picks it up and only once loads in or rather `appends` the additional content (using Ajax) based on the HTML `data-device-tablet` URL on the `div` -tag. This can be static HTML or even dynamic HTML generated by say PHP etc.

When the keyword is `device-desktop` or`device-wide`, the Javascript should load in the 'lower' device fragments like `device-tablet` AND `device-desktop` as well.

## CSS hides the content again, based on the same viewport breakpoints

When the viewport becomes smaller you can use the same breakpoints to subsequently use CSS to `hide` these `div` -tags with the specific `data-attribute`. The additional content is already loaded so their is no shame to hide it using this method.

	@media (max-width: 320px) {
		body:after {
			content: "device-smartphone";
			display: none;
		}
		
		body [data-device-tablet],
		body [data-device-desktop],
		body [data-device-wide] {
			display: none;
		}
	}
	
## Browser support

Ofcourse there is the issue of browser support. This technique relies on HTML5 `data-attributes`, CSS3 MediaQueries and a Javascript listner to 'read' the CSS3 generated content on the `body:after` element.

**Modern- and mobile- web browser all support these techniques but older (desktop) web browsers don't.** 

### Fallback

The Javascript should load in all the HTML fragments if the browser doesn't understand the above techniques. In this case the device category for this type of device should be `device-wide`. This mainly concerns **Windows Internet Explorer 8** and lower versions and maybe older mobile browsers.