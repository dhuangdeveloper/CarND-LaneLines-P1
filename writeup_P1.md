#**Finding Lane Lines on the Road** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

![image1]: ./examples/grayscale.jpg "Grayscale"

---


### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The latest version of my pipline described in this report consisted of 7 steps: 
1. I convert the image to grayscale.
[image1]: ./examples/solidWhiteCurve_grayscale.jpg "Grayscale"
2. I detect the edges using Canny Edge Detector
[image2]: ./examples/solidWhiteCurve_edges.jpg "Edge Detection"
3. I select a region of interest and only consider edges / segments in that region
4. I detect the lines use Hough's Transform
[image3]: ./examples/solidWhiteCurve_detected_lines.jpg "Line Detection"
5. I cluster individual line segments from step 5 based on their closeness in parameter space after Hough transform, I then combine and connect the line segments in the same cluster and obtain one "combined" line segment per cluster.
[image4]: ./examples/solidWhiteCurve_clustered_combined_lines.jpg "Combined Lines from Clustering"

6. Step 5 would lead to multiple long line segments. I then downselect them and obtain one or two lines based on their orientation, closeness to the center of view, and length. 
[image5]: ./examples/solidWhiteCurve_down_selected_and_extended_lines.jpg "Downselected Lines"

7. When drawing the obtained two lines, I extend each of them to the boundary of the region of interest.

In order to draw a single line on the left and right lanes, I modified the replace the draw_lines function with a combination of 
step 5: cluster_and_combine_in_hough
step 6: select_lines
step 7: draw_lines_to_boundary_image


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming is that I am using a pre-selected region of interest. Edge masking, and down selecting lines both depend on this operation.

Another shortcoming could be handling curved lanes, which becomes very important for curved roads. 


###3. Suggest possible improvements to your pipeline

A possible improvement would be to leverage continuity between video frames. In some cases, especially the challenge video, sometimes the lane disappeared completely for a short time interval. For example, instead of detect lanes for each frame, we could use new detected lanes to "adjust" previously detected lanes. If there is no newly detectedly, then we stick with previously detected lanes for a certain perdio. 

Another potential improvement could be to leverage the physical law: for example, other lane lines or boundary lines, or the boundary of fence will not intersect with the current lane, and we can use this rule to exclude some detected lines. 
