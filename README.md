web-animations-next
===================

Quick Start
---------------

To provide native Chrome Web Animation features (`Element.animate` and Playback Control) in other browsers, use `web-animations.min.js`. To explore all of the proposed Web Animations API, use `web-animations-next.min.js`.

What is Web Animations?
-----------------------

Web Animations is a new JavaScript API for driving animated content on the web. By unifying the animation features of SVG and CSS, Web Animations unlocks features previously only usable declaratively, and exposes powerful, high-performance animation capabilities to developers.

For more details see the [W3C specification](http://w3c.github.io/web-animations/).

Getting Started
---------------

Here's a simple example of an animation that scales and changes the opacity of a `<div>` over 0.5 seconds. The animation alternates producing a pulsing effect.

    <script src="web-animations.min.js"></script>
    <div class="pulse" style="width:150px;">Hello world!</div>
    <script>
        var elem = document.querySelector('.pulse');
        var player = document.timeline.play(new Animation(elem, [
            {opacity: 0.5, transform: "scale(0.5)"},
            {opacity: 1.0, transform: "scale(1)"}
        ], {
            direction: "alternate",
            duration: 500,
            iterations: Infinity
        }));
    </script>

Web Animations supports off-main-thread animations, and also allows procedural generation of animations and fine-grained control of animation playback. See <http://web-animations.github.io> for ideas and inspiration!

What is the polyfill?
---------------------

The polyfill is a JavaScript implementation of the Web Animations API. It works on modern versions of all major browsers. For more details about browser support see <https://www.polymer-project.org/resources/compatibility.html>.

Native Fallback
---------------

When the polyfill runs on a browser that implements Element.animate and AnimationPlayer Playback
Control it will detect and use the underlying native features.

API and Specification Feedback
------------------------------

File an issue on GitHub: <https://github.com/w3c/web-animations/issues/new>.
Alternatively, send an email to <public-fx@w3.org> with subject line “[web-animations]
… message topic …” ([archives](http://lists.w3.org/Archives/Public/public-fx/)).

Polyfill Issues
---------------

Report any issues with this implementation on GitHub: 
<https://github.com/web-animations/web-animations-next/issues/new>.

Different Build Targets
-----------------------

### web-animations.min.js

Contains the features that are implemented natively in Chrome. If you’re not sure what features you
will need, start with this.

### web-animations-next.min.js

Contains all of the features that the polyfill implements.

### web-animations-next-lite.min.js

Provides a powerful set of features with a small code size.

### Build Target Comparison

|                        | web-animations | web-animations-next | web-animations-next-lite |
|------------------------|:--------------:|:-------------------:|:------------------------:|
|Size (gzipped)          | 12.5kb         | 14kb                | 10.5kb                   |
|<div style='width: 180px'>Element.animate</div>|&#10003;|&#10003;|&#10003;|
|<div style='width: 180px'>Timing input (easings, duration, fillMode, etc.) for animations</div>|&#10003;|&#10003;|&#10003;|
|<div style='width: 180px'>Playback control</div>|&#10003;|&#10003;|&#10003;|
|<div style='width: 180px'>Support for animating lengths, transforms and opacity</div>|&#10003;|&#10003;|&#10003;|
|<div style='width: 180px'>Support for Animating other CSS properties</div>|&#10003;|&#10003;|&#10007;|
|<div style='width: 180px'>Matrix fallback for transform animations</div>|&#10003;|&#10003;|&#10007;|
|<div style='width: 180px'>Animation constructor</div>|&#10007;|&#10003;|&#10003;|
|<div style='width: 180px'>Simple Groups</div>|&#10007;|&#10003;|&#10003;|
|<div style='width: 180px'>Custom Effects</div>|&#10007;|&#10003;|&#10003;|
|<div style='width: 180px'>Timing input (easings, duration, fillMode, etc.) for groups</div>|&#10007;|&#10007;*|&#10007;|
|<div style='width: 180px'>Additive animation</div>|&#10007;|&#10007;\*|&#10007;|
|<div style='width: 180px'>Motion path</div>|&#10007;\*|&#10007;\*|&#10007;|
|<div style='width: 180px'>Modifiable animation timing</div>|&#10007;|&#10007;\*|&#10007;\*|
|<div style='width: 180px'>Modifiable group timing</div>|&#10007;|&#10007;\*|&#10007;\*|
|<div style='width: 180px'>Usable inline style**</div>|&#10003;|&#10003;|&#10007;|

\* support is planned for these features.
** see inline style caveat below.

Caveats
-------

Some things won’t ever be faithful to the implementation due to browser and CSS API limitations. These include:

### Inline Style

Inline style modification is the mechanism used by the polyfill to animate properties. Both web-animations and web-animations-next incorporate a module that emulates a vanilla inline style object, so that style modification from JavaScript can still work in the presence of animations. However, to keep the size of web-animations-next-lite as small as possible, the style emulation module is not included. When using this version of the polyfill, JavaScript inline style modification will be overwritten by animations.

### Prefix handling

The polyfill will automatically detect the correctly prefixed name to use when writing animated properties back to the platform. Where possible, the polyfill will only accept unprefixed versions of experimental features. For example:

    var animation = new Animation(elem, {"transform": "translate(100px, 100px)"}, 2000);

will work in all browsers that implement a conforming version of transform, but

    var animation = new Animation(elem, {"-webkit-transform": "translate(100px, 100px)"}, 2000);

will not work anywhere.


Breaking changes
----------------

When we make a potentially breaking change to the polyfill's API surface (like a rename) we'll continue supporting the old version, deprecated, for three months, and ensure that there are console warnings to indicate that a change is pending. After three months, the old version of the API surface (e.g. the old version of a function name) will be removed. If you see deprecation warnings you can't avoid it by not updating.

We also announce anything that isn't a bug fix on [web-animations-changes@googlegroups.com](https://groups.google.com/forum/#!forum/web-animations-changes).


Building
--------

* Install grunt: `npm install -g grunt-cli`
* Install dev dependencies: `npm install`
* Run grunt: `grunt`

Testing
-------

* Open test/runner.html
