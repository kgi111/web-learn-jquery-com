---
chapter : plugins
section : 2
title   : How to create a basic plugin
attribution:  jQuery Fundamentals
---
## How to create a basic plugin

The notation for creating a typical plugin is as follows:

<div class="example" markdown="1">
    (function($){
        $.fn.myNewPlugin = function() {
            return this.each(function(){
                // do something
            });
        };
    }(jQuery));
</div>

Don't let that confuse you though. The point of a jQuery plugin is to extend
jQuery's prototype object, and that's what's happening on this line:

<div class="example" markdown="1">
    $.fn.myNewPlugin = function() { //...
</div>

We wrap this assignment in an immediately-invoked function:

<div class="example" markdown="1">
    (function($){
         //...
    }(jQuery));
</div>

This has the effect of creating a "private" scope that allows us to extend
jQuery using the dollar symbol without having to risk the possibility that the
dollar has been overwritten by another library.

So our actual plugin, thus far, is this:

<div class="example" markdown="1">
    $.fn.myNewPlugin = function() {
        return this.each(function(){
            // do something
        });
    };
</div>

The `this` keyword within the new plugin refers to the jQuery object on which
the plugin is being called.

<div class="example" markdown="1">
    var somejQueryObject = $('#something');

    $.fn.myNewPlugin = function() {
        alert(this === somejQueryObject);
    };

    somejQueryObject.myNewPlugin(); // alerts 'true'
</div>

Your typical jQuery object will contain references to any number of DOM
elements, and that's why jQuery objects are often referred to as collections.

So, to do something with a collection we need to loop through it, which is most
easily achieved using jQuery's `each()` method:

<div class="example" markdown="1">
    $.fn.myNewPlugin = function() {
        return this.each(function(){

        });
    };
</div>

jQuery's `each()` method, like most other jQuery methods, returns a jQuery
object, thus enabling what we've all come to know and love as 'chaining'
(`$(...).css().attr()...`).  We wouldn't want to break this convention so we
return the this object.  Within this loop you can do whatever you want with
each element.  Here's an example of a small plugin using some of the techniques
we've discussed:

<div class="example" markdown="1">
    (function($){
        $.fn.showLinkLocation = function() {
            return this.filter('a').each(function(){
                $(this).append(
                    ' (' + $(this).attr('href') + ')'
                );
            });
        };
    }(jQuery));

    // Usage example:
    $('a').showLinkLocation();
</div>

This handy plugin goes through all anchors in the collection and appends the
href attribute in brackets.

<div class="example" markdown="1">
    &lt;!-- Before plugin is called: -->
    &lt;a href="page.html">Foo&lt;/a>

    &lt;!-- After plugin is called: -->
    &lt;a href="page.html">Foo (page.html)&lt;/a>
</div>

Our plugin can be optimized though:

<div class="example" markdown="1">
    (function($){
        $.fn.showLinkLocation = function() {
            return this.filter('a').append(function(){
                  return ' (' + this.href + ')';
            });
        };
    }(jQuery));
</div>

We're using the `append` method's capability to accept a callback, and the
return value of that callback will determine what is appended to each element
in the collection.  Notice also that we're not using the `attr` method to
retrieve the href attribute, because the native DOM API gives us easy access
with the aptly named href property.

Here's another example of a plugin.  This one doesn't require us to loop
through every element with the `each()` method.  Instead, we're simply going to
delegate to other jQuery methods directly:

<div class="example" markdown="1">
    (function($){
        $.fn.fadeInAndAddClass = function(duration, className) {
            return this.fadeIn(duration, function(){
                $(this).addClass(className);
            });
        };
    }(jQuery));

    // Usage example:
    $('a').fadeInAndAddClass(400, 'finishedFading');
</div>
