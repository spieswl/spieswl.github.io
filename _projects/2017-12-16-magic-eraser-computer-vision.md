---
layout:             project
title:              "Simple Texture Synthesis and Replacement with OpenCV"
date:               "2017-12-16"

description:        "This project involved performing contextual texture replacement on a pre-recorded video file through the use of OpenCV."
keywords:           python, computer vision, opencv, image processing, texture synthesis
tags:               [Python, OpenCV, Computer Vision]

specifics:
    featured:       false
    images:         "magic-eraser-cv"

published:          true

---

As part of our first Computer Vision course at Northwestern, a [fellow colleague, Lauren Hutson](https://github.com/laurenhut/) and I teamed up to create a "Smart" eraser program using **[Python](https://www.python.org/)** and Python bindings in **[OpenCV](https://opencv.org/)**. Our program works by using some simple image processing techniques on each frame of a sampled video feed. For each frame, we identified red foreground handwriting against a background texture of printed words, erase the foreground handwriting based on the position of an 'eraser' cursor (_in this case, a marker held near the top of the frame_), and synthesize a new texture for the erased regions that blends in with the background paper.

The "Smart" eraser program we developed operates on an AVI-format video file, using `OpenCV` video file parsing functionality as seen here:

```python
import cv2
import imageTiling
import numpy as np
import os
import sys

...

# Define the working path for the target video file
vid_path = os.path.dirname(os.path.abspath(__file__)) + "/" + videoFile
vid_capture = cv2.VideoCapture(vid_path)

# Read first frame from the video feed and convert to HSV space
frame_firstImage = vid_capture.read()[1]
frame_firstHSVImage = cv2.cvtColor(frame_firstImage, cv2.COLOR_BGR2HSV)

# Define frame height and width boundaries
frame_height = frame_firstImage.shape[0]  # Nominally 240
frame_width = frame_firstImage.shape[1]   # Nominally 320

# Develop mask (and inverse mask) of red text based on first video frame
mask_Text = developTextMask(frame_firstHSVImage)

# Pass masked image with occluded areas to imageTiling function
frame_preTiledImage = tilingMaskSetup(frame_firstImage, mask_Text)
frame_postTiledImage = imageTiling.process_image(frame_preTiledImage.copy(), frame_height, frame_width, tile_size=22, overlap_width=5)

# Extract the post-tiled image only in the area defined by the text mask
frame_TileMask = cv2.bitwise_and(frame_postTiledImage, frame_postTiledImage, mask=mask_Text)

# ////////////////////  Main image processing loop  ////////////////////
while vid_capture.isOpened():

    # Read all successive frames from the video feed
    vid_frameInBuffer, frame_baseImage = vid_capture.read()

    if vid_frameInBuffer == True:
        # Etc.
    else:
        break

...
```

We made some assumptions at the start of development that greatly simplified our tasks:

1. **_We can identify (and, later, mask) the text by setting threshold values for pixels matching HSV values corresponding with red text._**
2. **_The text being erased does not move throughout the video, so the original mask is valid for the duration of the video._**
3. **_Erasing as little of the background texture as possible, while potentially making our texture synthesis more difficult, is preferable to preserve the integrity of the rest of the video._**

Our solution was split into two components: one component (`magicEraser.py`) handled by myself and one component (`imageTiling.py`) handled by Lauren. My component handled the parsing of the video feed, identifying and masking the foreground text, and passing the "masked" image to Lauren's portion of the project. The following text is one part of the few functions required to identify, mask, and pass on the processed image from any arbitrary frame of the video:

```python
...

def eraseTextWithMask(frame_BGR, frame_HSV, mask, replacement_texture):

    # Define frame height and width boundaries
    frame_height = frame_BGR.shape[0]  # Nominally 240
    frame_width = frame_BGR.shape[1]   # Nominally 320

    # Initialize important mask variables
    frame_wandXCoord = 0
    kernel = np.ones((3,3), np.uint8)

    # HSV boundary ranges for "Red" (NOTE: Slightly different than original mask boundary values)
    mask_redLowerBound = np.array([0,0,0])
    mask_redUpperBound = np.array([20,255,255])

    # Use OpenCV function to generate red image mask
    frame_redMask = cv2.inRange(frame_HSV, mask_redLowerBound, mask_redUpperBound)

    # Erode and dilate the text mask until general enough to cover the marker text
    frame_erodedMask = cv2.erode(frame_redMask, kernel, iterations=1)
    frame_dilatedMask = cv2.dilate(frame_erodedMask, kernel, iterations=5)

    # Identify the location of the cursor based on the x-coordinate of the Red tip
    # The "break" statements are required to eject from the nested "for" loops when finding the first non-zero value
    for j in range(frame_height):
        for k in range(frame_width):
            if frame_dilatedMask[j][k] == 255:
                frame_wandXCoord = k

            if frame_wandXCoord != 0:
                break
            else:
                continue

        if frame_wandXCoord != 0:
            break
        else:
            continue

    # Set up occlusion masks for important areas to ignore as part of the texture tiling setup
    for m in range(frame_height):
        for n in range(frame_width):

            # Replace all text to the right of the marker tip with the white "screen"
            if n >= frame_wandXCoord and mask[m][n] == 255:
                frame_BGR[m][n] = replacement_texture[m][n]

    return frame_BGR

...
```

Some challenges we identified along the way:

1. **_There were regions we absolutely needed to prevent our texture synthesizer from pulling from to ensure that we could blend the new texture with the background texture._**
2. **_The video feed we were given was very noisy. Morphological operations on the image were required to trim out noise and restore key areas for the masking and cursor-search operations._**
3. **_Our initial design only included functionality to develop one synthesized 'tile' that ends up going everywhere. You can clearly see texture similarities in the final product._**

The easiest way to show how our solution progressed is to pull up the input, intermediate results, and final output for an example frame of the video. White regions in the frame represent 'erased' text that needs to be filled in, while blue regions represent areas where we prevented the script from pulling samples from.

<div class="project-image">
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/01_results.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/01_results.png" width="1000">
    </a>
</div>

We ended up with a fairly clean solution, though unsuitable for anything other than this toy problem (_which is fine for the requirements of this project_). Of course, the utility of a program that can do simplistic texture replacement on one single video source is extremely low, but we learned a good deal about video and image processing, using `OpenCV`, and ended up getting some practice in working collaboratively under a time crunch. The project code is [available on GitHub](https://github.com/spieswl/magic-eraser), as well as the documentation and images supporting our result, but beyond some post-presentation clean-up, we probably will not be coming back to work on this toy solution.
