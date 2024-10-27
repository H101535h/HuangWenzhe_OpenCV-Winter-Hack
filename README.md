# HuangWenzhe_OpenCV_WinterHack
This is a program that uses OpenCV and RealSense cameras to detect light spots emitted by laser pens. This program analyzes the position of laser points through real-time video streaming and marks the detected light points on the image.
## Function
Real time laser spot detection: using HSV color space to filter red light spots and accurately identify the light spot of the laser pen.

Image processing: Use OpenCV for image processing, including color conversion and contour detection.

Visualization: Draw a green circle on the detected light spot for easy observation.
## Technology Stack
Python
OpenCV
NumPy
RealSense SDK
