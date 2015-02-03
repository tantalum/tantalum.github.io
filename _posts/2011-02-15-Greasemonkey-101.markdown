---
layout: post
title: Greasemonkey 101
---

So I'm reading [InfoWorld](http://www.infoworld.com) the other day and on the side of the page ther is that annoying "Share This" toolbar. Now I have AdBlock installed but it skipped this one for some reason and AdBlock can't removes div's AFAIK. The solution I came up with was [Greasemonkey](http://www.greasespot.net/)

After [installing the plugin](https://addons.mozilla.org/en-US/firefox/addon/greasemonkey/) go back to that article you were reading. Right click on the Greasemonkey icon on the right side of Firefox's status bar and choose to add a new script. Fill out the Dialog that pop's up (see screenshot), make sure to give an URL for the namespace, a fake one will do, and to set the include line to "http://www.infoworld.com/*".

![GreaseMonkey](http://farm6.static.flickr.com/5218/5449967798_b57deb0219.jpg)

Click on the Greasemokey dialog box and your editor should pop up. This is where you write the code for your script, which is just plain Javascript. So our goal is to get rid of the annoying toolbar which just happens to have the fixed ID of "floating_tools", making our script REALLY simple. Remember to keep the comments that were originally in the file.

```javascript
// ==UserScript==
// @name           KSTB
// @namespace      http://greasemonkey.circuitsofimagination.com/
// @description    Kill the Share it Tool Bar
// @include        http://www.infoworld.com/*
// ==/UserScript==

var toolbarElement = document.getElementById("floating_tools");
toolbarElement.parentNode.removeChild(toolbarElement);

```

Save the script and close the editor. Refresh the infoworld page and magically no more annoying toolbar. Even better you are now a 1337 c0d3r cuz you writes amazing greasemokey scripties. Congradulations!

For more in-depth info go to the [Greasemokeys Hacks Wiki](http://commons.oreilly.com/wiki/index.php/Greasemonkey_Hacks) which is where I learned to do this from.
