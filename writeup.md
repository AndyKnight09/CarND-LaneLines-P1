### Self-Driving Car: Project 1

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[//]: # (Image References)

[image1]: ./test_images_output/1_input.jpg "Input"
[image2]: ./test_images_output/2_gray.jpg "Grayscale"
[image3]: ./test_images_output/3_blur_gray.jpg "Gaussian Smoothed"
[image4]: ./test_images_output/4_edges.jpg "Canny Edges"
[image5]: ./test_images_output/5_masked_edges.jpg "Masked Edges"
[image6]: ./test_images_output/6_hough_lines.jpg "Hough Lines"
[image7]: ./test_images_output/7_grouped_lines.jpg "Grouped Lines"
[image8]: ./test_images_output/8_averaged_lines.jpg "Averaged Lines"
[image9]: ./test_images_output/9_output.jpg "Output"

---

### Reflection

![Input image][image1]

**Description of the pipeline** 

The intial pipeline consisted of the following steps:

* Converting the image to grayscale to combine information from all colour channels into a single value for each pixel

![Grayscale conversion][image2]

* Applying Gaussian smoothing to reduce noise levels in the image

![Gaussian smoothed image][image3]

* Applying Canny edge detection to find edges within the smoothed grayscale image

![Canny Edge Detection][image4]

* Applying a region of interest mask to exclude non-road clutter from the detected edges

![Region of Interest Mask][image5]

* Applying a Hough transform of the masked edges to edges that belong to straight lines

![Hough Transform][image6]

Once this was in place we can then use the following steps to create a single pair of left and right lane markings:

* Grouping lines into either left or right lane marking - excluding lines that don't fall within expected orientation limits (e.g. horizontal lines)

![Grouped lines][image7]

* Averaging the coefficients of the grouped lines to return a best estimate for the left and right lane markings

![Averaged lines][image8]

* Overlaying the best estimate for the left and right lane markings over the original image

![Final Output][image9]

**Potential shortcomings with the pipeline**

There are a number of shortcomings with the current pipeline:

* The averaging of lines to form the single lane marking is sucseptible to outliers which can drastically distort the position of the lane marking estimate.
* It relies on lane markings being parralel with the direction of travel. While this is a reasonably safe assumption for lane keeping on a motorway it wouldn't help us to find lane markings when approaching a Stop line for instance.
* The current line detection doesn't work well with yellow line markings on a light road surface, often failing to detect the markings.
* There is no temporal knowledge being used, meaning from one measurement to the next the position of the lane marking can vary wildly. Really the system should have some rejection criteria based on previous measurements - although this would probably live in a tracker the level above this pipeline.


**Possible improvements to the pipeline**

To improve the performance of the pipeline the following changes could be made:

* Once lines have been grouped into left and right we could calculate the median line coefficients and then exclude outliers before calculating the mean line estimate.
* The conversion to grayscale throws away some information. It is possible that we could be more effective by first working out what color the road is and then using a different colour mapping which provides a better contrast between road and line markings. This would help to improve performance for the "Optional Challenge" video.
* Currently the parameters are fixed for all images. it might be possible to change the parameters on the fly to improve performance based on the characteristics of the image e.g. road surface colour, light levels, etc.
* There are image processing techniques to provide a "shadow invariant" image which would help deal with the shadows in the "Optional Challenge" video. See [http://www.robots.ox.ac.uk/~mobile/Papers/2013IROS_corke.pdf] for more details.

