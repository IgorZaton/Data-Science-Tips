<p align="center">
  <img src="https://github.com/IgorZaton/Data-Science-Tips/assets/50952749/6074bc6a-9749-4cee-83d8-13bf59f1b2dc" width="350" height="350"/>
</p>

## Table of Contents

1. [Computer Vision](#computer-vision)

## Computer Vision

### 1. Be careful with 8-bit images while working with different image modalities

If You're working with different modalities like infrared or X-Ray, you should
know the original color space of the image. In image processing 8-bit
images are the standard one, but in different modalities like infrared or X-Ray
it could be 16-bit or even more. It's possible that the first thing you're going
to do is squeezing it to the 8-bit form. It will make visualization and general
processing much easier, but you can lose much information in that way and make
your goal much harder to achieve.

If for example you'd like to detect some really hot objects on the image,
it's possible it's going to be really easy on 16-bit images, but quite challenging
in the 8-bit color space.
