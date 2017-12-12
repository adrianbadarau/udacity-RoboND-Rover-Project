## Project: Search and Sample Return
### Writeup Template: You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---


**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook). 
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands. 
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./sources/1.png
[image2]: ./sources/2.png
[image3]: ./sources/3.png


## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it!

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.
I have run, added and modified functions in the file. My dataset is recorded in folder ./my_data.

The way to select obstacles, rocks and navigable area are as follows:
1. the color_threshold will be (160, 160, 160) as default which is the navigable area. (line 3-15)
2. the obstacle_threshold will be set at (0, 160, 0, 160, 0, 160) which means the whole picture is obstacle excluding the navigable area and the black area, color of the obstacles. (line 17-30)
3. the rock_threshold set to (130, 180, 100,170,0,30).(line 32-44)

As is shown in the pic below, the obstacle is red and the navigable area is green.
![alt text][image1]

#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 

The output video is in  sources directory, named test_mapping.mp4

You can see the navigable terrain and obstacles ploted in the worldmap by the rover as it discovers the terain
![alt text][image2]

Description of the `process_image()` method :
1. Use color theshold to detect obstacles, rocks and navigable area. (line 11-13)
2. Convert there pictures into rover-centric and then into world coordinates. (line 16-18)
3. Plot obstacles, rocks and navigable area on worldmap, in red, blue, green seperately. (line 24-37)

### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.

The steps have ben resoved, you will find the code in the ./code directory

The following results have been obtained with the following simulator settings:
- Screen resolution - 1152 x 864
- Graphics Quality - Good

The code states are set out in a decision tree in the process order. See the code  function `decision_step()`, lines 148 to 180 in the file `decision.py` for more details.
1. Samples rocks found then move to the sample and pick up if close.
2. Turn around if there is no way forward, the rover will stop and turn around.
3. Move forward, explores the map until one of the steps above is triggered.

#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

The rover only explores navigatable paths using the calculated mean angle of the perspective transformed path. This then feeds into the `decision_step()`, as outlined above.

The rover does not keep track of where it has already been or where it has yet to discover. In spite of this, the rover can still navigate the world map and explore 40% of the area unaided. See image below.

![alt text][image3]


