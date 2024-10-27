import cv2
import numpy as np
import pyrealsense2 as rs

# Set the color range for red (HSV parameters)
low_red = np.array([0, 0, 200])
up_red = np.array([20, 255, 255])

# Initialize the depth camera
pipeline = rs.pipeline()

# Configure the depth camera
config = rs.config()
config.enable_stream(rs.stream.depth, 640, 480, rs.format.z16, 30)
config.enable_stream(rs.stream.color, 640, 480, rs.format.bgr8, 30)

# Start the camera stream
pipeline.start(config)

try:
    while True:
        # Wait for the camera frame
        frames = pipeline.wait_for_frames()
        
        # Get the color frame and depth frame
        color_frame = frames.get_color_frame()
        depth_frame = frames.get_depth_frame()
        
        if not color_frame or not depth_frame:
            continue
        
        # Convert the color frame to a NumPy array
        color_image = np.asanyarray(color_frame.get_data())
        
        # Convert to HSV color space
        hsv = cv2.cvtColor(color_image, cv2.COLOR_BGR2HSV)
        
        # Apply mask filtering based on the red range
        mask = cv2.inRange(hsv, low_red, up_red)
        
        # Find contours to detect the light spot
        contours, _ = cv2.findContours(mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
        
        for contour in contours:
            # Filter out small contours to ignore noise
            area = cv2.contourArea(contour)
            # The laser pointer light spot is relatively small
            if area > 1: 
                # Get the minimum enclosing circle
                (x, y), radius = cv2.minEnclosingCircle(contour)
                cv2.circle(color_image, (int(x), int(y)), int(radius), (0, 255, 0), 2)  # Green indicates detected target light spot

        # Display the processed frame
        cv2.imshow('Laser Detection', color_image)
        
        # Press 'x' to exit the loop
        if cv2.waitKey(1) & 0xFF == ord('x'):
            break

finally:
    # Stop the camera stream
    pipeline.stop()
    cv2.destroyAllWindows()
