# SocialDistancingAI
Using python, deep learning and computer vision to monitor social distancing.


# How to install?
It’s advisable to [make a new virtual environment] for this project and install the dependencies. Following steps can be taken to download get started with the project

## Clone the repository
```
git clone https://github.com/aqeelanwar/SocialDistancingAI.git
```
## Install required packages
The provided requirements.txt file can be used to install all the required packages. Use the following command

```
cd SocialDistancingAI
pip install –r requirements.txt
```


## Run the project
```
cd SocialDistancingAI
python main.py --videopath "vid_short.mp4"
```

Running main.py will open a window of the first frame in the video. At this point the code expects the user to mark 6 points by clicking appropriate positions on the frame.

#### First 4 points:
The first 4 among the 6 required points are used to mark the Region of Interest (ROI) where you want to monitor. Moreover, the lines marked by these points should be parallel lines in real world as seen from above. For example these lines could be the curbs of the road.
These 4 points need to be provided in a pre-defined order which is following.

* __Point1 (bl)__: Bottom left
* __Point2 (br)__: Bottom right
* __Point3 (tl)__: Top left
* __Point4 (tr)__: Top right



#### Last 2 points:
The last two points are used to mark two points 6 feet apart in the region of interest. For example this could be a person's height (easier to mark on the frame)

##### How does it work?
Detecting distances between pedestrian from monocular images without any extra information is not possible. One way (not very accurate though) is to ask user for specific inputs leading to a distance estimation between the pedestrians. If the user could mark two points on the frame that are 6 feet apart, using extrapolation, one could find the distance between different points on the frame. This would have been true if the camera was equidistant to all the points on the plane where the pedestrians were walking. The closer the pedestrians are to the camera the bigger they are. The closer the two points (which are same number of pixels apart ) on the frame to the camera, the smaller is the actual distance between them.
To cope this issue, the code receives 4 points input from the user to mark two lines that are parallel seen from the above. The region marked by these 4 points are considered ROI (can be seen in yellow in the gif above). This polygon shaped ROI is then warped into a rectangle which becomes the bird eye view. This bird eye view then has the property of points (which are same number of pixels apart) being equidistant no matter where they are. All it needs is a multiplier that maps the distance between two points in pixels to distance in real life units (such as feet or meters). This is where the last two user input points come in play.
Deep Learning is used to detect and localize the pedestrians which are then mapped to a bird eye view projection of the camera as explained above. Once we have the coordinates of the pedestrians in the bird eye view the social distancing parameters become straight forward.
The complete block diagram of the algorithm can be seen below.

###### Suggested improvements
The code could use a lot of improvements in a number of area. A few are suggested below
A more accurate approach of mapping the camera frame to bird eye view
The code uses an existing multi-class classifier that was trained on 1000 classes from COCO dataset. The code could use a re-trained classifier specifically trained for binary class (pedestrian, no-pedestrian).
More accurate way of calculating social distance parameters can be introduced.

Summary:

The algorithm can be used to analyze social distancing in a public area and carry out necessary actions to better deal with the pandemic. Automating the task will lead in effective actions taken in short time hence equipping us better to deal with the situation. The complete code is available here at GitHub. Feel free to modify and improve it.

