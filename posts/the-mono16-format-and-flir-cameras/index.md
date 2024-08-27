<!--
.. title: The Mono16 Format and Flir Cameras
.. slug: the-mono16-format-and-flir-cameras
.. date: 2024-08-27 14:15:36 UTC+02:00
.. tags: computer vision, cameras
.. category: optics
.. link: 
.. description: Flir's Mono16 image format has always been a bit weird.
.. type: text
-->

For a long time I had found the Mono16 image format of Flir's cameras a bit strange. In the lab I have several Flir cameras with 12-bit ADC's, but the images they output in Mono16 would span a range from 0 to around 65535. How does the camera map a 12-bit number to a 16-bit number?

If you search for the Mono16 format you will find that it's a padded format. This means that, in the 12-bit ADC example, 4 bits in each pixel are always 0, and the remaining 12 bits represent the pixel's value. But this should mean that we should get pixel values only between 0 and 2^12 - 1, or 4095. So how is it that we can saturate one of these cameras with values near 65535?

Today it occurred to me that Flir's Mono16 format might not use all the values in the range [0, 65535]. This is indeed the case, as I show below with an image stack that I acquired from one of these cameras:

```python
>>> sorted_unique_pixels = np.unique(images.ravel())
>>> np.unique(np.diff(sorted_unique_pixels))
array([ 16,  32,  48,  64,  96, 144], dtype=uint16)
```

This prints all the possible, unique differences between the sorted and flattened pixel values in my particular image stack. Notice how they are all multiples of 16?

Let's look also at the sorted array of unique values itself:

```python
>>> sorted_unique_pixels
array([ 5808,  5824,  5856, ..., 57312, 57328, 57472], dtype=uint16)
```

There are more than a million pixels in this array, yet they all take values that are integer multiples of 16.

It looks like Flir's Mono16 format rescales the camera's output onto the interval [0, 65535] by introducing "gaps" between the numbers equal to 2^16 - 2^N where N is the bit-depth of the camera's ADC.

But wait just a moment. Above I said that 4 bits in the Mono16 are zero, but I assumed that these were the most significant bits. If the least significant bits are the zero padding, then the allowed pixel values would be, for example, 
`0000 0000 = 0`, `0001 0000 = 16`, `0010 0000 = 32`, `0011 0000 = 48`, etc. (Here I ignored the first 8 bits for clarity.)

So it appears that Flir is indeed padding the 12-bit ADC data with 0's in its Mono16 format. But, somewhat counter-intuitively, *it is the four least significant bits that are the zero padding.* I say this is counter-intuitive because I have another camera that pads the most significant bits, so that the maximum pixel value is really 2^N - 1, with N being the ADC's bit-depth.
