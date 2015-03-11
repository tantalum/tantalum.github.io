---
layout: post
title: openFrameworks is cool
---

[openFrameworks](http://www.openframeworks.cc/) is a set of C/C++ libraries for "Creative Coding".

I heard about openFrameworks a while back off of reddit. The video on there home page had me sold! So one Saturday morning when I wasn't too hung over I made this.

![OFVideoGrabber](http://farm4.staticflickr.com/3375/3445897429_bbd93b5618_d.jpg)

It only took me 100-150 lines of code to write that app. You can get the Xcode project [here](http://github.com/tantalum/ofvideograbber/tree/master).

<!--more-->

IMO the filter looks best with some kind of light source in the background and an object between it and the camera. It's trippy!

So what really happened was that I was trying to write an edge detector and it went wrong. See code for a link, since I obviously have know idea what I'm doing, I won't try to explain why the filter does what it does. But I thought it was cool so I left it the way it is.

openFrameworks uses a [processing](http://processing.org/) style application loop with a setup(), update(), draw(), and event functions that you hook into by implementing a subclass of ofSimpleApp. My "main" function is only two lines long.

```c
int main (int argc, char * const argv[]) {
 ofSetupOpenGL(1024, 768, OF_WINDOW);
 ofRunApp(new OFVideoGrabberApp());
}
```

My real friend was ofVideoGrabber. Getting the pixels from a built in webcam takes three lines. One in setup():

```c
videoGrabber.initGrabber(camWidth, camHeight);

```

The update function has the other two:

```c
void OFVideoGrabberApp::update(){
 unsigned char *buffer = (unsigned char*)malloc(camWidth*camHeight);
 ofBackground(80, 80, 80);
 videoGrabber.grabFrame();
 if(videoGrabber.isFrameNew()){
  unsigned char* pixels = videoGrabber.getPixels();
  convert_to_greyscale(pixels, camWidth, camHeight, buffer);
  edge_detect(buffer, camWidth, camHeight, filterBuffer);
  filteredTexture.loadData(filterBuffer, camWidth, camHeight, GL_LUMINANCE);
 }
 free(buffer);
}

```

The reason I didn't convert the image to grey scale and do the filtering all at once, instead of looping over the image twice, is that by doing it this way I can easily replace the filter. I just write a new filter and edit a couple lines of code. Of course the proper way to do things would be to dynamically load blah blah blah. For heavens sake it's just a toy!

The same goes for allocating "buffer" every update

I like openFrameworks use of 3rd party libraries and them leaving it out there and on the surface. Most frameworks I've worked wrap the 3rd party API's with there own, or just reinvent the wheel. Which is great as long as you are working within there confines but sucks when you need that one little property that you can't get to.

The part of open frameworks that I like right now but probably wouldn't like so much if I was doing a complete project is the hackish feel that is has. There isn't a nice little config/make/install wrapper, and it _wants_ to hand you the raw pixel data. Which means there is a little less learning overhead and it help free you up to add on what you want quite easily. The flip side to that is there is a lot you have to do from scratch of you want to change/add stuff.

It did take me a little messaging for openFrameworks to compile correctly. It mostly had to do with header paths, due to the lack of a nice config/make wrapper. It also comes with pre-built versions of most of it's libraries but you might have to move some files around to fix link errors. Other then that it went pretty smooth.





