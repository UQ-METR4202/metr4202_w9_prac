---
marp: true
theme: uncover
style: |
    :root {
    --color-background: #FFFFFF;
    --color-foreground: #8B2781;
    --color-highlight: #8B2781;
    --color-dimmed: #888888;
    }
    h1 {
      color: #51247A
    }
    h2 {
      color: #8B2781
    }
    h3 {
      color: #220033
    }
    section {
      font-size: 30px;
    }
paginate: true

---

# METR4202
## Robotics & Automation
### Week 9: [PRA] Camera Setup
- XIMEA Camera Setup/Calibration + Aruco Detect

---

# Today's Practical
- We will be working with the XIMEA Cameras
0. Setting up the hardware (pretty easy)
1. Install XIMEA ROS Package
2. Calibrate Camera with ROS OpenCV Camera Calibration
3. Setup ArUco Tag Detection Library

---

# GitHub Link
https://github.com/UQ-METR4202/metr4202_ximea_ros
- Please clone this into the `src` folder of your workspace.

---

# Step 0: Hardware Setup
1. Take out your XIMEA cameras, and mount them onto your camera mount (or a tripod if not already assembled) with the 1/4"-20 screw.
2. Connect the Micro-USB 3.0 cable and screw it on.
3. Connect the USB-A end to the **USB 3.0** port (blue) on the RPi4.
4. Take note of your serial number, as you will be using it:
---



# Step 1: Installing XIMEA ROS
## Prerequisites
- Make sure you have followed the Raspberry Pi 4 setup beforehand and installed the XIMEA API.

You may need to install this as a dependency:
```console
sudo apt install ros-noetic-vision-msgs
```
Then run `catkin_make` or `catkin build` in your workspace.

---

## Testing the XIMEA Camera
### Disabling USB Memory Limits
- You need to run the following command after each boot to disable the USB memory limits
```console
echo 0 | sudo tee /sys/module/usbcore/parameters/usbfs_memory_mb
```
(Hint: You can also add this to your `~/.bash_aliases` file)

---

### Running the Camera Node
First, if you haven't got a master node running, start it up
```console
roscore
```
Then, you can run the demo.
```console
rosrun ximea_ros ximea_demo
```
If you wish, you can add this to a launch file.

---

# Viewing the Video
You can view video in ROS, by using `rqt_image_view`.
So you can run this node:
```console
rosrun rqt_image_view rqt_image_view
```
You should be able to select your image topic, and monitor the video in real-time.

---

# Step 2: Camera Calibration
- If it isn't installed you can install the camera calibration ROS package
```console
sudo apt install ros-noetic-camera-calibration
```
You can run the ximea_demo node, if it isn't already running.
```console
rosrun ximea_ros ximea_demo
```
- You need to run the camera calibration node, which will display a GUI
```
rosrun camera_calibration cameracalibrator.py --size 8x6 \
--square 0.025 image:=/ximea_cam/image_raw camera:=/ximea_cam
```

---

# Step 3: ArUco Tag Detection
- Finally, we can launch the aruco detection node:
```
roslaunch ximea_ros ximea_aruco.launch serial:=XXXXXXXX
```
(Insert your camera's serial number)